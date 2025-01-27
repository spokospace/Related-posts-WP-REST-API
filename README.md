# SPOKO Related Posts REST API

Wordpress plugin that adds related posts support through REST API with Polylang integration. Perfect for Headless WordPress setups using modern frameworks like Astro, Next.js, Nuxt, etc.

## Features

- Provides related posts via REST API endpoint
- Integrates with Polylang for multilingual support
- Returns related posts based on tags (primary) or categories (fallback)
- Includes complete post data including featured images in various sizes
- Returns relative URLs for better compatibility with different environments
- Lightweight and optimized for performance

## Requirements

- WordPress 5.0 or higher
- PHP 8.2 or higher
- Polylang plugin (for multilingual support)

## Installation

1. Download the plugin zip file
2. Upload it to your WordPress site through the WordPress plugins page
3. Activate the plugin


## Usage

The plugin adds a new REST API endpoint that returns related posts for a given post ID:

```
GET /wp-json/wp/v2/posts/{post_id}/related
```

### Example Response

```json
[
  {
    "id": 123,
    "title": {
      "rendered": "Post Title"
    },
    "slug": "post-slug",
    "link": "/blog/post-slug",
    "date": "2024-01-27T12:00:00+00:00",
    "featured_media": 456,
    "featured_image_urls": {
      "thumbnail": "...",
      "medium": "...",
      "large": "...",
      "full": "..."
    },
    "featured_image_alt": "Image Alt Text",
    "excerpt": {
      "rendered": "Post excerpt..."
    },
    "categories_data": [
      {
        "id": 789,
        "name": "Category Name",
        "slug": "category-slug",
        "description": "Category description",
        "count": 10,
        "parent": 0,
        "link": "/category/category-slug"
      }
    ]
  }
]
```

### Usage with Astro

```astro
---
const response = await fetch(`${import.meta.env.WORDPRESS_API_URL}/wp-json/wp/v2/posts/${postId}/related`);
const relatedPosts = await response.json();
---

<div class="related-posts">
  {relatedPosts.map((post) => (
    <article>
      <h2>{post.title.rendered}</h2>
      <img 
        src={post.featured_image_urls.medium} 
        alt={post.featured_image_alt} 
      />
      <Fragment set:html={post.excerpt.rendered} />
    </article>
  ))}
</div>
```

### Usage with Vue

```vue
<script setup>
const { data: relatedPosts } = await useFetch(
  `${apiUrl}/wp-json/wp/v2/posts/${postId}/related`
)
</script>

<template>
  <div class="related-posts">
    <article v-for="post in relatedPosts" :key="post.id">
      <h2 v-html="post.title.rendered"></h2>
      <img 
        :src="post.featured_image_urls.medium"
        :alt="post.featured_image_alt"
      />
      <div v-html="post.excerpt.rendered"></div>
    </article>
  </div>
</template>
```

## Configuration

The plugin has some constants that can be modified:

- `POSTS_LIMIT`: Number of related posts to return (default: 5)
- `REST_NAMESPACE`: REST API namespace (default: 'wp/v2')

## How it works

1. First tries to find related posts based on shared tags
2. If no posts are found with tags, falls back to posts in the same categories
3. Returns posts ordered by date (newest first)
4. Includes full post data including featured images and categories
5. All URLs are returned as relative paths for better compatibility

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

GPL v2 or later - see the [LICENSE](LICENSE) file for details.

## Author

Created by [spoko.space](https://spoko.space)