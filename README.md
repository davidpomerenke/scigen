![# SciGen.js](logo.png)

[![NPM version](https://img.shields.io/npm/v/scigen.svg)](https://www.npmjs.com/package/scigen)
[![Node CI](https://github.com/davidpomerenke/scigen.js/workflows/Node%20CI/badge.svg)](https://github.com/davidpomerenke/scigen.js/actions?query=workflow%3A%22Node+CI%22)
[![Gitter](https://badges.gitter.im/scigen-js/community.svg)](https://gitter.im/scigen-js/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

This project brings [SciGen](https://github.com/strib/scigen) to JavaScript, both for Node and for the browser.

[Try it here!](https://davidpomerenke.github.io/scigen.js)

## Usage

### Node
```javascript
import { scigen, scigenSave } from 'scigen'

const files = scigen(
  /* authors = */ ['Jeremy Stribling', 'Max Krohn', 'Dan Aguayo'], 
  /* avoid bibtex dependency = */ false)
console.log(files['paper.tex'])

scigenSave(
  /* directory = */ 'mydir', 
  /* authors = */ undefined, 
  /* avoid bibtex dependency = */ true)
```

### Command Line
```bash
$ git clone git@github.com:davidpomerenke/scigen.js && cd scigen
$ node lib/cli.js --help
Usage: node cli.js --save [<directory>] [--authors "<author1>, <author2>, ..."] [--bibinlatex] [--silent]
    directory   all files (.tex, .eps, .cls, .bib, ...) will be saved here
    authors     list of the authors in the paper
    bibinlatex  avoids dependency on BibTex (useful especially for texlive.js)
    silent      skip info logging
$ node cli.js --save tmp --authors "Jeremy Stribling, Max Krohn, Dan Aguayo" --silent
$ cd tmp
$ pdflatex -interaction=nonstopmode paper.tex 
$ bibtex paper.aux
$ pdflatex -interaction=nonstopmode paper.tex
$ pdflatex -interaction=nonstopmode paper.tex
$ xdg-open paper.pdf
```

### Browser
```bash
$ git clone git@github.com:davidpomerenke/scigen.js && cd scigen
$ npx webpack
$ python -m http.server -d docs
$ xdg-open http://localhost:8000
```

See also the [TexLive.js Wiki](https://github.com/manuels/texlive.js/wiki).

## Rule Compilation
The almost original rule files from the [original SciGen project](https://github.com/strib/scigen) are found in `rules/rules-original`. They can be compiled to JSON by running `perl rules/compile-rules.pl`. The JSON files are required for running the module. They are already included in the module and only need to be re-compiled for applying changes in the original `.in` rule files.

## Limitations
- Bibtex is not available for the browser (cf. [here](https://github.com/manuels/texlive.js/issues/7)). An almost perfect workaround is implemented for the parameter `--bibinlatex` (or setting the second/third function parameter to `true` in Node, see the above examples).
- Rendering diagrams and figures requires _Ghostscript_ in the [original SciGen project](https://github.com/strib/scigen). _Ghostscript_ is not available for the browser. A workaround would probably involve rewriting the original EPS rules in some format which is supported by _TexLive.js_ (maybe SVG or TIKZ). As this module is aimed at the browser, the diagram and figure code production is not yet implemented in the JavaScript code. For locally producing TEX and PDF files with figures and diagrams, use the [original SciGen project](https://github.com/strib/scigen) with [this unmerged fix](https://github.com/strib/scigen/pull/5).
- Works in Firefox Desktop & Mobile and in Chrome Mobile, but not in Chrome/Chromium Desktop. Cf. [this issue with TexLive.js](https://github.com/manuels/texlive.js/issues/63).

## Motivation
The server-side code at the [original SciGen website](https://pdos.csail.mit.edu/archive/scigen/) appears to be broken. The aim of this project is therefore to provide a more server-independent implementation.