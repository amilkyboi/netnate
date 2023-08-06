---
draft: false
title: Laplace Table
description: A simple table of Laplace transforms typeset in LaTeX.

tags: [computing]

date: 2023-01-28

ShowToc: false
TocOpen: false

math: false
---

Most Laplace tables I have found online have either too little information or do not meet my standards for typesetting, so I made my own.

## Document

![Laplace table](/images/laplace_table.webp)

## Source Code

```latex
\documentclass[12pt]{article}
\usepackage[margin=0.25in]{geometry}
\usepackage{tabularx, amsmath, mathrsfs, physics}

\begin{document}
\thispagestyle{empty}

\large{\begin{center}\textbf{Table of Laplace Transforms}\end{center}}
\normalsize
\begin{equation*}
    F(s)=\mathscr{L}\left\{f(t)\right\}=\int_{0}^{\infty}f(t)e^{-st}\dd{t}
\end{equation*}

\begin{center}
\begin{tabularx}{\linewidth}{rXc|rXc}
    & $f(t)=\mathscr{L}^{-1}\left\{F(s)\right\}$ & $F(s)=\mathscr{L}\left\{f(t)\right\}$ & & $f(t)=\mathscr{L}^{-1}\left\{F(s)\right\}$ & $F(s)=\mathscr{L}\left\{f(t)\right\}$ \\
    \hline
    \rule{0pt}{5.5ex}
    1. & $1$ & $\dfrac{1}{s}$ & 2. & $e^{at}$ & $\dfrac{1}{s-a}$ \\
    \rule{0pt}{5.5ex}
    3. & $t^n,\;n=1,2,3,\dotsc$ & $\dfrac{n!}{s^{n+1}}$ & 4. & $t^p,\;p>-1$ & $\dfrac{\Gamma(p+1)}{s^{p+1}}$ \\
    \rule{0pt}{5.5ex}
    5. & $\sqrt{t}$ & $\dfrac{\sqrt{\pi}}{2s^\frac{3}{2}}$ & 6. & $t^{n-\frac{1}{2}},\;n=1,2,3,\dotsc$ & $\dfrac{1\cdot3\cdot5\dotsm(2n-1)\sqrt{\pi}}{2^ns^{n+\frac{1}{2}}}$ \\
    \rule{0pt}{5.5ex}
    7. & $\sin(at)$ & $\dfrac{a}{s^2+a^2}$ & 8. & $\cos(at)$ & $\dfrac{s}{s^2+a^2}$ \\
    \rule{0pt}{5.5ex}
    9. & $t\sin(at)$ & $\dfrac{2as}{\left(s^2+a^2\right)^2}$ & 10. & $t\cos(at)$ & $\dfrac{s^2-a^2}{\left(s^2+a^2\right)^2}$ \\
    \rule{0pt}{5.5ex}
    11. & $\sin(at)-at\cos(at)$ & $\dfrac{2a^3}{\left(s^2+a^2\right)^2}$ & 12. & $\sin(at)+at\cos(at)$ & $\dfrac{2as^2}{\left(s^2+a^2\right)^2}$ \\
    \rule{0pt}{5.5ex}
    13. & $\cos(at)-at\sin(at)$ & $\dfrac{s\left(s^2-a^2\right)}{\left(s^2+a^2\right)^2}$ & 14. & $\cos(at)+at\sin(at)$ & $\dfrac{s\left(s^2+3a^2\right)}{\left(s^2+a^2\right)^2}$ \\
    \rule{0pt}{5.5ex}
    15. & $\sin(at+b)$ & $\dfrac{s\sin(b)+a\cos(b)}{s^2+a^2}$ & 16. & $\cos(at+b)$ & $\dfrac{s\cos(b)-a\sin(b)}{s^2+a^2}$ \\
    \rule{0pt}{5.5ex}
    17. & $\sinh(at)$ & $\dfrac{a}{s^2-a^2}$ & 18. & $\cosh(at)$ & $\dfrac{s}{s^2-a^2}$ \\
    \rule{0pt}{5.5ex}
    19. & $e^{at}\sin(bt)$ & $\dfrac{b}{(s-a)^2+b^2}$ & 20. & $e^{at}\cos(bt)$ & $\dfrac{s-a}{(s-a)^2+b^2}$ \\
    \rule{0pt}{5.5ex}
    21. & $e^{at}\sinh(bt)$ & $\dfrac{b}{(s-a)^2-b^2}$ & 22. & $e^{at}\cosh(bt)$ & $\dfrac{s-a}{(s-a)^2-b^2}$ \\
    \rule{0pt}{5.5ex}
    23. & $t^ne^{at}\;,n=1,2,3,\dotsc$ & $\dfrac{n!}{(s-a)^{n+1}}$ & 24. & $f(ct)$ & $\dfrac{1}{c}F\left(\dfrac{s}{c}\right)$ \\
    \rule{0pt}{5.5ex}
    25. & $u_c(t)=u(t-c)$ & $\dfrac{e^{-cs}}{s}$ & 26. & $\delta(t-c)$ & $e^{-cs}$ \\
    \rule{0pt}{5.5ex}
    27. & $u_c(t)f(t-c)$ & $e^{-cs}F(s)$ & 28. & $u_c(t)g(t)$ & $e^{-cs}\mathscr{L}\left\{g(t+c)\right\}$ \\
    \rule{0pt}{5.5ex}
    29. & $e^{ct}f(t)$ & $F(s-c)$ & 30. & $t^nf(t),\;n=1,2,3,\dotsc$ & $(-1)^nF^{(n)}(s)$ \\
    \rule{0pt}{5.5ex}
    31. & $\dfrac{1}{t}f(t)$ & $\int_{s}^{\infty}F(u)\dd{u}$ & 32. & $\int_{0}^{t}f(v)\dd{v}$ & $\dfrac{F(s)}{s}$ \\
    \rule{0pt}{5.5ex}
    33. & $\int_{0}^{t}f(t-\tau)g(\tau)\dd{\tau}$ & $F(s)G(s)$ & 34. & $f(t+T)=f(t)$ & $\dfrac{\int_{0}^{T}e^{-st}f(t)\dd{t}}{1-e^{-sT}}$ \\
    \rule{0pt}{5.5ex}
    35. & $\dot{f}(t)$ & $sF(s)-f(0^+)$ & 36. & $\ddot{f}(t)$ & $s^2F(s)-sf(0^+)-\dot{f}(0^+)$ \\
\end{tabularx}
\end{center}

\end{document}
```
