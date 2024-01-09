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
   - `amsfonts` is included and loads things like `\mathbb` for characters like $\mathbb{R}$
   - If the heirarchy of `ams` packages is confusing, check [this](https://tex.stackexchange.com/questions/32100/what-does-each-ams-package-do) thread out
2. [`amsthm`](https://ctan.org/pkg/amsthm) - enhanced version of $\LaTeX$'s `\newtheorem` command for defining customizable theorem-like environments
3. [`biblatex`](https://ctan.org/pkg/biblatex) - works in tandem with a `.bib` file to compile references
4. [`bm`](https://ctan.org/pkg/bm) - bold symbols in math mode that are safer than those from `\boldsymbol`
5. [`booktabs`](https://ctan.org/pkg/booktabs) - improves the look of tables
6. [`braket`](https://ctan.org/pkg/braket) - provides macros to typeset bra-ket notation
7. [`cancel`](https://ctan.org/pkg/cancel) - places lines through cancelled terms in math equations
8. [`caption`](https://ctan.org/pkg/caption) - customized captions in figure and table environments, I like my caption headings **bolded** for readability
9. [`derivative`](https://ctan.org/pkg/derivative) - derivatives and differentials via short, easy to remember macros
10. [`enumitem`](https://ctan.org/pkg/enumitem) - provides additional numbering and structuring control over the enumerate, itemize, and description environments
11. [`esint`](https://ctan.org/pkg/esint) - useful for typesetting line and surface integrals
12. [`float`](https://ctan.org/pkg/float) - sometimes necessary for the placement of stubborn figures and tables
13. [`graphicx`](https://ctan.org/pkg/graphicx) - allows for graphics, images, etc. to be added
14. [`mathrsfs`](https://ctan.org/pkg/mathrsfs) - provides the `\mathscr` command for formal uppercase script letters like $\mathscr{P}$
      - The internal $\LaTeX$ script letters from `\mathcal` look like $\mathcal{P}$
15. [`mhchem`](https://ctan.org/pkg/mhchem) - typesetting for chemical molecular formulae and equations
16. [`minted`](https://ctan.org/pkg/minted) - syntax highlighting for all sorts of source code using the [Pygments](https://pygments.org) library
17. [`multicol`](https://ctan.org/pkg/multicol) - can be used to have multiple columns of text, I primarily use it for 2-column lists
18. [`pgfplots`](https://ctan.org/pkg/pgfplots) - draws high-quality function plots in normal or logarithmic scaling with a user-friendly interface
19. [`siunitx`](https://ctan.org/pkg/siunitx) - a comprehensive SI unit package that includes macros for scientific notation
20. [`tcolorbox`](https://ctan.org/pkg/tcolorbox) - nicely formatted and customizable boxes for examples and worked problems
21. [`tikz`](https://ctan.org/pkg/tikz) - adds support for native diagram and graph creation

## A Note on the [`physics`](https://ctan.org/pkg/physics) Package

For a while, I used the `physics` package for most of my mathematical typesetting. After using this package to write dozens of homeworks and reports over the past year, I have some thoughts on it.

### The Good

With macros for automatic bracing, vector notation, operators, derivatives, differentials, and Dirac notation, `physics` includes everything I have ever needed for typesetting higher level physics and engineering work (aside from more obscure integrals, which are handled by `esint`). It's easy to use, and still has the best macros for derivatives and differentials.

### The Bad

The problem most people have with `physics` is how it's been implemented. There's a great [thread](https://tex.stackexchange.com/questions/471532/alternatives-to-the-physics-package) on the $\TeX$ StackExchange explaining how `physics` abuses spacing using the `\xparse` macro and other issues present within the source code. This is not ideal, but also isn't a dealbreaker for me.

Honestly, I never had any issues with spacing, but I did have other problems that led me to use other packages. When using the package, I would often run into small issues with macro names or properties not being to my liking. However, due to the design of `physics` going against the first point of the Unix philosophy, trying to add packages to fix these issues just led to overwriting macro definitions and even more problems. There's also the tendency of `physics` to rename macros already defined by $\LaTeX$ or `amsmath`, which I don't care for. Additionally, the `\qty` command is notorious for conflicting with `\siunitx`, which I happen to use regularly. Finally, the package hasn't been updated since 2012, I suppose because the authors see no need to fix the issues present.

### The Fix

Reimplementing `physics` solely using other packages isn't really possible. There's a newer package named [`physics2`](https://ctan.org/pkg/physics2) that implements some of the features of `physics`, but it's by no means a replacement. Instead, I've used `derivative` to replace derivatives and differentials, `bm` to help with vector notation, and `braket` for Dirac notation, along with several custom commands to replace the parts of `physics` that I most often use.

My preamble is shown below. I'm not totally happy with how the automated braces are being performed; ideally I'd like a single macro `\ab` that accepts `(`, `[`, `{`, and `|` as arguments. I'll get to that at some point.

```latex
\usepackage{bm, braket, derivative, esint, mathtools}

% automated bracing
\DeclarePairedDelimiter{\pr}{\lparen}{\rparen}        % parenthesis
\DeclarePairedDelimiter{\bk}{\lbrack}{\rbrack}        % brackets
\DeclarePairedDelimiter{\br}{\lbrace}{\rbrace}        % braces
\DeclarePairedDelimiter{\vr}{\lvert}{\rvert}          % pipes
\DeclarePairedDelimiter{\dvr}{\lVert}{\rVert}         % double pipes
\newcommand{\abs}[1]{\vr*{#1}}                        % absolute value
\newcommand{\norm}[1]{\dvr*{#1}}                      % norm
\newcommand{\eval}[3]{\left.#1\right\rvert_{#2}^{#3}} % evaluation limits
\newcommand{\order}[1]{\mathcal{O}\pr*{#1}}           % order of magnitude
\newcommand{\comm}[2]{\bk*{#1, #2}}                   % commutator
\newcommand{\acomm}[2]{\br*{#1, #2}}                  % anticommutator
\newcommand{\pb}[2]{\br*{#1, #2}}                     % poisson bracket

% vector notation
\newcommand{\vb}[1]{\bm{#1}}                % bold vector
\newcommand{\vu}[1]{\bm{\hat{#1}}}          % unit vector
\newcommand{\vdot}{\bm{\cdot}}              % dot product
\newcommand{\vcrs}{\bm{\times}}             % cross product
\newcommand{\grad}[1]{\bm{\nabla} #1}       % gradient
\newcommand{\divr}[1]{\bm{\nabla} \vdot #1} % divergence
\newcommand{\curl}[1]{\bm{\nabla} \vcrs #1} % curl
\newcommand{\slap}[1]{\nabla^2 #1}          % scalar laplacian
\newcommand{\vlap}[1]{\bm{\nabla}^2 #1}     % vector laplacian

% dirac notation
\renewcommand{\bra}{\Bra}             % expanding bra as default
\renewcommand{\ket}{\Ket}             % expanding ket as default
\renewcommand{\braket}{\Braket}       % expanding bra-ket as default
\newcommand{\ev}[1]{\braket{#1}}      % expectation value
\newcommand{\ip}[2]{\braket{#1|#2}}   % inner product
\newcommand{\op}[2]{\ket{#1}\bra{#2}} % outer product

% matrices
\newcommand{\pmx}[1]{\begin{pmatrix} #1 \end{pmatrix}} % parenthesis matrix
\newcommand{\bmx}[1]{\begin{bmatrix} #1 \end{bmatrix}} % bracketed matrix
\newcommand{\vmx}[1]{\begin{vmatrix} #1 \end{vmatrix}} % vertical matrix
\newcommand{\cmx}[1]{\begin{Bmatrix} #1 \end{Bmatrix}} % curly matrix
\newcommand{\tp}{^\mathrm{T}}                          % transpose
\DeclareMathOperator{\tr}{tr}                          % trace

% conjugates
\newcommand{\cc}[1]{#1^*}    % complex conjugate
\newcommand{\hc}[1]{#1^\dag} % hermitian conjugate

% linear operators
\newcommand{\vop}[1]{\mathbf{#1}} % vector operator
\newcommand{\sop}[1]{\mathrm{#1}} % scalar operator

% constants
\newcommand{\img}{i} % imaginary unit
\newcommand{\eul}{e} % euler's number

% other
\newcommand{\defn}{\coloneq}            % definition
\newcommand{\subtxt}[1]{_{\mathrm{#1}}} % upright subscripts
\DeclareRobustCommand\_{\ifmmode\expandafter\subtxt\else\textunderscore\fi}
```

### Small Aside on Upright Math

I have a neat little upright subscript command that I found on StackExchange at some point and use regularly. Many authors have a tendency to only use italicized characters in math mode. My rules for using upright characters are thus (list pulled from [here](https://tex.stackexchange.com/questions/33120/should-subscripts-in-math-mode-be-upright)):

- upright function names ($\sin{x}$, $\cos{x}$, $\ln{x}$, etc.)
- upright differentials ($\mathrm{d}x$ and $\int x \ \mathrm{d}x$)
- upright dimensionless numbers ($\mathrm{Re}$, $\mathrm{Pr}$, etc.)
- upright descriptive text
- upright descriptive variable indices (that don't correspond to other variables)

I think the first bullet point is clear enough, and is generally well followed. After all, most commonly used functions are already defined as built-in macros within $\LaTeX$, so there's no reason *not* to use them.

Typesetting differentials is mainly a matter of personal taste, but I find upright differentials to be more clear (and more correct, since they're operators, but I won't go there). Most mathematicians still prefer the italic text, and most existing material still uses this form. In newer texts, I have started seeing a marked increase in the upright variation, so it seems to be catching on, at least in the physics and engineering communities.

Upright dimensionless numbers are more important. For instance, using $Re$ instead of $\mathrm{Re}$ to denote the Reynold's number is unclear, since $R$ and $e$ could easily be interpreted as two separate variables. In most cases, the meaning will be clear from context, but it's always better to adopt a standard and follow it for the sake of consistency. Descriptive text falls into the same category.

Variable indices are the most tricky to nail down. When the subscript is descriptive, like $a_\mathrm{profile}$, it should be upright. For index counters as in $\sum_i x_i$, italic is correct. Technically, according to ISO and NIST, one-letter subscripts that refer to a name, such as $k_\mathrm{B}$ for the Boltzmann constant, or $m_\mathrm{e}$ for the mass of an electron, should be upright. However, the American Physical Society recommends all single-letter subscripts to be italic. I've seen both in books and in papers, but I prefer the APS guidelines.

If you're following the 'official' ISO standards, even the imaginary unit $i$, Euler's number $e$, and $\pi$ should be upright. This practice is absolutely atrocious and I don't recommend following it.
