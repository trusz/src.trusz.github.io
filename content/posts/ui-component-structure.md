+++
title = "UI Component Structure"
date = 2020-06-02
tags = ["ui", "react", "component", "structure"]
categories = ["UI Development"] 
imgs = []
cover = "/images/ui-component-structure.jpg"  # image show on top
coverAuthor = "Alvaro Pinot"
coverUrl = "https://unsplash.com/@alvaropinot?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText"
readingTime = true  # show reading time after article date
toc = true
comments = false
justify = false  # text-align: justify;
single = false  # display as a single page, hide navigation on bottom, like as about page.
license = ""  # CC License
draft = false
+++

## Summary

Basic component file structure:

```sh
src/
└─ components/
   └─ <component>/
      ├─ index.ts                         # entrypoint
      ├─ <componentt>.tsx                 # main component
      ├─ <sub-component>.tsx              # any sub component
      ├─ <component>.stories.tsx          # stories about the main component
      ├─ <component>.spec.stories.tsx     # stories for testing purposes
      └─ <component>.spec.ts              # component tests
```

> ###### Table of Contents
>
> - [Summary](#summary)
> - [Details](#details)
>   - [Naming](#naming)

## Details

The following is a styleguide that I use when working on web UIs.  
Most parts are framework/library independent but usually I use the following the tech-stack:

- <a href="https://www.typescriptlang.org" target="_blank">↗ TypeScript</a> (Language)
- <a href="https://reactjs.org/" target="_blank">↗ ReactJS</a> (View Library)
- <a href="https://storybook.js.org/" target="_blank">↗ Storybook</a> (UI-Component-Lib Framework)
- <a href="https://github.com/puppeteer/puppeteer" target="_blank">↗ Chrome Puppeteer</a> (Browser Automation)

### Naming

- hyphen case
