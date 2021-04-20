# Will's Senior Thesis

* Use [this package](http://tug.ctan.org/macros/latex/contrib/algorithm2e/doc/algorithm2e.pdf) for algorithms

## Bibliography troubles

I've been having weird errors when compiling beamer presentations and regular latex documents with a bibliography. Seemingly randomly, I'll get all `citation ... undefined` errors. I tried lots of different things, and the only thing that seemed to work was copying the `refs.bib` file into the folder with the presentation source, changing the bib path, compiling, then changing the path again to the original bib file. No idea why that would work, but it seems to...
