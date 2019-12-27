---
title: The APIs of Eve Online
date: 2019-12-26 19:02:34
tags:
- games
- eve
---

[Eve Online's](https://www.eveonline.com) APIs and how they got me interested in SQL, Python, webdev, yaml, [Linux](https://en.wikipedia.org/wiki/Ubuntu_version_history#1204), [Nginx](https://www.nginx.com), [DigitalOcean](https://www.digitalocean.com), and more.

<!-- more -->

## Outline

* Getting into EVE
* Finding a problem
* Getting into EVE's APIs
* The APIs and what they taught me
    * XMLAPI
    * SDE
    * CREST
    * ESI
* Conclusion

## Getting into EVE

I first got into Eve back in October of 2011. I've always liked space and spaceships, maybe that's what got me interested? Maybe it was other people talking about how much fun they were having? Maybe it was one of [EVE's huge thousand+ player battles](https://duckduckgo.com/?q=eve+online+battle)? I think I had heard of it from reddit, but it's so long ago now that I really can't pinpoint what got me to pay for (it was subscription-only back then) and play the game.

Regardless of how I stumbled across EVE, I played it off and on for a year before getting into [wormholes](https://wiki.eveuniversity.org/Wormholes). Without spending hours explaining what that entails, basically I had gotten interested in a part of the game that was built on randomness - who you were near, who was near you, and what was going on all fluctuated on a day-to-day, hour-to-hour basis. You never knew what was going to be happening in a few hours. In September of 2012, just shy of a year since I started playing, I entered a corporation (a guild, effectively) called _Wormbro_, a corporation that was designed to help players new to the game or new to wormholes to get their "space legs" and get used to the gameplay. It's there that I found a community to really be a part of, something I hadn't had for years (since helping run a Minecraft server for several years, which is where I picked up Java).

## Finding a problem

A big part of "wormholes" was tracking them - if players didn't explore them and record where they led, then other players would have no way of knowing what was going on (wormholes appeared and disappeared (mostly) randomly). We recorded this information in an in-game chat channel _message of the day_ (effectively a notepad that anyone in the corporation could see but only a few could edit). It is in this "notepad" that we kept track of everything that was going on. As our experience and exploration increased though, we ran into a problem - this "notepad" had a character limit.

Although many people these days are used to character limits (Twitter basically having been built on one), the character limit in these "notepads" did _not_ have a good user experience: when the editor would go over the limit, there was no visual indicator that they were over. When the editor saved, the game would simply trim the content size down to the limit, and save that; whatever was over the limit was lost. As tracking this information was critical to players' (in-game) safety, this invisible deletion of hard-earned information was quite debilitating.

The leaders of the corporation searched for alternatives, but came up short, as the requirements for what we needed were pretty strict: must be visible by a group of people, but editable by a subset of that group; must have a higher character limit than the in-game one; must allow coloring and formatting. There were a few options that provided everything _except_ the authentication & authorization, but as those two feature were the most important, no alternative was found.

## Getting into EVE's APIs

As a budding programmer, actively searching for some place to delve into and continue writing code for other people, I figured that it'd be a good time to start learning web development. Back when I coded for Minecraft servers, a fellow programmer had suggested that I get into Python, as it was their favorite language, certainly more than Java. A quick web searched showed that Python had several active frameworks for building web servers, so I picked the biggest - [Django](https://www.djangoproject.com) - and got to work.

This first app didn't really require anything specific from EVE other than the authentication & authorization; everything else was generic enough. Instead of representing the data in a text form, I'd store everything as individual rows in an SQLite DB and retrieve data to present to the user when they loaded up the web page. This allowed for many more features, including edit history, which eliminated the need for editing to be restricted to a trusted group.

At the time, the simplest way for a website to tell if a user was a player in the game was through HTTP headers. EVE's in-game browser would send [a set of headers](https://wiki.eveuniversity.org/In_Game_Browser_Development#Request_Headers) to any website that the player navigated to that had previously added the domain to their "trusted sites" list. The simplest use of these headers was to get the player's name, corporation, and alliance ("guild of guilds"). These data could be checked against a configuration to see if the player had access to special information. For my site, these was used to either allow the user access to the site, or block them completely.

> **Side note**: readers may have spotted a problem with this authentication approach in how I've described it and by reading the eveuni link - _there was no authentication_! Headers could simply be sent with other player's names or information.

Once I had a working version of the site and had shared with the alliance leadership, everyone started editing the in-game "notepad" _and_ my site to keep track of the information. My site was good at storing any amount of data, but the "notepad" was better at providing information at a glance.

> **Side note**: if your users are still using their old way, then your solution isn't good enough to replace the old way (or the users are just very stubborn). My solution suffered from the former.

From there, I branched out. I started using the [XMLAPI](https://github.com/ccpgames/eveonline-third-party-documentation/blob/c0ff7ec8799b1d25847ac73cbb1af017e2ef72c9/docs/xmlapi/index.md) to get more information on characters based on access they granted me (technically the alliance, but effectively me). I started using the [static data export](https://github.com/ccpgames/eveonline-third-party-documentation/tree/c0ff7ec8799b1d25847ac73cbb1af017e2ef72c9/docs/sde/index.md) to get information on the in-game universe and its non-player inhabitants. Eventually, when the XMLAPI was deprecated in favor of the new JSON-based [CREST](https://github.com/ccpgames/eveonline-third-party-documentation/tree/c0ff7ec8799b1d25847ac73cbb1af017e2ef72c9/docs/crest/index.md) API, I switched over to that (but only sort of: CREST was pretty much only a failure or a proving ground for what came after, depending on your viewpoint). Finally, when the [ESI](https://github.com/esi/esi-docs) came out, I switch over (mostly) to using that.

### The APIs and what they taught me

As each of EVE's APIs were different in both how they were interacted with and with what they each provided, each one taught me different skills, most of which I still use today, both in my personal projects and also my professional work.

## IGB

The "in-game browser" wasn't an API, but I want to call it out here because of what I learned, driven and encouraged by what I could do with the IGB. The IGB didn't really enable me to do anything that I otherwise could not have, but it was a catalyst for motivating me to jump head-first into an area of the software field that I hadn't explored: web development.

Things I learned (at the time):

* Python
* Django
* Flask (later)
* HTML & CSS
* Ubuntu
* VPSs through DigitalOcean
* Nginx
* Supervisord
* Vim
* A bunch more Unix stuff

## XMLAPI

The XML API was the first EVE API that I dealt with. Other players would generate an API key, selecting which "permissions" they wanted to include in that key, and then give it to our alliance. I'd use that key to build an HTTP request that was sent to EVE's API. Assuming the key was valid for the endpoint I requested, I'd get back data on the character or corporation in a big block of XML. Documentation existed, but there was a lot of trial and error required in order to become familiar with the API.

Things I learned:

* Sending HTTP requests, including headers and querystrings
* Parsing XML
* Trying to convince people that there data was safe with me

## SDE

The SDE ("static data export") was an SQL dump of a ton of information about the game that didn't change, like starsystems, information on stars, links between systems, item information, crafting information, etc. The XMLAPI provided information that _could_ change, while CCP (the makers of the game) requested that developers make use of the SDE whenever possible to reduce traffic to the XMLAPI.

Things I learned:

* Not all SQL dumps are the same
* MySQL dumps are not compatible with SQLite
* Trying to set up a LAMP stack just to pull data out of a MySQL dump was too painful to do, even though I tried multiple times
* Manually typing commands into the SQLite prompt to load data from CSV into a new DB was very error-prone
* The developer who runs www.fuzzwork.co.uk was a hero - they provide data conversions of the official export for easier use by other developers

## CREST

Like I wrote above, CREST was either a failure or a proving ground, depending on who you asked. For me, it was the former, right up until CCP announced ESI, which turned CREST into ... well, both a failure and a proving ground. Either way, it was definitely a step in the right direction. It was intended to be a discovery-based JSON REST API which would replace the XMLAPI.

Things I learned:

* XML is awful
* JSON is great
* Discovery-based API layouts are NOT substitutes for documentation

## ESI

Finally, CCP announced ESI - the "EVE Swagger Interface". No longer would documentation have to be maintained by the community! No longer would we have to wonder at the stars to know what data we could get from the API! No longer would we hav-------- Well, that was the intention.

ESI started rough: it was slow (both the Swagger site and the API itself), the endpoint documentation in Swagger wasn't very helpful, it didn't provide any interesting new data, and the data it _did_ provide was already covered by either the XMLAPI or CREST (and sometimes both). I remember seeing it, talking about it with a few other developers I had met in-game, and largely ignoring it for several years. Unfortunately, by the time that it lived up to everything that was promised, I'd taken a step away from the game.

Things I learned:

* Swagger is great
* Swagger is not _automatically_ a replacement for documentation
* Swagger can help developers explore your API to help adoption
* Proving a new and shiny alternative to an existing API, deprecating the old one, and asking people to migrate _won't_ actually get them to migrate until the new API is better than the now-deprecated one

## Conclusion

I owe a great deal of my knowledge today to what I learned during my time playing [internet spaceships](https://www.eveonline.com/article/internet-spaceship-crashes-are-serious-business). The EVE APIs allowed me to access information about **something I was interested in**, which helped me push forward whenever I got stuck. There was always another endpoint I hadn't explored yet, always another SQL table to examine, always more documentation to read and contribute to. As the in-game universe of EVE was always changing, so too was the information I could query for. As I was actively playing the game, the **data that I got back was meaningful** to me, both as a developer and as a player. Finally, the years I spent writing websites for my alliance actually **helped the users**, and I was one of them. All these contributed to a time of unparalleled learning in my journey to become a professional software developer.
