# amilkyboi.github.io

> *Last Updated: 14 November 2022*

This is a temporary repository for my website development project, NetNate. The current website is deployed using raw HTML, CSS, and JS and uses a third-party CSS theme hosted by W3Schools. Hugo, a static site generator, will be used on this repository to usher in a new age for NetNate. Hugo will allow me to maintain a more secure, easy to upgrade, and quicker to load website with full support for custom theming.

## Backend

Hugo sites are primarily written in Markdown (`.md`). This website is hosted on GitHub as a GitHub Pages project. Each time code is pushed to `main`, GitHub Actions builds the site automatically and the content is stored on the `gh-pages` branch. The GitHub Pages site is then built from the `gh-pages` branch.

## Philosophy

Websites today are extremely bloated and more often than not contain copious amounts of unnecessary or downright predatory tracking tools. A good website abides by the following ruleset:

- Free/Libre and Open-Source Software (F/LOSS)
- No tracking tools
- Minimal amounts of JavaScript
- Delivery optimization on any system for any network

Any and all of the information and/or opinions made in this project, during any stage, are and will always be completely free to share and distribute. My ultimate goal in creating this website is to make something that I can be proud to call my own.

## Upgrades

In what ways will the new iteration of NetNate be improved over the current version? Good question, here's just a few:

- Minimal[^1] JavaScript.
- Much improved load times.
- Compressed `.webp` image formats only.
- Improved security.
- No reliance on outside CSS libraries.
- Increased modularity.
- Static site generation (thanks [Hugo](https://gohugo.io/)!).

[^1]: No, I don't have some sort of vendetta against JS, but I do feel that it is extremely overused in today's online landscape, many times as an exploitative tool. Since the KaTeX typesetting tool is built using JS, I cannot completely refrain from using JS without compromising the purpose of the website.

## Pages

Good motivations are key to any project, but what will this site actually contain? Out of all the improvements that I plan on making to the new website, I am most excited to showcase blog/wiki style pages. These pages will be available for all users to view with the foremost intention of documenting the progress that I have made on various projects throughout the years. I hope that the culmination of knowledge (good and bad) that will eventually be documented on these pages will be useful not only to me, but to anyone who decides to give my website a visit.

Below is a tentative list of all the blog/wiki style pages that I plan on implementing on the new website. Not all of these pages will be made immediately, some might never be made, and still others will be made that are not listed here.

- Linux
  - Arch
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
- *And More!*

## Roadmap

- [x] Create a working Hugo site.
- [x] Understand content organization and begin creating pages.
- [x] Get a workable outline of website structure.
- [x] Update the theme.
- [ ] Create first post.
- [ ] Make working series, categories, and tags pages.
- [ ] Create an about page.
