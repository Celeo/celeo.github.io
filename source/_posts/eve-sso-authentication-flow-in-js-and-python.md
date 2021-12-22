---
title: EVE SSO authentication flow in JS and Python
date: 2016-01-01 00:00:00
tags:
- programming
- old_blog
---

This past week, I wrote about the EVE SSO flow in [Python using Flask](https://celeodor.com/eve-sso-authentication-flow-in-python/). Here's how to do it with a separate frontend using [Vue.js](https://vuejs.org/).

<!-- more -->

Several months ago, I [wrote](https://celeodor.com/vue-js/) that I dove into the world of the dedicated JS frontend frameworks and decided on Vue.js. Since then, I've been putting more time into learning it. I find that it's easier to do things on "both sides" using [Flask](http://flask.pocoo.org/) and its bundled template rendering engine, [Jinja](http://jinja.pocoo.org/), but as I get more experience with Vue.js, the more I think about using it in projects instead of HTML from Flask.

As a challenge, I took the EVE SSO flow app in Flask and made another one to practice using Vue.js. The result can be found on [Github at Celeo/Vue-Flask-EVE-SSO](https://github.com/Celeo/Vue-Flask-EVE-SSO) (I know, what a name). I'll skip over most of the backend, as it's a cut-down version of the other app's backend. Instead, let's look more at the front and the communication between the two.

### Libraries

Vue.js, much like Flask, comes as a "minified" approach to its role: instead of including everything right "out of the box", it comes with what you need to get up and running and provides all the extra stuff for larger apps on the side. This results in a framework that is easy to get setup and working while still retaining the ability to make more complicated apps, like those that include routing.

For the frontend, to Vue.js I added [vue-router](https://github.com/vuejs/vue-router) for page routing, [vue-resource](https://github.com/vuejs/vue-resource) for communication with the backend, [URI.js](https://github.com/medialize/URI.js) for query string parsing, and [jsrsasign](https://github.com/kjur/jsrsasign) for JWT verification, in addition to the standard items in the items contained in the [webpack-simple Vue project template](https://github.com/vuejs-templates/webpack-simple/tree/1.0).

For the backend, to Flask I added [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/) to enable [cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), [Preston](https://github.com/Celeo/Preston) to handle the EVE SSO and CREST authentication, and [PyWJT](https://github.com/jpadilla/pyjwt) to create and sign JWTs.

### Flow

There are [2](https://github.com/Celeo/Vue-Flask-EVE-SSO/tree/f199a78156b0eeed1705bf81b6b435861b1785de/front/src) different [`.vue`](https://github.com/vuejs/vue-loader) files in the project, meaning there are two different pages that the user can interact with. The first page that the user will likely land on, coming into the app, is the `App` page. This page is restricted to logged-in users only, as can be seen in [`main.js`](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/front/src/main.js):

```javascript
router.beforeEach(function(transition) {
  if (!transition.to.path.startsWith('/login')) {
    let name = localStorage.getItem('character_name')
    if (!name || name == undefined) {
      transition.redirect('/login')
      return
    }
  }
  transition.next()
})
```

This makes use of vue-router's [`router.beforeEach`](https://router.vuejs.org/en/api/before-each.html) hook. Whenever the user navigates to a page in the app, their destination is checked. If they aren't going to the login page and they're not logged in (a key in `localStorage`), then they're redirected to the login page. If they are logged in, then their destination is not modified and the page they requested will load. The `App` page isn't much to look at, as the work is all in the `Login` page.

Taking [a look](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/front/src/Login.vue) at the code, the template is broken into two sections: `#start` and `#back`, as identified by the two high-level `div`s' ids. The `start` div is shown when the variable `back` is false, which it [defaults to](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/front/src/Login.vue#L26). The other div, `#back`, is shown when `back` is `true`, set by the `created` hook's parsing of the URL.

When the user has landed on this page (usually from being redirected to it after attempting to load the root URL), their URL does not have a query string, so the bulk of the login in the `created` hook is skipped, jumping to the [three line](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/front/src/Login.vue#L72-L74) logic in the `else`:

```javascript
this.$http.get('http://localhost:5000/').then((response) => {
  this.url = response.data.url
})
```

This is the first call to the backend. A simple, unadorned GET, it requests the URL to redirect the client to in order to start the EVE SSO process. That URL, like in the Flask-only app, is generated by Preston. The bulk of the login here is for that process, so we'll scroll back up to that.

### JWTs

> JSON web tokens are an open, industry standard RFC 7519 method for representing claims securely between two parties.

\- [jwt.io/](https://jwt.io/)

This app uses JWTs to send the user's authenticated character name from the backend to the frontend. Why? Well, in the apps that I've written with the EVE SSO, the character's name is the key to permissions, so all components of the website need to be able to trust it. The backend gets its from CREST, and then hands it off to the frontend.

Also, I wanted to learn how to use JWTs.

The Python code to [generate the JWT](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/back/app.py#L46) is very simple:

```python
return jwt.encode({'character_name': character_name}, app.config['SECRET_KEY'], algorithm='HS256')
```

Note that this token is **signed**, not **encrypted** - someone could catch the token in transit and look at the contents. In this use case, the contents don't matter, the fact that they're from the backend is the important piece.

The code to verify the token and get the contents are [two separate calls](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/front/src/Login.vue#L44-L46) to the jsrsasign library:

```javascript
let isValid = rs.jws.JWS.verifyJWT(token, config['secret_key'], {alg: ['HS256']})
if (isValid) {
  let payloadObj = rs.jws.JWS.readSafeJSONString(rs.b64utoutf8(token.split(".")[1]))
```

After this, it's just getting the name from the token's payload, and storing it in `localStorage` and the flow is complete. In a user-facing app, I'd store the character name in a [store](https://github.com/vuejs/vuex) and drop the entire JWT into the `localStorage` so the app could load the name from the verifiable storage into the store on init. This way, changes to the logged-in state could be seen across components.

### Pitfalls

First off, it took me a good couple of minutes to understand that the tokens aren't encrypted (though they can be), but instead signed. This turned out to be fine for what I wanted to do, just be aware of the difference.

Next, getting jsrsasign to build in webpack was a hassle. I'm not a webpack expert; I just use the setup provided by the Vue project template. I found a solution on a [Github issue for the Pug engine](https://github.com/pugjs/pug-loader/issues/8#issuecomment-55568520): add `node: {fs: "empty"}` to the [webpack configuration](https://github.com/Celeo/Vue-Flask-EVE-SSO/blob/f199a78156b0eeed1705bf81b6b435861b1785de/front/webpack.config.js#L48-L50).

---

**Update**

After working with tokens more, I realized that I didn't really need to perform full verification on the client of the token's signature. It doesn't matter if someone somehow feeds the client an invalid token or changes the stored data - as soon as the client makes a request to the server with that invalid token, the server will know and send a response back to the client denying them access. The client doesn't need to perform verification, it just needs to be able to read the token's contents. To this end, I now use [jwt-decode](https://github.com/auth0/jwt-decode) in the client to read the contents of the token.