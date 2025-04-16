+++
title = 'Hello Notes'
date = 2025-04-03T15:13:03+01:00
draft = true
tags = ["hugo", "tutorial", "static site generator"]
categories = ["Tech", "Tutorials"]
description = "Hello Notes"
+++

# Hello Notes

[Hugo](https://gohugo.io/) is a fast and modern static site generator written in Go. It's designed to make website creation fun again. In this post, I'll walk you through the basics of setting up a Hugo website.

## Why Choose Hugo?

There are several reasons why Hugo stands out among static site generators:

- **Speed**: Hugo is incredibly fast, generating most sites in milliseconds
- **Flexibility**: Highly customizable with templates and shortcodes
- **Easy Installation**: Simple setup with a single binary
- **Great Community**: Active development and helpful community
- **No Dependencies**: No need for complex runtime environments

## Installation

Installing Hugo is straightforward. Here are the most common methods:

### On macOS:

```bash
brew install hugo
```

### On Windows:

```bash
choco install hugo -confirm
```

### On Linux:

```bash
snap install hugo
```

## Creating a New Site

Once Hugo is installed, you can create a new site with:

```bash
hugo new site my-blog
```

This will create a new directory with the basic Hugo structure.

## Adding a Theme

Hugo sites rely on themes for design and functionality:

```bash
cd my-blog
git clone https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

Then edit your `config.toml` file to include:

```toml
theme = "PaperMod"
```

## Creating Content

To create a new post:

```bash
hugo new posts/my-first-post.md
```

Edit the created markdown file to add your content.

## Running Your Site Locally

To preview your site:

```bash
hugo server -D
```

This will start a local web server, usually at `http://localhost:1313/`.

## Building and Deploying

When you're ready to publish:

```bash
hugo
```

This generates your static site in the `public` directory, which you can then deploy to any web server or hosting service like GitHub Pages, Netlify, or Vercel.

## Next Steps

As you get more comfortable with Hugo, you can explore:

- Custom shortcodes
- Taxonomies for organizing content
- Customizing themes
- Adding comments and analytics
- Creating custom layouts

---

*Have you built a site with Hugo? Share your experience in the comments!*
