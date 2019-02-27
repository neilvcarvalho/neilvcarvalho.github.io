---
layout: post
title:  "Rails/Webpacker asset compilation error: SyntaxError> Unexpected token"
date:   2019-02-26 21:20:00
categories: rails webpacker asset compilation syntaxerror unexpected token es6 pipeline
---

A colleague was having this problem today:
```
ExecJS::RuntimeError: SyntaxError: Unexpected token: operator (>) (line: 69370, col: 12, pos: 3058098)
```

It happened to be the file extension. The file was `.js`, so Asset Pipeline was
trying to compile it. As the code had arrow functions (`=>`), introducted in
EcmaScript 6, the code has to be in a '.es6' file for webpacker to pick and
compile to mundane javascript.

# TL/DR

If you are using ES6 features, use the `.es6` extension.