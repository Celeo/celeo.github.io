---
title: Making developers productive
date: 2021-07-16 20:15:37
tags:
- programming
---

Thoughts on how to (and not to) make developers productive in the workplace.

<!-- more -->

Over my (still relatively short) career in the programming / software development / software engineering industry, I've encountered many environments, processes, and setups. Some of these were great boons to developer productivity, and others weren't. Without going into so much detail that I'd be pointing fingers, I'll attempt to highlight the high and low points of environments I've worked in that directly influenced productivity. I'm breaking "environment" into 3 segments: tooling, tasks, and timing.

## Tooling

Tooling is the hardware (workstations, peripherals) and software (programs, websites, command-line tools, IDEs, editors, etc.) that people use to write software.

Hardware usually changes far less frequently as compared to software, likely both due to the cost and effort involved in changing hardware. Developers are usually assigned a workstation (be it desktop or laptop) on their first or first few days of employment, and this remains in their possession and use until either a scheduled upgrade, or the device malfunctions to the point of needing replacing. For the corporations I've worked for, I've used a variety of computer brands, models, and setups, both desktop and laptop. I dramatically prefer the laptop model, despite it most often being an Apple laptop (which, though they sport an excellent size and hardware, I don't much like the OS) as it gives the freedom to work from anywhere, be it a desk, a conference room, outside, or from home, without hassle. All in all, I've had very few issues with hardware in and out of the office.

Software on those workstations, build platforms, and source control platforms can be a huge boon to productivity, or completely crush it by tanking morale. A "proper" setup that _empowers_ the developer (or for that matter, any employee) to do their job is a key factor of a workforce's ability to complete their work. Software underlines a huge part of a developer's job, as they're constantly interacting with it in one facet or another. A "proper" setup is one that enables a developer to turn their knowledge, experience, and creativity into correct, accurate, and legible code that furthers the corporation's goals in the industry. Lack of such a setup will constantly and dramatically hamper employees' ability to complete tasks.

> When developers don't have to fight the tools to get their jobs done, they'll be able to actually do their jobs.

Generally, companies and organizations will each align on the _baseline_ toolset to be used in the workplace. I'd group the both the source control and build platforms in this baseline, as they are often globally used and complicated. Developers are used to many of these: for source control platforms, GitHub and GitLab are likely the most popular followed by Bitbucket and the cloud-providers, with Perforce and SVN fading out, and FOSS-developed tools like Gitea and Gogs rounding out the selection. I vastly prefer git over other such tools, and as most of the git-supporting platforms share similar feature sets, switching between them isn't particularly difficult or cumbersome.

Next in line comes the product-specific software selections, like programming language, framework, main libraries, and supporting programs like databases, caches, and servers. Usually, once selected, these choices are permanent for the lifetime of the product, pending large ground-up rewrites to redefine the structure, flexibility, velocity, or feature-set. Individual developer style can show between these pillars, but even that is sometimes brushed away through tools like linters and formatters that seek to make all code in a project look and feel the same despite having different logic to complete tasks. I find this code homogeneity helpful to lowering the mental overhead of diving through code, but many don't care for the removal of the individual from a codebase. Thankfully, many of these tools are highly configurable, so individual development teams can opt for whatever works best for them. Hopefully.

Finally, we get to the software that is often the individual employee's choice: IDE, terminal setup, and other workflow-supporting tools. While most teams (should) sport onboarding documentation that minimizes the amount of time it takes to get started with a codebase, individualism plays out in this arena most strongly, as many developers will garner their preferred "working environment" for how they like to visualize and change software. Above all of the other choices, as this choice plays out most at the individual level without requiring buy-in from others, this is the area where there are the most options available to developers, both for within companies and on their solo endeavors. Developers can and do select their IDEs for speed, freedom, conformity and nonconformity, and above all, personal taste. From minimal-appearing editors like Sublime Text and vanilla vim, to "full" editors like VS Code and Atom, to all-in-one IDEs like IntelliJ and Visual Studio, developers can select how much they want their editor to constantly give them feedback or stand to the side to allow the developer to drive the process. Each of these editors, especially the non-all-in-one options, sport a vast bounty of configuration options that can both inspire and overwhelm users. I use IntelliJ to run Java applications as it is commonplace in the enterprise environment, but VS Code for everything else. I'm currently writing this article in Markdown in VS Code with several community-developed extensions to check my spelling and conformity to Markdown best practices.

I encourage all developers to spent a few hours diving deep into their editor or IDE of choice and its configuration menus and extensions/plugins, as there are so many incredible options that can lessen or remove frustration, lower cognitive overhead, minimize distractions, improve organization, and more, allowing the developer to do what they're paid to do, write code, without having to worry about much else. This is an area I'm very passionate about, and frequently read articles and watch videos from other people sharing their setups to see if there's anything I can adopt for my workflows both at work and at home. Allowing developers to choose, and also _configure_ their tooling at this level is a huge boon to productivity directly and also through providing large boosts to morale and comfort.

## Tasks

Developers are not unique in that they must get things done to warrant their continued employment, and that shouldn't be news to anyone. Software development is often a high-paid field, and employers expect to receive a corresponding amount of revenue through improvements to their products. How tasks are created, monitored, updated, and completed is a huge part of productivity. When tasks are self-contained, explained, clear, understandable, and achievable, developers are empowered to take them on without having to think about the cost of doing so. A task that requires changing some component of the product in a specific way to complete some specific outcome has the makings of a great task, as it contains most everything that a developer and hopefully a Quality Assurance teammate will need to complete it. Tasks that don't have some or any of these create a barrier to entry for starting on the work, as the developer, before even getting to think about how they may go about completing the task, have to talk to other employees often outside their immediate team who have their own schedules and deliverables. This means emails, chat messages, and meetings. This means paperwork. This means **time** - time that isn't spend doing work. Here, a strong argument can be made that this _is_ work, that the idea of "work" absolutely contains the effort of defining and understanding what needs to be done as well as actually completing that, and in general I agree. For _this article_ however, I'm separating them into "work", actually completing tasks and changing code, and "pre-work" and "post-work" which are the other auxiliary efforts that are in service of getting things done.

Any hurdle to completing a task is a drain to productivity for the individual, team, product, and eventually the company. That statement can certainly be viewed as hyperbolic, as sometimes getting clarification on the requirements on a task is as simple as a 30 second back-and-forth over text chat, but as it's most often significantly more than that, it adds up. A 30 second text chat is both quick and largely non-intrusive, but a 30 minute meeting with 5 people requires dramatically more effort from all involved, as that meeting requires the physical or virtual presence and focus of all participants, getting around often-complicated schedules and meeting hours, and face to face (or ~ish) communication. While I prefer to have some meetings "in person" to go over more complicated product deliverables and goals, having to set up something like this for every or many tasks that could otherwise go without will quickly drain all involved, especially developers, and cement the workflow hurdles of starting new bodies of work.

Can this meeting-driven task creation work for some teams? Sure, I imagine so. Does it work for all teams? Absolutely not.

> When developers don't have to undertake a lot of auxillary effort to begin a task, they'll be more active in starting new work.

## Timing

Timing is such a complicated, non-specific, and ever-evolving subject that it's hard to say what works rather than just listing off everything that doesn't.

## Summary

TODO

- all work together
- an issue in 1 will bleed over to the others
