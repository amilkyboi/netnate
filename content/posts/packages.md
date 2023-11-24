+++
title = 'LaTeX Packages'
date = 2022-08-02
draft = false
tags = ['computing']
summary = 'Some neat packages.'

math = true
+++

## Essentials

Generally useful packages that offer commonly used macros.

1. [`fancyhdr`](https://ctan.org/pkg/fancyhdr) - provides tools for constructing headers and footers with all kinds of customizable options
2. [`geometry`](https://ctan.org/pkg/geometry) - changes page dimensions and removes the unnecessarily large default margins
3. [`hyperref`](https://ctan.org/pkg/hyperref) - support for external hyperlinks and internal document references
4. [`mathtools`](https://ctan.org/pkg/mathtools) - an extension that internally loads the [`amsmath`](https://ctan.org/pkg/amsmath) package along with various fixes and improvements
5. [`parskip`](https://ctan.org/pkg/parskip) - allows for zero `\parindent` and nonzero `\parskip`, which makes for much more readable documents

## Other Useful Packages

Packages I use depending on the type and scale of the document itself.

1. `amssymb` - provides an extended symbol collection as a superset of [`amsfonts`](https://ctan.org/pkg/amsfonts) that includes some more obscure symbols
   - `amsfonts` is included and loads things like `\mathbb` for characters like \\(\mathbb{R}\\)
   - If the heirarchy of `ams` packages is confusing, check [this](https://tex.stackexchange.com/questions/32100/what-does-each-ams-package-do) thread out
2. [`amsthm`](https://ctan.org/pkg/amsthm) - enhanced version of \\(\LaTeX\\)'s `\newtheorem` command for defining customizable theorem-like environments
3. [`biblatex`](https://ctan.org/pkg/biblatex) - works in tandem with a `.bib` file to compile references
4. [`bm`](https://ctan.org/pkg/bm) - bold symbols in math mode that are safer than those from `\boldsymbol`
5. [`booktabs`](https://ctan.org/pkg/booktabs) - improves the look of tables
6. [`cancel`](https://ctan.org/pkg/cancel) - places lines through cancelled terms in math equations
7. [`caption`](https://ctan.org/pkg/caption) - customized captions in figure and table environments, I like my caption headings **bolded** for readability
8. [`enumitem`](https://ctan.org/pkg/enumitem) - provides additional numbering and structuring control over the enumerate, itemize, and description environments
9. [`float`](https://ctan.org/pkg/float) - sometimes necessary for the placement of stubborn figures and tables
10. [`graphicx`](https://ctan.org/pkg/graphicx) - allows for graphics, images, etc. to be added
11. [`mathrsfs`](https://ctan.org/pkg/mathrsfs) - provides the `\mathscr` command for formal uppercase script letters like \\(\mathscr{P}\\)
      - The internal \\(\LaTeX\\) script letters from `\mathcal` look like \\(\mathcal{P}\\)
12. [`minted`](https://ctan.org/pkg/minted) - syntax highlighting for all sorts of source code using the [Pygments](https://pygments.org) library
13. [`multicol`](https://ctan.org/pkg/multicol) - can be used to have multiple columns of text, I primarily use it for 2-column lists
14. [`pgfplots`](https://ctan.org/pkg/pgfplots) - draws high-quality function plots in normal or logarithmic scaling with a user-friendly interface
15. [`physics`](https://ctan.org/pkg/physics) - a somewhat controversial package that includes several macros for derivatives, linear algebra, and vector calculus
      - I find the features included in this package extremely useful and concise, but many people point out that it abuses `xparse`: see [this](https://tex.stackexchange.com/questions/471532/alternatives-to-the-physics-package) thread for more details
      - I've never had any issues with this package despite using it alongside all kinds of things, so I don't buy the hate
16. [`siunitx`](https://ctan.org/pkg/siunitx) - a comprehensive SI unit package that includes macros for scientific notation
17. [`tcolorbox`](https://ctan.org/pkg/tcolorbox) - nicely formatted and customizable boxes for examples and worked problems
18. [`tikz`](https://ctan.org/pkg/tikz) - adds support for native diagram and graph creation
