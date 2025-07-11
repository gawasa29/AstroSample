# API Documentation - Astro Blog Tutorial

## Table of Contents

1. [Components](#components)
2. [Layouts](#layouts)
3. [Pages](#pages)
4. [Utilities](#utilities)
5. [RSS Feed](#rss-feed)
6. [Styling](#styling)
7. [Usage Examples](#usage-examples)

---

## Components

### Footer.astro

A footer component that displays social media links.

**Props:** None

**Usage:**
```astro
---
import Footer from '../components/Footer.astro';
---
<Footer />
```

**Features:**
- Displays social media links (Twitter, GitHub, YouTube)
- Uses the Social component for link rendering
- Includes flexbox styling for horizontal layout

### Greeting.jsx

An interactive Preact component that displays random greeting messages in Japanese.

**Props:**
- `messages` (array): Array of greeting messages to cycle through

**Usage:**
```astro
---
import Greeting from '../components/Greeting';
---
<Greeting client:load messages={["Hej", "Hallo", "Hola", "Habari"]} />
```

**Features:**
- Interactive button to change greeting message
- Uses Preact hooks (useState)
- Displays Japanese thank you message
- Randomly selects from provided messages array

**State:**
- `greeting`: Current greeting message (initialized with first message from array)

**Methods:**
- `randomMessage()`: Returns a random message from the messages array

### Hamburger.astro

A mobile hamburger menu icon component.

**Props:** None

**Usage:**
```astro
---
import Hamburger from '../components/Hamburger.astro';
---
<Hamburger />
```

**Features:**
- Three-line hamburger icon
- Styled with CSS classes `.hamburger` and `.line`
- Requires JavaScript interaction (handled by menu.js script)

### Header.astro

Main site header containing navigation elements.

**Props:** None

**Usage:**
```astro
---
import Header from '../components/Header.astro';
---
<Header />
```

**Features:**
- Contains Hamburger menu for mobile
- Theme toggle icon
- Main navigation component
- Wrapped in semantic `<header>` and `<nav>` elements

### Navigation.astro

Navigation links component for the main site menu.

**Props:** None

**Usage:**
```astro
---
import Navigation from '../components/Navigation.astro';
---
<Navigation />
```

**Features:**
- Links to: Home (/), About (/about), Blog (/blog), Tags (/tags/)
- Uses `.nav-links` CSS class for styling
- Responsive design (hidden on mobile, shown on larger screens)

### Social.astro

Individual social media link component.

**Props:**
- `platform` (string): The social media platform name
- `username` (string): The username for the platform

**Usage:**
```astro
---
import Social from '../components/Social.astro';
---
<Social platform="twitter" username="astrodotbuild" />
<Social platform="github" username="withastro" />
<Social platform="youtube" username="astrodotbuild" />
```

**Features:**
- Generates platform-specific URLs
- Styled with purple background and white text
- Padding and hover effects included

### ThemeIcon.astro

Dark/light theme toggle component with SVG icons.

**Props:** None

**Usage:**
```astro
---
import ThemeIcon from '../components/ThemeIcon.astro';
---
<ThemeIcon />
```

**Features:**
- SVG icons for sun (light mode) and moon (dark mode)
- Client-side JavaScript for theme switching
- Persists theme preference in localStorage
- Respects system preference on first visit
- Toggles `.dark` class on document element

**JavaScript Functions:**
- Theme detection and initialization
- `handleToggleClick()`: Toggles theme and saves to localStorage

### BlogPost.astro

Simple blog post list item component.

**Props:**
- `title` (string): The blog post title
- `url` (string): The URL path to the blog post

**Usage:**
```astro
---
import BlogPost from '../components/BlogPost.astro';
---
<BlogPost url="/posts/my-post" title="My Blog Post" />
```

**Features:**
- Renders as a list item (`<li>`) with a link
- Used in blog listing pages

---

## Layouts

### BaseLayout.astro

Main layout template for all site pages.

**Props:**
- `pageTitle` (string): The title for the page

**Usage:**
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---
<BaseLayout pageTitle="My Page Title">
  <p>Page content goes here</p>
</BaseLayout>
```

**Features:**
- Full HTML document structure
- Japanese language attribute (`lang="ja"`)
- Includes Header and Footer components
- Imports global CSS
- Includes menu.js script
- Meta tags for charset, viewport, favicon
- Uses Astro generator meta tag

### Layout.astro

Simple base HTML layout without header/footer.

**Props:** None

**Usage:**
```astro
---
import Layout from '../layouts/Layout.astro';
---
<Layout>
  <h1>Content goes here</h1>
</Layout>
```

**Features:**
- Minimal HTML structure
- English language attribute
- Basic CSS reset
- Full width/height styling

### MarkdownPostLayout.astro

Specialized layout for markdown blog posts.

**Props:**
- `frontmatter` (object): Markdown frontmatter containing post metadata

**Expected Frontmatter Structure:**
```yaml
---
title: "Post Title"
description: "Post description"
pubDate: 2022-07-01
author: "Author Name"
image:
  url: "image-url"
  alt: "Alt text"
tags: ["tag1", "tag2"]
---
```

**Usage:**
```astro
---
layout: ../../layouts/MarkdownPostLayout.astro
title: 'My Blog Post'
# ... other frontmatter
---
Blog post content here...
```

**Features:**
- Extends BaseLayout
- Displays post metadata (description, date, author)
- Shows featured image
- Renders tags as clickable links
- Styled tag display with flexbox
- Japanese text for "Author:" (著者:)

---

## Pages

### index.astro (Home Page)

The main landing page of the site.

**Features:**
- Uses BaseLayout with "Home Page" title
- Japanese subtitle
- Interactive Greeting component with multiple language greetings
- Client-side hydration with `client:load`

### about.astro

Personal about page with dynamic content.

**Features:**
- Japanese title ("私について")
- Dynamic content using JavaScript variables and expressions
- Conditional rendering based on variables
- Skills list rendering with map function
- Personal information display

**Data Variables:**
- `identity`: Object with personal information
- `skills`: Array of technical skills
- `happy`, `finished`, `goal`: State variables for conditional content

### blog.astro

Blog listing page showing all posts.

**Features:**
- Uses `Astro.glob()` to fetch all markdown posts
- Maps through posts to render BlogPost components
- Japanese title ("私のAstro学習ブログ")

### tags/index.astro

Tag listing page showing all available tags.

**Features:**
- Extracts unique tags from all blog posts
- Uses Set to remove duplicates and flat() to flatten arrays
- Renders tags as clickable links
- Styled with tag-specific CSS

### tags/[tag].astro

Dynamic page for filtering posts by tag.

**Props:**
- `tag` (string): The current tag from URL parameters
- `posts` (array): Filtered posts for this tag

**Static Generation:**
```javascript
export async function getStaticPaths() {
  const allPosts = await Astro.glob('../posts/*.md');
  const uniqueTags = [...new Set(allPosts.map((post) => post.frontmatter.tags).flat())];
  
  return uniqueTags.map((tag) => {
    const filteredPosts = allPosts.filter((post) => post.frontmatter.tags.includes(tag));
    return {
      params: { tag },
      props: { posts: filteredPosts },
    };
  });
}
```

**Features:**
- Generates static paths for all tags
- Filters posts by tag
- Japanese text for filtered posts display

---

## Utilities

### menu.js

Mobile menu functionality script.

**Usage:**
Automatically imported in BaseLayout. No manual usage required.

**Features:**
- Adds click event listener to hamburger menu
- Toggles `.expanded` class on navigation links
- Enables mobile menu show/hide functionality

**Event Listeners:**
- `.hamburger` click → toggles `.nav-links.expanded`

---

## RSS Feed

### rss.xml.js

RSS feed generator for the blog.

**Export:**
- `GET(context)`: Async function that returns RSS feed

**Usage:**
Accessible at `/rss.xml` endpoint.

**Features:**
- Japanese language RSS feed
- Uses `@astrojs/rss` package
- Automatically includes all markdown posts
- Custom data includes Japanese language tag

**Configuration:**
- Title: "Astro学習者 | ブログ"
- Description: "Astroを学ぶ旅"
- Language: Japanese (ja-jp)

---

## Styling

### Global CSS (global.css)

**Variables and Base Styles:**
- Background: Light gray (`#f1f5f9`)
- Font: Sans-serif
- Max width: 80 characters
- Responsive design with mobile-first approach

**Navigation Styles:**
- Hamburger menu styling with orange background (`#ff9776`)
- Mobile-hidden navigation that shows on larger screens
- Responsive breakpoint at 636px

**Dark Theme Support:**
- Dark background (`#0d0950`) when `.dark` class is present
- White text in dark mode
- Theme-aware navigation styling

**Responsive Breakpoints:**
- Mobile: < 636px (hamburger menu visible)
- Desktop: ≥ 636px (full navigation visible)

---

## Usage Examples

### Creating a New Blog Post

1. Create a new markdown file in `src/pages/posts/`:
```markdown
---
layout: ../../layouts/MarkdownPostLayout.astro
title: 'My New Post'
pubDate: 2024-01-01
description: 'Post description'
author: 'Author Name'
image:
    url: 'https://example.com/image.jpg'
    alt: 'Image description'
tags: ["astro", "tutorial"]
---

# My New Post

Content goes here...
```

### Adding a New Page

1. Create a new `.astro` file in `src/pages/`:
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
const pageTitle = "New Page";
---
<BaseLayout pageTitle={pageTitle}>
  <h2>Page content</h2>
  <p>Your content here...</p>
</BaseLayout>
```

### Creating a Custom Component

1. Create a new component in `src/components/`:
```astro
---
const { title, content } = Astro.props;
---
<div class="custom-component">
  <h3>{title}</h3>
  <p>{content}</p>
</div>

<style>
  .custom-component {
    border: 1px solid #ccc;
    padding: 1rem;
    margin: 1rem 0;
  }
</style>
```

2. Use it in any page or layout:
```astro
---
import CustomComponent from '../components/CustomComponent.astro';
---
<CustomComponent title="Hello" content="World" />
```

### Theme Integration

The theme system automatically handles:
- System preference detection
- localStorage persistence
- CSS class toggling
- Icon state management

To add theme-aware styles:
```css
/* Light theme styles */
.my-element {
  background: white;
  color: black;
}

/* Dark theme styles */
:global(.dark) .my-element {
  background: #333;
  color: white;
}
```

---

## Development Commands

- `npm run dev`: Start development server
- `npm run build`: Build for production
- `npm run preview`: Preview production build

## Dependencies

- **Astro**: Static site generator
- **Preact**: React alternative for components
- **@astrojs/preact**: Astro integration for Preact
- **@astrojs/rss**: RSS feed generation

This documentation covers all public APIs, components, and utilities in the Astro blog project. Each component is designed to be reusable and follows Astro best practices for static site generation.