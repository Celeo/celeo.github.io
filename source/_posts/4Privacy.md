---
title: 4Privacy
date: 2021-10-25 19:20:45
tags:
- programming
- privacy
---

The [SmarterEveryDay YouTube channel](https://www.youtube.com/channel/UC6107grRI4m0o2-emgoDnAA) launches a Kickstarter for "4Privacy", an "app \[that\] helps you privately communicate and securely organize your important information". Sounds like a good win for privacy, right?

<!-- more -->

TD;DR: we'll see.

Disclaimer: I am not a security professional.

## Premise

On October 21st, the SmarterEveryDay channel posted a video called ["Is Your Privacy An Illusion? (Taking on Big Tech) - Smarter Every Day 263"](https://www.youtube.com/watch?v=KMtrY6lbjcY).

### Main points raised, first video

- The first thing he talks about is the trust he's gained with his viewing audience over the many years of high quality and educational videos he's published to the platform
  - He wants to leverage this trust to talk about the subject of the video: digital privacy
- He leads into a metaphor, then into a history of digital communication
- He includes the "if you don't pay for the product, you are the product" moniker
- He addresses that digital information can and has been accessed by other parties, notably the government
  - The US 4th Amendment only covers physical storage media in someone's home
- Talks briefly about sending data to someone else and how that routes through a technology company's infrastructure
- Notes several examples of "Big Tech" cooperating with the US government for getting them access to data, and he mentioned that they don't have a choice due to warrant and subpoena powers (and gag orders on top)
- He raises an argument against the seemingly-common "I have nothing to hide" position, noting that hacks and leaks can still impact peoples' lives
  - He says that these are due to data "sitting unprotected on someone else's system"
- His summary of why this can happen is that we've "given up control" of our data
- Tie into the earlier metaphor
- "Is this the way it has to be?"
- He claims that we "still use the same kind of engine we started with" to run the Internet, and this is the reason for the "spewing our of your private data all over the Internet"
- Comparing combustion vehicle engines to EVs, he claims that "it's time to take the next big step for how we use the Internet. We need a new technology that's clean, that doesn't spew out your private information and pollute the environment. We need an engine built for privacy."
- He's been working with a group of developers for several years
  - States that there needs to be regulation and technology to product data
  - Technology
    - E2E encryption where uses "hold the keys"
    - Zero-knowledge architecture
      - Claims that this will protect against hacks, leaks, and government intervention
    - OSS
- Want to come up with a way to apply [Signal](https://signal.org/)'s pattern "to all the other digital stuff you do in your life"
  - "Imagine how cool it would be to have a very simple technology that could apply encryption to the everyday business of your digital life in a way that gives you the encryption keys. That's the magic of what we're trying to do. Said in a complex way, the most important thing we're doing is encryption key handling in a very simple, transparent way for the user."
- Kickstarter plug
- The first introduction to the product - an app that's being used to spearhead development on their platform
  - From the snippet of video we get from the app, it shows a encrypted file vault with the ability to invite people to it
- Call to action: "we're a small team, and we're looking for people to partner with to make this thing work."
- Calls the app a proof of concept
- Says that funding the Kickstarter campaign will "send a message that privacy is important and things need to change"
- More videos to come
- The only link in the description is to the Kickstarter

### Main points raised, second video

Unlisted companion video [here](https://www.youtube.com/watch?v=Hy6STq337qo)

- Claims that many companies don't do actual E2E encryption because they hold the encryption keys
  - This new platform wants to give the keys to the users only
- Claims that their platform will make use of temporary viewing and encryption such that users can send data to someone else but retain ownership of that data, such that other parties cannot store copies of it
- Claims that the future OSS nature of their code means that anyone can audit it for trustworthiness

### My notes

- Yes, people should be at least interested in the production, storage, and usage of their data
- File encryption has existed for a long time and is largely supported by a wide variety of platforms
- HTTPS (via SSL and TLS) has similarly existed for a long time
- If you want to look up a summary of a product's Terms of Service, try <https://tosdr.org/>
- Protecting your data is a dramatically different task depending on who you want to product it _from_ (hacker, ISP, company, Big Tech, government)
- The do have a GitHub Org, but there's no code
  - Only one [repo](https://github.com/4PrivacyEngine/4PrivacyEngine-Core) with a README that broadly details the open source effort
    - Won't be any time soon
- Auditing code is nice, but if they're running "a platform", then there's no way to tell what they're running on their servers
  - That's still a company that people have to put their trust in
    - A company that's making money
- This idea of "temporary-lended, still-owned, volatile, temporarily-decrypted data" isn't how computers work
- The comments on the unlisted video are largely in disagreement with the feasibility of the project's premise
  - The comments on the [reddit thread](https://www.reddit.com/r/SmarterEveryDay/comments/qcu3b1/is_your_privacy_an_illusion_taking_on_big_tech/) are similar
- The idea of having such a huge YouTube channel sell their Kickstarter has been wildly successful: they've raised over half a million USD in pledges in 4 days from over 10 thousand people
- Their Kickstarter page links to their [site](https://4privacy.com/)
- I originally pledged $25 but later cancelled

## When video education and advertising cross paths

<https://youtu.be/CM0aohBfUTc>

- Veritasium hawking self-driving cars
- Veritasium hawking genome sequencing
- Physics Girl hawking Toyota Hydrogen-powered cars
- SmarterEveryDay co-founding 4Privacy
- Many, _many_ channels being sponsored by VPN companies

## Getting people interested in privacy

Is a good thing. People who are interested will find a world of resources, both FOSS and paid, available to them to progressively migrate themselves and (some of) their data away from common mediums. Take, for example, <https://privacyguides.org/> (and [/r/PrivacyGuides](https://www.reddit.com/r/PrivacyGuides/)) and various reddit communities, including <https://reddit.com/r/selfhosted/>. Nothing that the 4Privacy team are claiming to do is new (nothing, that is, that is actually technologically possible).

## Project rebuttals

In addition to the videos and reddit thread linked about, see also:

- "4privacy nonsense exposed" <https://www.youtube.com/watch?v=BM-4l5R4P4I>
- "How SmarterEveryDay's 4privacy can, and cannot, meet its goals" <https://drewdevault.com/2021/10/22/Smarter-every-day-and-4privacy.html> ([HN](https://news.ycombinator.com/item?id=28973636))

## History of the project

The videos note that the project has been in the works for several years. Apparently another project with a similar goal called "Lockdown" [launched in summer 2019](https://www.macrumors.com/2019/07/24/lockdown-firewall-app-privacy-protection/). The team of that app, [a reddit user noted](https://www.reddit.com/r/SmarterEveryDay/comments/qcu3b1/is_your_privacy_an_illusion_taking_on_big_tech/hhjblfy/), is the same team working on 4Privacy.

## Closing thoughts

I'm glad that digital privacy is getting more attention, and if the 4Privacy team seeks to do good with their "platform" (and app?), then I hope it goes well for them. Certainly, I hope they're not just trying to capitalize on the many buzzwords that the launch video contains.
