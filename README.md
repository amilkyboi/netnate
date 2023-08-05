# netnate

> *Last Updated: 4 August 2023*

This is my website development project, [netnate](https://www.netnate.com). Previously, this website was written using raw HTML, CSS, and JS and used a third-party CSS theme hosted by W3Schools. I'm now using [Hugo](https://gohugo.io/), a static site generator, as my framework. Hugo allows for a more secure, upgradeable, and efficient website with full support for custom theming.

## Backend

Hugo content pages are written in Markdown (`.md`) and the website itself is hosted here on GitHub as a GitHub Pages project. Each time code is pushed to `main`, GitHub Actions builds the site automatically and the content is stored on the `gh-pages` branch. The GitHub Pages site is then built from the `gh-pages` branch after some automated checks are performed.

## Philosophy

Websites today are extremely bloated and more often than not contain copious amounts of unnecessary or downright predatory tracking tools. A good website:

- Abides by the four essential freedoms of free software
- Contains no tracking tools
- Is written using minimal amounts of JavaScript
- Prioritizes delivery optimization

Any and all information and/or opinions made in this project, during any stage, are and always will be completely free to share and distribute. This website is licensed under GNU GPLv3.

## Upgrades

Below are a list of features that this website implements, all of which are improvements over the old website:

- Minimal[^1] JavaScript
- Greatly improved load times
- Compressed `.webp` image formats only
- Improved security
- No reliance on outside CSS libraries
- Increased modularity
- Static site generation

[^1]: To be clear, a few select features on this website implement JS, such as the live table of contents, KaTeX math support, and the fuzzy finder search feature. Many websites, however, use JS far in excess of anything useful and hinder performance and/or security for no reason.

## Pages

Below is a tentative list of all the blog/wiki style pages that I plan on implementing in the future.

- Linux
  - Arch
  - Gentoo
- Software
  - Code Editors
  - Browsers
- Engineering
  - Aerospace
- Books
  - Virtual Bookshelf
  - Reviews
- Electronics
  - Arduino Projects
  - 3D Printed Components
- Stationery
  - Fountain Pens
  - Mechanical Pencils
  - Handwriting
- Language
  - Latin
  - German
- Philosophy
- History
- Art
- Music
- Cars

## Site Roadmap

- [x] Create a working Hugo site
- [x] Understand content organization and begin creating pages
- [x] Get a workable outline of website structure
- [x] Update the theme
- [x] Create first post
- [x] Make working series, categories, and tags pages
- [x] Create an about page
- [x] Fork a Hugo theme and implement upgrades
- [x] Change to a custom color theme
- [x] Improve the dropdown table of contents feature
