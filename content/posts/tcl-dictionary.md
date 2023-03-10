---
title: "Tcl Dictionary"
date: 2023-03-10T12:55:20-05:00
draft: true
---

Recently I've been playing around with possible machine-readable formats for a Spanish to English dictionary. My first attempt was using YAML, something along the lines of

```yaml
hablar:
  class: verb
  translation: to talk
```

The schema was pretty simple and soon enough I needed something a bit more complex, with support for multiple definitions, context, etc.
I could have probably worked on the YAML schema a bit more, and gotten it to be a good fit. But:

1. I don't like YAML, aside from [all the good reasons](https://ruudvanasseldonk.com/2023/01/11/the-yaml-document-from-hell) to not like it, I really just don't like whitespace-based syntax.
2. It's boring.

## Enter TCL

I've been TCL-curious for a while. I keep *trying* to use it for something but its never stuck. But, a little mostly-data DSL seems like the *perfect* use-case.
