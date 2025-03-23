---
date: "2020-05-27T16:48:36Z"
title: Hello World
description: "Hello everyone! I'm Jochen Kraushaar, a passionate software developer and architect. This is my first blog post, and what better way to start than with a classic Hello World? In this blog, I'll share my best practices and experiences from everyday life to make life easier for myself and maybe other developers. Learn more about my journey and the technology stack that powers this blog. Welcome to Jochen's Cache!"
image: header.webp
---

This is my first article in this new blog.
And as it should be for a blog about software development, it starts with *Hello World*.

My name is Jochen Kraushaar.
I work as a software developer and architect.
I'm writing this blog primarily for myself.
It serves as a memory aid to find best practices from my daily routine without having to conduct extensive research again.
Or, in other words, it serves as a cache - Jochen's Cache.

# Requirements

As a software architect, I defined the following requirements at the beginning of this project:

* I want to use tools that I also use in my normal work routine.
* The site should be fast and avoid unnecessary JavaScript.
* Occasionally, I want to be able to easily embed code snippets.
* For security reasons, the site should be as static as possible.
* At the same time, I don't want to struggle with design, but rather focus on content.
* The site's build process should be automated.

# First Attempt: Hugo

During my research, I came across this article: [How to create a blog with AsciiDoc](https://opensource.com/article/17/8/asciidoc-web-development)

The author combines [Hugo][hugo] with [AsciiDoc][asciidoc] or [Asciidoctor][asciidoctor] to generate a blog that is then hosted on [GitHub Pages][github-pages].
Since I already use AsciiDoc in my developer routine with [docToolchain](https://doctoolchain.github.io/docToolchain/), this approach would be perfect for me.
I could write my blog posts in an editor (in my case, [Atom][atom]) and then generate a static site using Hugo.
Furthermore, Hugo also offers various themes and other features that I would have to create myself for a purely static site.

Unfortunately, I then discovered that while there is a basic integration with GitHub Pages, it requires a separate build process.
Somewhat disillusioned, I continued my research.

# Second Attempt: Jekyll

Upon closer examination of the GitHub Pages documentation, I quickly found that [Jekyll][jekyll] is preferred.
Here, the build process is taken over directly by GitHub Pages.
Unfortunately, GitHub Pages restricts the possible Jekyll plugins.
In my case, this means:

* No AsciiDoc support (Markdown is used instead)
* Very limited I18N support

On the other hand, a seamless integration with GitHub Pages is very important to me.
Moreover, the project structure and templates are somewhat easier to understand.
My choice fell on Jekyll as the generator.
I'm using [Atom][atom] as my editor in this case as well.

# My Technology Stack

My technology stack for this blog now looks like this:

* [Ruby][ruby] - as a prerequisite for Jekyll
* [Markdown][markdown]
* [Jekyll][jekyll]
* [Atom][atom]

Equipped with this, I only needed an imprint and a privacy policy.
And, of course, I had to install the application somehow.

# GitHub Pages

[GitHub Pages][github-pages] is particularly suitable for blogs in a private environment.
With a size of 1 GB for the site and 100 GB for bandwidth, GitHub Pages should be suitable for almost any private blog.
All of this is also free of charge.

Additionally, there is good [Getting Started](https://help.github.com/en/github/working-with-github-pages/getting-started-with-github-pages) documentation and documentation on [integrating with Jekyll](https://help.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll).

I'm also a big fan of the idea of making the source code of this site available on GitHub [here](https://github.com/bit-jkraushaar/bit-jkraushaar.github.io).

# Hello World

With that, this project is ready to see the light of day. In this sense:

> Hello World

-- *Every Programming Language ever*

[hugo]: https://gohugo.io/
[asciidoc]: https://asciidoc.org/
[asciidoctor]: https://asciidoctor.org/
[github-pages]: https://pages.github.com/
[atom]: https://atom.io/
[ruby]: https://www.ruby-lang.org/
[jekyll]: https://jekyllrb.com/
[markdown]: https://daringfireball.net/projects/markdown/