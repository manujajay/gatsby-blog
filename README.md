# Gatsby Blog with Netlify CMS and React

This project sets up a Gatsby blog with Netlify CMS and React. Follow the steps below to get started.

## Prerequisites

Ensure you have the following installed on your machine:

- [Node.js](https://nodejs.org/) (v14.x or later)
- [Gatsby CLI](https://www.gatsbyjs.com/docs/reference/gatsby-cli/)
- [Git](https://git-scm.com/)
- [Netlify CLI](https://docs.netlify.com/cli/get-started/)

## Getting Started

### 1. Create a new Gatsby site

Use the Gatsby CLI to create a new site.

```bash
gatsby new gatsby-blog https://github.com/gatsbyjs/gatsby-starter-blog
cd gatsby-blog
```

### 2. Install Netlify CMS

Add Netlify CMS and Gatsby plugins for Markdown and Netlify CMS.

```bash
npm install netlify-cms-app gatsby-plugin-netlify-cms gatsby-transformer-remark gatsby-source-filesystem
```

### 3. Configure Gatsby Plugins

Update your `gatsby-config.js` to include the necessary plugins.

```javascript
module.exports = {
  siteMetadata: {
    title: `Your Blog Title`,
    description: `A brief description of your blog.`,
    author: `@yourhandle`,
  },
  plugins: [
    `gatsby-plugin-react-helmet`,
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/content/blog`,
        name: `blog`,
      },
    },
    {
      resolve: `gatsby-plugin-netlify-cms`,
      options: {
        modulePath: `${__dirname}/src/cms/cms.js`, // Custom CMS configuration file
      },
    },
  ],
};
```

### 4. Create the CMS Configuration

Create a `static/admin/config.yml` file for Netlify CMS configuration.

```yaml
backend:
  name: git-gateway
  branch: main

media_folder: static/img
public_folder: /img

collections:
  - name: blog
    label: Blog
    folder: content/blog
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Publish Date", name: "date", widget: "datetime" }
      - { label: "Body", name: "body", widget: "markdown" }
```

### 5. Create a Custom CMS Preview

Create a `src/cms/cms.js` file to customize the CMS preview.

```javascript
import CMS from 'netlify-cms-app';
import { StyleSheetManager } from 'styled-components';
import BlogPostPreview from './preview-templates/BlogPostPreview';

CMS.registerPreviewTemplate('blog', (props) => (
  <StyleSheetManager>
    <BlogPostPreview {...props} />
  </StyleSheetManager>
));
```

### 6. Create a Blog Post Preview Template

Create a `src/cms/preview-templates/BlogPostPreview.js` file.

```javascript
import React from 'react';
import { BlogPostTemplate } from '../../templates/blog-post';

const BlogPostPreview = ({ entry, widgetFor }) => {
  const data = entry.getIn(['data']).toJS();

  return (
    <BlogPostTemplate
      content={widgetFor('body')}
      title={data.title}
      date={data.date}
    />
  );
};

export default BlogPostPreview;
```

### 7. Deploy to Netlify

1. Push your code to a Git repository (e.g., GitHub).
2. Connect your repository to [Netlify](https://www.netlify.com/).
3. Set up continuous deployment in Netlify.

### 8. Access Netlify CMS

Once deployed, you can access Netlify CMS at `https://your-site.netlify.app/admin`.

## Development

To start the development server, run:

```bash
gatsby develop
```

Visit `http://localhost:8000` to view your site.

## Build

To build the site for production, run:

```bash
gatsby build
```

## Deploy

To deploy your site manually, you can use the Netlify CLI:

```bash
netlify deploy
```

## Contributing

Feel free to open issues or submit pull requests if you find bugs or want to add features.

## License

This project is licensed under the MIT License.

