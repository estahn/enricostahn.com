---
title: "Kubernetes Secrets in plain-text"
date: 2021-01-04T16:44:20+11:00
description: ""
keywords: []
tags: [kubernetes, secrets, k8s, kubectl]
toc: true
---

I've seen some elaborate solutions to easily retrieve Kubernetes secrets in plain-text.
This is what I came up with to make my life easier.

## Prerequisites

```bash
$ brew install jq
```

## Installation

Create a file (e.g. `~/.shellfn`) containing the following shell function:

```bash
function ksecret () {
  kubectl get secrets $@ -ojson | jq '{name: .metadata.name, data: .data | map_values(@base64d)}'
}
```

Include the file in your `.zshrc`, `.bashrc`, etc. or just via your CLI `source ~/.shellfn`.

## Usage

```bash
$ ksecret -n my-namespace the-secret
```

Output:
```json
{
  "name": "the-secret",
  "data": {
    "foo": "something in plain-text",
    "bar": "more something in plain-text"
  }
}
```

Use with with `pbcopy` or `jq` for additional convenience, e.g.

```bash
$ ksecret -n my-namespace the-secret | pbcopy
```
