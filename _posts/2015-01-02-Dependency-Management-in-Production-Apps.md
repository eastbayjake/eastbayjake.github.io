---
layout: post
title: Dependency Management in Production Apps
date: January 2, 2015
---

I'm loving [The Twelve-Factor App](http://12factor.net) guide to best practices for web applications. Its prose is concise and readable, avoiding the technical density that limits the audience for many excellent technical write-ups. I appreciate that the authors remained language-agnostic, choosing examples from several languages to avoid coming off as merely "Best Practices for [LANGUAGE] Web Development". It aimed for a wide audience and it deserves to be widely read.

But I disagree with some of their best practices in *[Dependencies](http://12factor.net/dependencies)*. The authors advise:

> A twelve-factor app never relies on implicit existence of system-wide packages. It declares all dependencies, completely and exactly, via a dependency declaration manifest. ... One benefit of explicit dependency declaration is that it simplifies setup for developers new to the app. The new developer can check out the app’s codebase onto their development machine, requiring only the language runtime and dependency manager installed as prerequisites. They will be able to set up everything needed to run the app’s code with a deterministic build command.

I'm a proponent of checking-in dependencies with version control. The JavaScript community seems split between back-end and front-end developers: NPM [recommends](http://stackoverflow.com/a/19416403/2596740) using npm with `npm shrinkwrap` to resolve all dependencies, while the Bower team [recommends](http://addyosmani.com/blog/checking-in-front-end-dependencies/) checking-in dependencies for production apps. I have been following Mikeal Rogers' [advice](http://www.futurealoof.com/posts/nodemodules-in-git.html) about checking in dependencies. In fact, even though NPM has changed to support using `npm shrinkwrap`, I still think about their old FAQ about this issue when explaining my preferences to others:

> Check node_modules into git for things you deploy, such as websites and apps. Do not check node_modules into git for libraries and modules intended to be reused. Use npm to manage dependencies in your dev environment, but not in your deployment scripts.

For the skeptical purists, I have three arguments for checking-in dependencies:

#### It continues working even if the Git remote or package manager is down ####

I may feel this one more acutely as a JavaScript developer, but I've been stung too many times by running `npm install` only to find NPM is down. I can't do anything but take a walk until it's back up. It's bad enough that even NPM's co-founder has nightmares about it:

![]({{ site.url }}/images/npm-nightmare-tweet.png)

If you check-in your dependencies with your codebase, all you have to do is `git clone` the repo and you're set locally. It has all the same benefits outlined above for using a package manager: easy setup for a new developer with a single command.

#### Using a package manager isn't faster ####

François Zaninotto did a [speed test](http://www.redotheweb.com/2013/09/12/should-you-commit-dependencies.html) for Node.js, PHP, and Ruby package managers and found that "committing dependencies is **between 5 and 12 times faster** than using a dependency manager." We're only talking about 60 seconds for a sample Node project so it's not a huge deal, but speed should not be an argument in favor of package managers.

#### Deploy an identical codebase in development and prodution ####

Let's suppose your project requires a specific version of a module, but that module has a wildcard dependency on another module. What happens if that sub-dependency gets updated and the latest version causes breaking changes in your main dependency that its author hadn't foreseen? When you go to `npm install` your dependencies, you'll get an updated version of the sub-dependency and a broken version of the main dependency.

Avoid that headache by installing the dependency and its sub-dependencies in development, develop and test, then commit all of those dependencies and pull down an identical version in production. As an engineer I want to minimize the number of descrepancies between my development and production environment, not introduce new complexity to save pennies on disk space.

Insisting on a dependency manifest seems needlessly DRY. ***Is our real bottleneck as engineers the disk space of our repository or the time to hunt down and fix a broken dependency when a new instance gets deployed?*** And if your goal is to minimize new developer learning curve, do you really want to throw the new guy into debugging an obscure dependency-requirement discrepancy?

(If you still want to use NPM for all dependencies, you can use `npm shrinkwrap` to [lock-in all sub-dependencies with the version used while you're developing](https://docs.npmjs.com/cli/shrinkwrap). I would like to use this a bit in my personal work -- I'm willing to admit my preferences may be behind the rest of the Node community!)