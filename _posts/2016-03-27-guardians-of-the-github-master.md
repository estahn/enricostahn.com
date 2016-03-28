---
layout: post
subtitle: null
date: "2016-03-27"
published: true
title: Guardians of the GitHub master
---

**TL;DR**: Use GitHub protected branches with required status checks.

GitHub provides a feature called *protected branches* to disallow direct commits into certain branches. On top you can define status checks that need to pass before you can merge anything into the protected branch.

## Why is this important?

It will provide a safety net for your project. I recently suggested a feature in one of the open source projects we’re using. The project maintainer liked the idea and it was fairly simple to implement. He implemented the feature, committed the change and released a new version. Great, but the version was not working. Looking at the CI server made clear that existing tests failed. You could blame the project maintainer for not running the tests, not waiting for the CI server to finish the build or not checking the build results. However, this wouldn’t have changed the end-result or avoid such mistakes in the future. With hindsight, the delivery process was clearly too manual, one might say broken.

## Block all the things & automate everything!

The first step to fix the process is to enable the branch protection for master (or whatever branch you use as main branch).
![GitHub Protected Branches]({{site.baseurl}}/img/github-protected-branches-config.png)

This will permit anyone to commit directly to master. Instead they will need to create a new branch and a pull request. The pull request can then be reviewed, either manually or automatically, preferably both ([Giving better code reviews — Medium](https://getpocket.com/a/read/1173023422)).

## Guardians -- Check 1, Check 2, Check 3, ...

The next step is to decide what checks are required to complete successfully before a pull request can be merged to your main branch.
![github-protected-branches-checks.png]({{site.baseurl}}/img/github-protected-branches-checks.png)

Pull Requests are a great tool, they can provide an overview on the impact of introduced changes. At [Zanui](https://www.zanui.com.au/) we're using them for:

* Code Reviews
* Code Coverage Overview (with [Codecov Chrome Extension](https://chrome.google.com/webstore/detail/codecov-extension/keefkhehidemnokodkdkejapdgfjmijf))
* Status checks, such as
  * Coding Style Checks (automatically fixed via [StyleCI](http://styleci.io))
  * Coding Standard Checks (via [Codacy](https://codacy.com/), [Scrutinizer](https://scrutinizer-ci.com) or [SensioLabs Insights](https://insight.sensiolabs.com))
  * Unit and Integration tests (via [CircleCI](https://circleci.com))
  * Code Coverage minimum requirements (via [Codecov](https://codecov.io))

While this looks like a lot of effort, it’s fairly easy to set up and the return on investment is almost immediate. The need for coding style checks during Code Reviews was eliminated entirely. Thus shifted the focus of code reviews purely to implementation details. Both, the reviewer and coder save time, since nobody needs to either point out or fix issues that can be rectified automatically.

With code coverage minimum checks we'll make sure the code coverage percentage doesn't drop below a certain threshold. Which means the developer needs to either write tests for his changes or if he's sneaky add more tests somewhere else.

## Required Guardians

You and your team need to decided what can be a deal breaker for a merge and what is optional. At [Zanui](https://www.zanui.com.au/) we require our style checks to be completed (which works automatically anyways) and our tests to finish successfully. However, we don't require Codacy or Code coverage minimum requirements to be successful. Codacy provides the coder/reviewer with a list of issues on what could be improved, but we didn't find all reported issues make sense. Code coverage minimum requirements were just introduced recently and are in a trial phase so everyone gets used to them.

Being able to selectively decide what is required provides a lot of flexibility in optimizing your software development process.

Setting up protected branches and required status checks will force you to wait for successful completion. It eliminates anyone jumping the gun and introducing automatically detectable issues or creating faulty releases.

## Resources
* [Protected branches and required status checks](https://github.com/blog/2051-protected-branches-and-required-status-checks)
