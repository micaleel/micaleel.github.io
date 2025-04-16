# BlogVibes - A Hugo Blog

This is a personal blog built with [Hugo](https://gohugo.io/), a fast and modern static site generator. The blog uses the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme for a clean and responsive design.

## Getting Started

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.80.0 or newer)
- Git (for version control and theme management)

### Local Development

To run the blog locally:

```bash
# Start the Hugo development server
hugo server -D
```

This will start a local server, typically at http://localhost:1313/

The `-D` flag tells Hugo to include draft posts. The server has hot-reloading enabled, so any changes you make will be immediately visible in the browser.

## Creating Content

### Blog Posts

To create a new blog post:

```bash
hugo new content posts/my-new-post.md
```

This will create a new markdown file in the `content/posts` directory with the necessary front matter.

### Pages

To create a new page:

```bash
hugo new content pagename/index.md
```

## Customization

### Configuration

The main configuration file is `hugo.toml`. You can modify this file to change the site title, theme, menu items, and other settings.

### Theme

This blog uses the PaperMod theme. You can customize the theme by:

1. Modifying the theme settings in `hugo.toml`
2. Creating custom layouts in the `layouts` directory
3. Adding custom CSS in the `assets/css` directory

## Building for Production

To build the site for production:

```bash
hugo
```

This will generate the static site in the `public` directory, ready to be deployed to any hosting service.

## Deployment

You can deploy this Hugo blog to various platforms:

- **GitHub Pages**: Use GitHub Actions to build and deploy
- **Netlify**: Connect your GitHub repository for continuous deployment
- **Vercel**: Similar to Netlify, with automatic deployments
- **Any Static Hosting**: Upload the contents of the `public` directory

## Additional Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [PaperMod Wiki](https://github.com/adityatelange/hugo-PaperMod/wiki)
- [Markdown Guide](https://www.markdownguide.org/) 