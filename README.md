# Sander's network (automation) blog

A technical blog focused on network automation. Built with
[MkDocs Material](https://squidfunk.github.io/mkdocs-material/) and deployed to Cloudflare Pages using GitHub actions.

## About

This blog shares insights, tutorials, and experiences related to network automation, infrastructure, and related
technologies.

## Local development

### Prerequisites

- Python 3.13 or higher
- [uv](https://github.com/astral-sh/uv) package manager

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/sanderdelden/blog.sanderdelden.nl.git
   cd blog.sanderdelden.nl
   ```

2. Install dependencies using uv:

   ```bash
   uv sync
   ```

### Running the development server

Start the live-reloading development server:

```bash
uv run mkdocs serve
```

The site will be available at [http://127.0.0.1:8000](http://127.0.0.1:8000). Changes to source files will automatically
trigger a rebuild and browser refresh.

### Building the site

Generate the static site files:

```bash
uv run mkdocs build
```

Built files will be output to the `site/` directory.

> [!TIP]
> Use the `--clean` flag to clear the `site/` directory before building.
>
> Using the `--strict` flag MkDocs will catch warnings as errors.

## Writing content

### Creating a new blog post

1. Create a new markdown file in `blog/posts/YYYY/` (where YYYY is the current year):

   ```bash
   touch blog/posts/2025/my-new-post.md
   ```

2. Add content:

   ```markdown
   ---
   date: 2025-10-01
   authors:
   - sander
   tags:
   - python
   - automation
   ---

   # My new post title

   Post content goes here...
   ```

3. The post will automatically appear on the blog homepage and in the RSS feed.

> [!TIP]
> See `blog/posts/example.md` for an example.

## Code quality

The project uses several linters to maintain code quality:

- **Markdown**: `markdownlint`

   Markdownlint is configured with the config file `.markdownlint.yml`. To run it locally
   [markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli) is used:

   ```bash
   markdownlint .
   ```

- **YAML**: `yamllint`

   Yamllint is configured with the config file `.yamllint.yml`. It is installed as a dev dependency by uv, this means
   that it is added (or updated) when `uv sync` is ran. You can run the linter with:

   ```bash
   uv run yamllint .
   ```

### Actions

In the GitHub actions [super-linter](https://github.com/super-linter/super-linter/tree/main) is used to lint all files.

## Deployment

The site is automatically deployed to Cloudflare Pages via GitHub actions on every push to the `main` branch of the
repository. The workflow looks as follows:

1. **Lint**: Code quality checks with super-linter
2. **Build**: Generate static site with MkDocs
3. **Deploy**: Push to Cloudflare Pages

> [!NOTE]
> Each job must be successful for the next one to run, meaning that `build` will not start if `lint` fails and so
> forth.

### Manual deployment

If needed, you can deploy manually using Cloudflare Wrangler. Running the following command will redirect to an OAth
page to authenticate with Cloudflare:

```bash
npx wrangler pages deploy site/ --project-name=blog-sanderdelden-nl
```
