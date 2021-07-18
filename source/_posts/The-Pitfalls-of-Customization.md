---
title: The Pitfalls of Customization
date: 2021-07-17 16:40:12
tags:
- programming
---

Creating custom libraries, frameworks, tools, and products can bring great value in the workplace but putting time, effort, and money into these customizations can bite back.

<!-- more -->

When companies want something, there are several options available to them.

## Buy

First, they can purchase. If a product already exists in the market, is an acceptable price, has a set of features that the company wants, and is of the correct licensing, payment, usage, etc. models, then it's likely the best option given often-bundled support and training. The advantages to this are many, and build up to this being the fastest option both in terms of delivering the product and also in putting it to use. The chief disadvantage of this option is that it's often the most expensive.

## Use

The second option is using something from the FOSS world. GitHub and GitLab host thousands of high-quality, actively-developed, and production-ready projects. Many of these projects are free (as in freedom) in addition to free (as in beer) to use. Some require commercial licenses, and some offer direct support for optional commercial licenses. Here, the company still gets the benefit of high-quality software and doesn't have to pay as much, but loses some or all of the corporate-to-corporate environment, as support and training will likely be the same for them as it is for anyone stumbling upon the project while poking through reddit. Of course, the biggest advantage to open source is the ability to extend and change what's there, but that requires an in-house or hired development team. While not as fast as purchasing a corporate-focused product, this can still be up and running fairly quickly.

## Create

Finally, there's the option to create something yourself. A company could assign this task to an internal development team or contract the work out. This could be starting from scratch, building on top of an existing internal product, or using a FOSS project as a base. This option requires a development team, which requires hiring, training, paying, managing, etc. If the company is already a large tech company, finding or putting together a team to take on this work might not be a difficult problem. Funding the endeavor might not be an issue. Putting aside the time to get it to a working state, regardless of the starting point, may be a cost that the company can shoulder. This is the slowest option, but the tradeoff is for complete control over what the final product looks like, how it works, and what its capabilities are. The initial cost for this undertaking for a large company might be next to zero - just reassign a handful of engineers. The actual cost is what the company will experience in the long term:

- employees working on this project still have to be **paid**
- employees working on this project **aren't working on anything else**
- anyone brought onto the project, whether internal or external, will have **no experience** with working on it
- it will be **brand new** to all users
- all **support** for the project will have to be internal
- all **training** for the project will have to be internal
- all **auditing** for the project will have to be internal
- all **maintenance** for the project will have to be internal

Every one of these items costs time, money, or both. This is the slowest option. Depending on the project, this is the most expensive option. This is, however, the most _flexible_ option. For companies that are large enough to handle the costs of these and are planning out for the future, this has a lot of valuable prospects.

### Additional technical costs

The above bulleted points apply both to _products_ and to less concrete technical _projects_ like languages and frameworks. For these, the last 4 costs are compounded. Not only will no employee have any experience with it, no _engineer_ will have experience with it. **Candidates cannot be hired with any previous experience**. Additionally, working on the project will grew far fewer skills in employees that translate to their efforts on other projects in that company or further in their career. That may sound like a non-issue for the company currently paying them, but it's a key factor in what potential applicants look for when applying for jobs.

## Summary

The option a company takes when needing a solution differ based on many different financial, workforce, and specificity requirements. I've seen groups both too willing and too hesitant to opt for each of these options.
