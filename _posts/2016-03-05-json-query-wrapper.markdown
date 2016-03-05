---
layout: post
title:  "jq for PHP"
date:   2016-03-04 09:00:00
categories: php
tags:   jq json
github_username: estahn
---

[jq](https://stedolan.github.io/jq/) is a great tool for processing JSON on the command line.
It's easy to install (e.g. `apt-get intall jq`) and easy to use.

When we started writing tests for our new REST API i wanted to have an easy way to query the returned JSON responses.
The end result is a small wrapper around [jq](https://stedolan.github.io/jq/).

[You can find the project on GitHub](https://github.com/estahn/json-query-wrapper)

Since i have revisited our testing strategy and we moved on to using JSON Schema.

There are [alternative projects](https://github.com/estahn/json-query-wrapper#alternatives) which might be better suited for you.
