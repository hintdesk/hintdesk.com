---
title: "How to integrate Tailwind CSS with Blazor?"
date: 2022-03-11 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: tailwindcss
tags: tailwindcss blazor
---

## Intro
In this post I would like to document how I integrate Tailwind CSS with Blazor project.

## Steps

- Create a Blazor server project.
- Start a new **npm** project at the root of the Blazor project.

```terminal
npm init
```

This creates a new package.json file.

{%
    include image.html
    year='2022'
    month='03'
    file='11_001.png'
    alt='NPM init'
%}

- Add Tailwind CSS and its dependencies packages.

```terminal
npm install tailwindcss postcss autoprefixer postcss-cli
```

- Create a new **postcss.config.js** file at the root of Blazor project with following content.

```terminal
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

- Add a new Tailwind CSS configuration file.

```terminal
npx tailwindcss init
```

- Go to the **wwwroot** folder, add new file **app.css** with following content.

```terminal
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- Edit the **package.json** to create the command **buildcss** for generating **app.min.css** from **app.css**.

```terminal
{
  "name": "hintdesk.clipboard.web",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "buildcss": "postcss wwwroot/css/app.css -o wwwroot/css/app.min.css",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "autoprefixer": "^10.4.2",
    "postcss": "^8.4.8",
    "postcss-cli": "^9.1.0",
    "tailwindcss": "^3.0.23"
  }
}
```

- Run **buildcss** command to test. A new file **app.min.css** must be created at the **wwwroot/css** folder.

```terminal
npm run buildcss
```

{%
    include image.html
    year='2022'
    month='03'
    file='11_002.png'
    alt='app.min.css'
%}

- Edit **tailwind.config.js** to purge the generated **app.min.css** so that it keeps only the css classes which are used in code.

```terminal
module.exports = {
  content: [
    './**/*.html',
    './**/*.razor'
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

- Add the build command to **Post-build event** of the web project.

```terminal
npm run buildcss
```

{%
    include image.html
    year='2022'
    month='03'
    file='11_003.png'
    alt='Post-build event'
%}

- Now edit **_Layout.cshtml** file, add reference to **app.min.css**

```terminal
<link href="css/app.min.css" rel="stylesheet" />
```