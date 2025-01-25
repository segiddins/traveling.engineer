+++
date = '2025-01-24T13:17:12-08:00'
draft = false
title = '2024 in Review'
toc = true
tags = ["residency"]
+++

2024 was an eventful year, both personally & as the Ruby Central Security Engineer in Residence.

In my first year working full-time on the Ruby packaging ecosystem I shipped some bugs, a few big projects, and 

Oh, I also got married. Can't forget that.

Read on for some of the highlights, keeping in mind that there's always ongoing maintenance work on Bundler, RubyGems, and RubyGems.org.

<!--more-->

---

## Thanks

Of course, I need to start off by thanking the organizations that made this residency possible. First and foremost is AWS, who sponsored the residency for the first eleven months of the year. Second is Alpha-Omega, who has taken up the torch from AWS and will be covering the second year of the residency.

Finally, I need to thank the RubyGems team for their support and teamwork throughout the past _decade_ (I know, where have the years gone).
Additionally, continued professional work on RubyGems wouldn't be happening without the financial and organizational support of [Ruby Central](https://rubycentral.org).

## Sigstore

Adding support for [sigstore](https://sigstore.dev) to the RubyGems ecosystem was my main project for 2024, and it was a big one.

### sigstore-ruby

Built and released a pure-Ruby implementation of the sigstore protocols, providing the foundation for RubyGems' signing capabilities. The library handles certificate management, transparency log operations, and supports both online and offline signing workflows.

### RubyGems.org integration

Implemented sigstore support in RubyGems.org's infrastructure, enabling developers to sign gem releases using their GitHub identities. Added new API endpoints, storage systems, and monitoring for the signing pipeline.

### `gem push` support

Enhanced the `gem push` command with transparent sigstore signing support, making it seamless for developers to sign their releases, especially in GitHub Actions environments.

### What's Next

Planning to roll out signature verification in `gem install`, expand identity provider support beyond GitHub, and work on binary transparency integration.

## Incident Response

As a member of the RubyGems.org oncall rotation and as RubyGems security lead, responding to incidents is an ongoing part of my role.

### [research.rubygems.info](https://research.rubygems.info)

A new site that indexes all of the files in every single gem published on RubyGems.org.
Having this data available is a huge boon for incident response.
For example, we were able to identify that RubyGems was not a distribution vector for backdoored releases of `xz` in under 24 hours.

### xz

See [blog post](https://blog.rubygems.org/2024/03/31/rubygems-and-xz.html).

## RubyGems.org Audit

See [blog post](https://blog.rubygems.org/2024/12/11/security-audit.html).

## Trusted Publishing

Finished rollout of the [trusted publishing feature](https://guides.rubygems.org/trusted-publishing/).
Gems such as rails and many parts of the Ruby standard library are now using trusted publishing to push their releases.

### Tooling for Automated Setup

In preparation for the RailsConf hack day, I built out the tooling necessary to help gem maintainers configure trusted publishing for their gems.
There's now an API to add trusted publishing to a gem (scoped behind its own API key scope, which was a whole other can of worms), and a [CLI](https://github.com/rubygems/configure_trusted_publisher) that automates the setup process.

## Conferences

### Open Source Summit NA

I got to spend most of the Summit with peers in the package security space, which is just awesome. There's dozens of us! And we all show up at the same small set of events and know each other and get to commiserate.
We used the face time to map out the software supply chain landscape in the Alpha-Omega roundtable, and conversations like that help push together the entire field. (I can't share more details because Chatham House rules.)

### RailsConf

See the [Tooling for Automated Setup](#tooling-for-automated-setup) section above. Besides focusing on eating copious amounts of Detroit pizza, I got to cajole many gem owners into enabling trusted publishing for their gems. Every single one was amazed at how little effort it took to set up.

### Ruby Kaigi

At Kaigi, I gave a [talk about the Marshal binary serialization format](https://www.rubyvideo.dev/talks/remembering-ok-not-really-sarah-marshal?back_to=%2Fspeakers%2Fsamuel-giddins%3Fscroll_top%3D288&back_to_title=Samuel+Giddins). It was a solid reminder that securing the packaging ecosystem is inextricably linked to the security posture of the language and standard library itself, and I got to use the time to talk to Ruby Core members about some of the areas that could use attention.

After ending the conference, I went out karaoke-ing until 2am, and then woke up to fly home to attend my fiancee's business school graduation.

### Baltic Ruby

Post-wedding, I was off to Malmo to give a talk about [the state of RubyGems](https://www.rubyvideo.dev/talks/keynote-what-it-takes-to-keep-ruby-gems-a-thing?back_to=%2Fspeakers%2Fsamuel-giddins%3Fscroll_top%3D288&back_to_title=Samuel+Giddins) and host a space at the OSS Expo, again focused on trusted publishing.

### EuRuKo

https://www.rubyvideo.dev/talks/a-survey-of-recent-rubygems-cves?back_to=%2Fspeakers%2Fsamuel-giddins%3Fscroll_top%3D0&back_to_title=Samuel+Giddins

### Open Source Summit EU

### SigstoreCon

link to recording

https://speakerdeck.com/segiddins/the-challenges-of-building-a-sigstore-client-from-scratch

conversation about binary transparency schemes for package indices

### RubyConf

I helped give the [State of RubyGems talk](https://www.rubyvideo.dev/talks/the-state-of-rubygems?back_to=%2Fspeakers%2Fsamuel-giddins%3Fscroll_top%3D0&back_to_title=Samuel+Giddins) (slides [here](https://speakerdeck.com/mghaught/state-of-rubygems-2024)) along with Martin & Marty. I also helped man the RubyGems table during hack day, getting some folks onboarded to contribute to RubyGems.org.

Interestingly, I had some good hallway-track conversations about the future of Ruby version managers, inspiring a [holiday project](https://github.com/segiddins/chrb) to write my own.

## 2025

### Areas of Focus and Major Outcomes

For my second year as security engineer in residence, we’ve outlined three areas of focus for my time.  In addition, I will continue to be the security point of contact along with maintaining RubyGems.org as a member of the RubyGems team.

#### Precompiled Binaries

The major project will be adding the ability to upload multiple artifacts for a gem (supporting different ruby implementations, ruby versions, and operating systems) in a similar approach to Python’s “wheels”, with the goal being to decrease the number of extensions that get compiled on package install (running arbitrary code on install is not a desirable security property, needless to say).  Samuel will focus on this throughout the year with the goal to have a prototype in RubyGems that produces an RFC by summer.  The ultimate goal is to launch a public beta by the fall.

#### Build Provenance

The first piece of build provenance is releasing the Sigstore client work to RubyGems and encouraging public adoption of the new feature. This will be followed by “publish” attestations.  The next block of work is collaborating with Ruby Core to look at how to introduce reproducible builds of ruby itself.

#### Trusted Publishing Adoption

The focus will be increasing adoption of trusted publishing by the Ruby community.  I will improve RubyGems actions and introduce the ability to enforce trusted publishing and attestations with specific gems.

### Roadmap

This roadmap provides details on how I plan to sequence the work.

Precompiled Binaries

* Spec out the “wheel” format – file names, what they represent, what they are compatible with, specificity, and how they interact with Gem::Platform.  
* Prototype with an emphasis on client-side (RubyGems) work.  
* Finalize design/rfc based on the prototype.  
* Finish the majority of the changes in PRs ready to merge by summer.  
* Launch wheel support in RubyGems behind a feature flag for a public beta by fall.

Build Provenance

* Release the Sigstore client work, including trust policies, after iterating on beta feedback in late winter.  
* Implement “publish” attestations in Sigstore, figuring out what identity package repos sign with and the identifier used for those identities.  
* Attend RubyKaigi, Draft up plan for ruby “builds” / reproducibility / provenance after meeting with Ruby Core members.  
* Begin design work on the Ruby artifact story, starting with reproducible builds based on feedback from Ruby Core in fall.

Trusted Publishing

* Improve “official” RubyGems actions for publishing gems in accordance with new best practices.  
* Introduce the ability to enforce trusted publishing and attestations for specific gems on RubyGems.org by summer.

### Feedback

As always, I welcome feedback both on my work as well as the broader strategy for securing the Ruby ecosystem.
Please don't hesitate to reach out to me [via email](mailto:segiddins@rubycentral.org) or on the [Bundler Slack](https://join.slack.com/t/bundler/shared_invite/zt-1rrsuuv3m-OmXKWQf8K6iSla4~F1DBjQ).
