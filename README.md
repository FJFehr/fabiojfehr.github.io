# Personal Website

Personal portfolio and blog hosted on GitHub Pages.

## Quick Edits

**Change site content (no code needed):**
- **Profile bio** - Edit `content/site/bio.md`
- **Site name/role** - Edit `content/site/config.yaml` (site section)
- **Navigation links** - Edit `content/site/config.yaml` (layout section)
- **Social links** - Edit `content/site/config.yaml` (contacts section)
- **Publications** - Edit `content/publications/publications.yaml`
- **Timeline** - Edit `content/timeline/timeline.yaml`
- **Media highlights** - Edit `content/media/media.yaml`

**Change appearance:**
- **Colours, fonts, spacing** - Edit `content/site/config.yaml` (design section)
- **Advanced styling** - Edit `css/style.css` (CSS variables at top)

**Add a blog post:**

1. **Create a markdown file** in `blogs/posts/` with YAML frontmatter:
   ```markdown
   ---
   title: 'My Awesome Blog Post'
   date: 2025-01-15
   excerpt: "A brief description for social media previews (WhatsApp, LinkedIn, etc.)"
   thumbnail: "blogs/images/my-image.jpg"
   ---
   
   ## Introduction
   
   Your blog content here in markdown format...
   
   ## Main Section
   
   More content...
   ```

2. **Add images** (optional): Place images in `blogs/images/` and reference them in your markdown:
   ```markdown
   ![Alt text](images/my-image.jpg)
   ```

3. **Convert and generate** (required for each blog):
   ```bash
   python3 convert-blog.py blogs/posts/your-post.md --update-index
   ```
   
   **Why generate HTML?** Each blog gets its own standalone HTML file with:
   - ✅ Embedded content (no separate JSON files needed)
   - ✅ Open Graph tags for rich social media previews
   - ✅ Clean, shareable URL
   
   This creates:
   - `blogs/your-post.html` - Standalone shareable page
   - Updates `blogs/blogs.yaml` - Blog index for homepage

4. **Preview locally**: Open `blogs/your-post.html` in your browser

5. **Commit and push**:
   ```bash
   git add .
   git commit -m "Add blog post: My Awesome Blog Post"
   git push
   ```

6. **Share**: Your blog will be live at:
   - Main site: `https://fjfehr.github.io/`
   - Blog post: `https://fjfehr.github.io/blogs/your-post.html` ✨
   
   When shared on WhatsApp, LinkedIn, or Facebook, it will show a nice preview with your title, excerpt, and thumbnail image!

## View Locally
Open `index.html` in a browser or use Live Server in VS Code.

## Structure

**Design Philosophy:** All site content (publications, timeline, media, config) lives in the `content/` directory for easy organisation. **Exception:** Blogs are in the root-level `blogs/` directory—this was a deliberate choice to create cleaner, more shareable URLs (`fjfehr.github.io/blogs/post-name.html` instead of `fjfehr.github.io/content/blogs/post-name.html`).

```
index.html          # Main homepage
blogs/              # Blog content (separate for cleaner URLs!)
  posts/            # ← Your markdown source files
    *.md
  images/           # ← Blog images
  *.html            # ← Generated shareable blog pages (each has embedded content & OG tags)
  blogs.yaml        # Blog index (auto-updated)
  _blog_template.html  # Template for generating HTML
css/
  style.css         # Consolidated styles with CSS variables
js/
  main.js           # Consolidated JavaScript (handles routing, theme, content loading)
content/            # All other site content lives here
  site/
    config.yaml     # Site configuration (name, layout, contacts, design)
    bio.md          # Profile bio content
    profile_picture.jpg
    icons/          # Social media icons
  publications/
    publications.yaml  # Publications list (YAML)
    files/          # Publication PDFs, thumbnails, posters
  timeline/
    timeline.yaml   # Timeline events (YAML)
    logos/          # Timeline event logos
  media/
    media.yaml      # Media highlights (YAML)
convert-blog.py     # Script to convert markdown → HTML with embedded content
```

## Configuration

All site configuration lives in **`content/site/config.yaml`** - a single, human-readable file with comments explaining each setting.

**What's in config.yaml:**
- **site** - Name, role, affiliation, footer text
- **layout** - Navigation menu, section titles, footer content  
- **contacts** - Social media links with icons (emoji or SVG)
- **design** - Complete design system (colours, typography, spacing, sizes)

**Why YAML?**
- Human-readable and easy to edit (unlike JSON)
- Comments allowed to explain settings
- Single source of truth
- Clean structure with indentation

## Content Files

All site content (except blogs) is stored in the `content/` directory in YAML format for easy editing:

- **`content/site/config.yaml`** - Site configuration, layout, contacts, and design system
- **`content/site/bio.md`** - Profile bio content in markdown
- **`content/publications/publications.yaml`** - Publication list with links to papers, code, posters
- **`content/timeline/timeline.yaml`** - Work experience and education timeline
- **`content/media/media.yaml`** - YouTube videos and media highlights

**Why is blog content separate?** Blogs live in `blogs/` (not `content/blogs/`) to create cleaner, more shareable URLs. Each blog post gets its own HTML file with embedded content and Open Graph tags for better social media sharing.

Each YAML file has clear structure with comments. Just edit the fields directly - no code knowledge needed.

## Blog Converter

The `convert-blog.py` script converts markdown blog posts to standalone HTML files with embedded content and social media meta tags.

**Why generate HTML for each blog?**
- **Better sharing:** Each blog has its own URL with Open Graph tags for rich previews on WhatsApp, LinkedIn, Facebook, etc.
- **Cleaner URLs:** `fjfehr.github.io/blogs/post-name.html` instead of query parameters
- **No build tools:** Pure client-side rendering without Jekyll or other static site generators
- **Self-contained:** Each HTML file embeds its own content (no separate JSON files)

**Commands:**
- `python3 convert-blog.py <file.md>` - Convert single file to HTML
- `python3 convert-blog.py <file.md> --update-index` - Convert and update blogs.yaml index
- `python3 convert-blog.py --convert-all` - Convert all markdown files in blogs/posts/

**What it does:**
- Extracts YAML frontmatter (title, date, excerpt, thumbnail)
- Generates unique blog ID from title
- Creates standalone HTML file with:
  - Open Graph meta tags (og:title, og:description, og:image, etc.)
  - Embedded blog content (window.BLOG_POST_DATA)
  - Proper relative paths for CSS/JS
- Updates `blogs/blogs.yaml` index for homepage display

## Architecture Philosophy

This site uses a **single-file structure** for CSS and JavaScript to optimise for:
- **Agentic Editing** - AI agents can easily understand and modify the entire codebase in one context
- **Maintainability** - All related code is in one place with clear section markers
- **Performance** - Single HTTP request for CSS/JS, better browser caching
- **Simplicity** - No build tools, bundlers, or complex dependency chains

Both `css/style.css` and `js/main.js` are heavily commented with section headers to make navigation easy for both humans and AI.

## Code Organisation

### CSS (`css/style.css`)
Single consolidated stylesheet (680 lines) organised into clearly marked sections:
- **CSS Variables** - Theme colours, spacing, sizing (supports light/dark modes)
- **Base Styles** - Typography, links, body defaults
- **Layout Components** - Header, navigation, profile, sections
- **Feature Components** - Timeline, media grid, publications, blog posts
- **Responsive Design** - Mobile-friendly media queries

**Why single file?**
- Easy to search and edit (Ctrl+F finds everything)
- All theme colours in one place (CSS variables at top)
- AI agents can analyse entire stylesheet in one pass
- No duplicate styles across multiple files
- Eliminates need for CSS preprocessors or build tools

**Quick changes:**
Edit CSS variables at the top of `style.css` to change colours, spacing, sizing, or typography.

### JavaScript (`js/main.js`)
Single consolidated JavaScript file (847 lines) wrapped in IIFE for proper scoping:

**Structure:**
1. **Configuration Loading** - YAML config loader with caching
2. **Utility Functions** - Reusable helpers (date formatting, markdown parsing)
3. **Timeline Functions** - Load and render timeline events
4. **Media Functions** - Load and embed YouTube videos
5. **Main Page Functions** - Content loaders for homepage (publications, blogs, profile)
6. **Blog Page Functions** - Blog post loader, markdown parser, TOC generator
7. **Page Detection** - Auto-initialises correct page on load

**Why single file?**
- All JavaScript logic in one place (no hunting across files)
- Utility functions easily shared across features
- Clear section headers make navigation trivial
- IIFE prevents global scope pollution
- Perfect for AI agents to understand entire codebase at once
- Single HTTP request = faster page loads

**Key Features:**
- Theme toggle with localStorage persistence
- Dynamic content loading from YAML files
- Markdown-to-HTML conversion
- Responsive navigation
- Blog post rendering with auto-generated table of contents

## Development Notes

- All CSS is in `css/style.css` with clear `/* ====== SECTION NAME ====== */` markers
- All JavaScript is in `js/main.js` with clear `/** === SECTION NAME === */` markers
- Both files use consistent indentation and extensive inline comments
- No build step required - edit and reload
- Search for keywords to find relevant sections instantly (Ctrl+F)
- Use your editor's outline/symbol view to navigate sections
- CSS variables at top of style.css for quick theme changes
- All content stored in YAML - no code changes needed for updates

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**TL;DR:**
- ✅ Free to use as a template for your own website
- ✅ Code, design, and structure are open source
- ❌ Personal content (bio, photos, blog posts, publications) is NOT included
- 🔄 You MUST replace all content in `content/` with your own

The template is designed to make this easy - just edit the YAML files and swap in your content!
