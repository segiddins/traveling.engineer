+++
date = '2025-02-24T4:30:00+01:00'
draft = false
title = 'Goals for Binary Gems ("Wheels")'
toc = true
tags = ["rubygems"]
+++

We want to encourage gem authors to ship precompiled binaries of their gems whenever possible (instead of the gem building an extension at install time).
This is better for security -- no code running at install time, and more ergonomic for users -- no need to install build tools, deal with build errors, waiting for slow compilation.

Problem statement: Dealing with gems that have different implementations and requirements across different OSes, CPU architectures, libc Ruby Engines, Ruby versions, (and ruby engine versions) is challenging for both gem authors and consumers.

<!--more-->

---

Challenges include:

Gem authors:
* Restricted in set of build tooling they can use

Gem consumers:
* Need to have the right build tools installed

Dependency resolution:

(Unrelated) Switch to a new /info_checksums instead of /versions
/info files will have a new metadata field, `wheel=true` that indicates that the "version" (version + platform string) is in this new format
Bundler/RubyGems will resolve to the most specific match whenever possible, falling back on existing (non-wheel) gems as needed.

Enforce filename canonicalization
First file in the archive is a plain-text key-value mapping of everything in the filename, so clients can easily verify that they haven't been confused by the normalization scheme

Version strings will continue to be `(version, platform)` tuples, but the platform piece will be expanded. See [pypa docs on platform tags](https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#platform-compatibility-tags) for inspiration on the different pieces. File names (and gem "full names") will also mirror [what python does for wheels](https://packaging.python.org/en/latest/specifications/binary-distribution-format/).

`{gem_name}-{version}-{ruby tag}-{abi tag}-{platform tag}.gem2`

Given this will necessitate publishing _new_ packages, that will only be understood by _new_ versions of RubyGems + Bundler, I see several potential oppportunities that come along with the semi-related breaking change.

Potential opportunities:

* Package each piece of gems into their own directory:
  * `lib/` for Ruby code
  * `ext/` for built extensions
  * `bin/` for executables
  * `share/` or `data/` for data files
  * `doc/` for documentation
  * `sig/` for signatures
  * Goal is to allow installation to be as simple as copying files into the right place, permitting other implementations for installers of wheels
* Allow for non-Ruby executables to be included in the gem
* Rethink how gem specs get serialized and stored
  * Can we move away from YAML that needs to reference RubyGems classes?
* Canonicalize encoding of file names in packages _or_ allow for specification of encoding
* Obviate the need for using `__dir__` or `__FILE__` in gem code, provide runtime lib like python resources module
