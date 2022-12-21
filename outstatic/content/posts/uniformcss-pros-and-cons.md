---
title: 'Pros and Cons of using UniformCSS in NextJS'
status: 'draft'
author:
  name: 'Iris Ibekwe'
  picture: 'https://avatars.githubusercontent.com/u/5355315?v=4'
slug: 'uniformcss-pros-and-cons'
description: ''
coverImage: ''
publishedAt: '2022-12-21T11:17:23.142Z'
---

Pros:

- It's easy to create a set of utility classes.

- You can add a prefix to generated classes eg. u-color-primary.

- Supports Sass modules.

- If you don't want to generate classes, you can still use your preset settings with helper functions by using it in Headless mode.

- You can select which CSS properties to generate classes for.

Cons:

- Poorly documented.

- Unlike Tailwind, unused classes are not removed by default. You can add PurgeCSS to your Next JS postcss.config.js file to do this. Link: [https://purgecss.com/guides/next.html#customize-postcss-configuration-next-js-9-3](https://purgecss.com/guides/next.html#customize-postcss-configuration-next-js-9-3)

