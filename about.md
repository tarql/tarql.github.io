---
layout: page
title: About
permalink: /about/
---

## Contact us

[tarql@googlegroups.com](mailto:tarql@googlegroups.com)


## Changelog

### 1.1 (2016-12-24)
Various minor new features, improvements and bugfixes.

- Upgrade from Jena 2 to Jena 3.1.1
- Add `tarql:expandPrefix` and `tarql:expandPrefixedName` functions
- Better handling of stray quotes that are misinterpreted as starting a multi-line field (#10)
- Handle question marks in column headers
- Allow `--quotechar none` (#10)
- Recognize `tab`, `singlequote`, `backslash` etc. as values for `--delimiter`, `--quotechar`, `--escapechar`
- Add `--base` argument (#20)
- Ensure `rdf:` prefix is always registered when writing Turtle (#36)
- Add `--stdin` option to read input from STDIN (#21)
- Generally show just error message instead of stack trace (#38)

### 1.0a (2014-10-02)
* Second release

### 0.9 (2013-02-08)
* First release


Last Update: {{ site.time | date: '%B %d, %Y' }}
