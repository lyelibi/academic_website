# Copilot Instructions: Academic Website (Wowchemy Hugo Theme)

## Project Overview

This is a **Wowchemy Academic Resume template** - a Hugo-based static site generator for academic/personal websites. The site combines content written in Markdown with a modular widget-based homepage system, powered by Hugo modules and deployed via Netlify.

**Key Stack:** Hugo (v0.83.1), Wowchemy Hugo Modules, YAML configuration, Markdown content

## Architecture & Major Components

### 1. Hugo Module-Based Theme System
- **Theme source:** GitHub modules (`github.com/wowchemy/wowchemy-hugo-modules`)
- **Modules** (in `config/_default/config.yaml`):
  - `wowchemy-cms`: Editing interface
  - `wowchemy`: Core theme with widgets
- **Key config files:**
  - `config/_default/config.yaml` - Hugo build settings, module imports, markup handlers
  - `config/_default/params.yaml` - Site appearance (theme, fonts), contact info, SEO
  - `config/_default/menus.yaml` - Navigation links (reference `#about`, `#posts`, etc. = home widgets)
  - `config/_default/languages.yaml` - Multi-language support (currently English only)

### 2. Homepage as Widget System
- **Single entry point:** `content/home/index.md` (marks page as `headless: true`)
- **Widgets are separate files in `content/home/`:** `about.md`, `featured.md`, `contact.md`, `posts.md`, `projects.md`, `publications.md`, `skills.md`, `experience.md`, `accomplishments.md`, `talks.md`, `tags.md`, `demo.md`
- Each widget has YAML front matter defining:
  - `widget:` type (e.g., `about`, `featured`, `contact`)
  - `weight:` sort order (lower = earlier on page)
  - `headless: true` (invisible in sidebar)
  - `active: true/false` (include/exclude from homepage)
  - Widget-specific fields (e.g., `author: admin` for About widget)

**Pattern:** Modify `content/home/` files to customize homepage sections; don't edit `index.md` itself.

### 3. Content Structure by Type
- **Authors:** `content/authors/` - Author profiles (e.g., `admin/_index.md` for site owner)
  - Front matter: `title`, `role`, `organizations`, `education`, `interests`, `social` links, `superuser` flag
  - Special field: `icon:` for status emoji
- **Publications:** `content/publication/` - Academic papers with metadata
  - Fields: `publication_types` (0-8 numeric codes), `doi`, `author_notes`, `featured`, `tags`
- **Posts:** `content/post/` - Blog entries (simpler than publications)
- **Projects:** `content/project/` - Portfolio projects
- **Events/Talks:** `content/event/` - Conference talks, presentations

### 4. Critical Markdown & Templating Features
- **Callout shortcodes:** `{{% callout note %}}..{{% /callout %}}` (note, warning, etc.)
- **Uncoded shortcodes:** `{{% ... %}}` (text/rich content blocks)
- **Inline math:** `$...$` (LaTeX)
- **Display math:** `$$...$$`
- **Highlight fenced code:** Uses Goldmark markdown handler with `unsafe: true` renderer
- **Author linking:** In publications, use folder names (e.g., `admin` = `content/authors/admin/`)

## Developer Workflows

### Local Development
```bash
# Start dev server with full warnings
bash view.sh
# Starts: hugo server --disableFastRender --i18n-warnings
# Default: http://localhost:1313
```

### Updating Wowchemy Theme & Hugo Version
```bash
bash update_wowchemy.sh
```
- Runs `hugo mod get -u ./...` and `hugo mod tidy`
- Fetches compatible Hugo version from upstream and updates `netlify.toml`
- **Important:** Review release notes after running - may require config/content migration

### Building for Production
```bash
hugo --gc --minify -b $URL
# Outputs to: public/
```
(Netlify runs this automatically; see `netlify.toml` for environment settings)

### Deployment
- **Netlify config:** `netlify.toml`
  - Build command: `hugo --gc --minify -b $URL`
  - Build environment: `HUGO_VERSION = "0.83.1"`, `HUGO_ENABLEGITINFO = true`
  - Caching plugin: `netlify-plugin-hugo-cache-resources`
  - Deploys from `public/` folder

## Project-Specific Patterns & Conventions

### Navigation & Menu Linking
- Menus reference homepage widgets with hash anchors: `url: '#about'`, `url: '#posts'`
- Weights determine order (10 = Home, 20 = Posts, 30 = Projects, 40 = Contact)
- Disabled items are commented out (e.g., Talks, Publications)

### Ignored Files (in `config.yaml`)
- `.ipynb` notebooks, Jupyter checkpoints
- `.Rmd` R Markdown files
- `_cache` directories
These are excluded to avoid duplicate processing by Hugo.

### Publication Type Codes
```
0=Uncategorized, 1=Conference paper, 2=Journal article, 3=Preprint,
4=Report, 5=Book, 6=Book section, 7=Thesis, 8=Patent
```

### Taxonomy System
- **Tags, Categories, Publication Types, Authors** are auto-indexed
- Theme renders tag/category archive pages automatically
- Permalink patterns: `/tag/:slug/`, `/category/:slug/`, `/publication-type/:slug/`

### Author Profiles
- **Special author:** `admin` (folder: `content/authors/admin/`)
  - Set `superuser: true` to mark site owner
  - Can have avatar: `avatar.jpg` in same folder
- Used for attribution in publications and About widget

### Featured Content
- Publications/projects with `featured: true` appear in Featured widget (`featured.md`)
- Homepage widget displays filtered content

## Key Files to Know

| Path | Purpose |
|------|---------|
| `config/_default/config.yaml` | Hugo build config, modules, markdown settings |
| `config/_default/params.yaml` | Site theme, appearance, contact info |
| `config/_default/menus.yaml` | Navigation menu structure |
| `content/home/*.md` | Homepage widgets (edit to customize homepage) |
| `content/authors/admin/_index.md` | Site owner bio, role, education, social links |
| `content/publication/*/index.md` | Academic publications (with citation metadata) |
| `content/post/*/index.md` | Blog posts |
| `content/project/*/index.md` | Portfolio projects |
| `netlify.toml` | Netlify build & deployment config |
| `theme.toml` | Theme metadata |
| `update_wowchemy.sh` | Update theme and Hugo version |

## Common Tasks

- **Change site title/description:** Edit `config/_default/config.yaml` (`title:`) and `params.yaml` (`description:`)
- **Customize homepage:** Edit `content/home/*.md` files (toggle `active:`, adjust `weight:`)
- **Update author bio:** Edit `content/authors/admin/_index.md`
- **Add publication:** Create `content/publication/my-paper/index.md` with metadata
- **Change theme colors/fonts:** Edit `config/_default/params.yaml` (`theme:`, `font:`)
- **Add navigation menu item:** Edit `config/_default/menus.yaml` → `main:` array
- **Enable multi-language:** Uncomment Chinese config in `languages.yaml`, create `content/zh/` folder

## Conventions for Modifications

1. **Homepage customization:** Modify individual `content/home/*.md` widgets, not the index
2. **New content types:** Follow existing patterns in `content/publication/`, `content/post/`, etc.
3. **Author references:** Use folder names as references (e.g., `authors: [admin, robert-ford]`)
4. **Widget ordering:** Use `weight:` field (numeric; lower = higher on page)
5. **Enabling/disabling features:** Set `active: true/false` or comment menu items
6. **Metadata in YAML:** Use exact field names from examples (theme is strict about YAML schema)

## External Resources

- [Wowchemy Documentation](https://wowchemy.com/docs/)
- [Hugo Documentation](https://gohugo.io/getting-started/)
- [YAML Syntax Reference](https://learnxinyminutes.com/docs/yaml/)
- [Update & Migration Guide](https://wowchemy.com/docs/guide/update/)
