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
2. It's boring! I've written a lot of YAML. It works, but I wanted to try something new.

## Enter TCL

I've been TCL-curious for a while. I keep *trying* to use it for something but its never stuck. But, a little mostly-data DSL seems like the *perfect* use-case.


So, here's what I've got so far. I plan to develop it further as I study more spanish.

## Dictionary Format

The basic format looks something like this.

```tcl
word subir {
    class verb.transitive

    context "to ascend" {
        a. to go up
    }

    context "to increase" {
        a. to raise
    }
}
```

Translations are defined with the "word" command.
The block it's passed is evaluated in the `::dictionary::definition` namespace, which defines a few procedures.

- `class` Primarily defines the words part of speech (noun, verb, adverb, etc.) then further (optional) classifications each preceded by a '.'
- `context` Defines a sub-definition associated with some context, described by the first parameter. The following block can is also evaluated in the `definition` namespace, so it can even contain sub-contexts, or special-case the part of speech.
- `[a-z].` A single letter followed by a "." defines a translation, formed by joining the arguments with spaces.

And that's about it. So far...

I'm planning to add a few more procedures the definition namespace, namely

- `example` Will give an example sentence with the word, and an English translation of the sentence.
- `conjugations` Will list conjugations for verbs. I'm still trying to figure out exactly how to do this. I think the best way is to write them all out, so that irregulars don't have to be special-cased, and for my own practice. But, I want some helper methods that will autofill regular verbs in some times.

## Implementation

Now, here's where it gets weird. I'm new to TCL, I don't know the idioms, and have *may* have gone a little crazy with it's dynamic features.

There's a global inside the `::dictionary` namespace called `EXECUTOR`. Functions are invoked in the namespace referenced by `EXECUTOR` as the dictionary is evaluated. The `::dictionary::definition` procedures normalize their input call an associated procedure in the `$EXECUTOR` namespace. For example `a. to go up` will invoke `$EXECUTOR::definition "a" "to go up"`. `$EXECUTOR::begin-word <WORD>` and `$EXECUTOR::end-word <WORD>` are called before and after the block is evaluated, there are similar functions for context.