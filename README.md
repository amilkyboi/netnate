# netnate

> *Last Updated: 21 November 2022*

This is a repository for my website development project, netnate. The legacy website was deployed using raw HTML, CSS, and JS and uses a third-party CSS theme hosted by W3Schools. Hugo, a static site generator, is now used and will allow for a more secure, upgradeable, and efficient website with full support for custom theming.

## Backend

Hugo content pages are written in Markdown (`.md`). This website is hosted on GitHub as a GitHub Pages project. Each time code is pushed to `main`, GitHub Actions builds the site automatically and the content is stored on the `gh-pages` branch. The GitHub Pages site is then built from the `gh-pages` branch.

## Philosophy

Websites today are extremely bloated and more often than not contain copious amounts of unnecessary or downright predatory tracking tools. A good website abides by the following ruleset:

- Free and Open-Source Software (FOSS)
- No tracking tools
- Minimal amounts of JavaScript
- Delivery optimization

Any and all of the information and/or opinions made in this project, during any stage, are and will always be completely free to share and distribute. This repository is licensed under the GNU GPLv3.

## Upgrades

Below are a list of features that this website implements, all of which are improvements over the old website:

- Minimal[^1] JavaScript.
- Much improved load times.
- Compressed `.webp` image formats only.
- Improved security.
- No reliance on outside CSS libraries.
- Increased modularity.
- Static site generation via [Hugo](https://gohugo.io/).

[^1]: No, I don't have some sort of vendetta against JS, but I do feel that it is extremely overused in today's online landscape, many times as an exploitative tool. A few select features on this website implement JS, such as the live table of contents and KaTeX math support.

## Pages

Out of all the improvements that I plan on making to the new website, I am most excited to implement blog and wiki style pages. These pages will be available for all users to view with the foremost intention of documenting the progress that I have made on various projects throughout the years. I hope that the culmination of knowledge that will eventually be documented on these pages will be useful not only to me, but to anyone who happens to stumble across my website.

Below is a tentative list of all the blog/wiki style pages that I plan on implementing in the future. Not all of these pages will be made immediately, some might never be made, and still others will be made that are not listed here.

- Linux
  - Arch
  - Gentoo
  - LFS
- Software
  - Code Editors
  - Browsers
- Engineering & Physics
  - Aerospace
  - Electrical
- Books
  - Virtual Bookshelf
  - Reviews
- Electronics
  - Arduino Projects
  - 3D Printed Components
- Fitness
  - Current Routine
  - Progress & Goals
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

## Roadmap

- [x] Create a working Hugo site.
- [x] Understand content organization and begin creating pages.
- [x] Get a workable outline of website structure.
- [x] Update the theme.
- [x] Create first post.
- [x] Make working series, categories, and tags pages.
- [x] Create an about page.
- [x] Fork a Hugo theme and implement upgrades.
- [ ] Implement a custom color theme.
