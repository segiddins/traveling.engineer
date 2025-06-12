---
date: '2025-06-12T02:02:47Z'
created_at: '2025-06-12T02:02:47Z'
updated_at: '2025-06-12T02:02:47Z'
title: When One Class Should Be Two
slug: when-one-class-should-be-two
draft: true
---
I’ve been working on refactoring some of the guts of RubyGems and Bundler lately, with the goal of improving their handling of “platforms”. The idea being we want to go from a platform like `arm64-darwin-20` to a platform that can encode compatibility across multiple different CPUs, OSs, and OS versions (while adding in the ability to encode compatibility with different ruby language versions, engine versions, and ABIs).

Naturally, there’s a `Gem::Platform` class that represents, 
