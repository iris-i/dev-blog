---
title: 'How to add UniformCSS to NextJS'
status: 'published'
author:
  name: 'Iris Ibekwe'
  picture: 'https://avatars.githubusercontent.com/u/5355315?v=4'
slug: 'how-to-add-uniform-css-to-nextjs'
description: ''
coverImage: ''
publishedAt: '2022-12-21T00:18:37.485Z'
---

An Introduction to Uniform CSS.

### Dependencies

- An existing NextJS app. Follow [the NextJS docs](https://nextjs.org/docs/getting-started) to get started.

To add UniformCSS to an existing NextJS app:

Install the package and dart-sass:

```javascript
# Install Uniform and its dependencies 
npm install sass --save-dev
npm install uniformcss
```



In your next.config.js file, load sass files by their filenames and add a path to the uniform package:

```javascript
const nextConfig = {
  sassOptions: {
    // Load sass files by @import filename.scss instead of typing the full paths.
    includePaths: [
      path.join(__dirname, 'styles'),
      path.resolve(__dirname, 'node_modules/uniformcss')
    ],
  },
}
```

Change the styles/global.css file to a sass file: global.scss.

Pull the uniform package into the global.scss file.

```javascript
@use "uniform" as *;
```

You can add extra uniform config options to the file. For example the code below adds extra custom colors to your theme.

```javascript
@use "uniform" as * with (
  $config: (
    colors: (
      custom-color-1: red,
      custom-color-2: blue
    ),
  )
);
```

Test the custom colors you added by applying it to a selector using the uniform [fill function](https://uniformcss.com/docs/api-functions/#fill-functions)

```css
.element {
  background-color: fill(custom-color-1);
}
```



I'm excited abut this framework because it brings easy, configurable utility classes that work seamlessly with dart-sass. You can choose to only generate CSS utility classes, or apply your settings to sass.

