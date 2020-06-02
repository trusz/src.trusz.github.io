+++
title = "File Naming Convention"
date = 2020-06-01
tags = ["file", "naming", "git", ]
categories = ["General"] 
imgs = []
cover = "/images/file-naming-convention.jpg"  # image show on top
coverAuthor ="Maksym Kaharlytskyi"
coverUrl ="https://unsplash.com/@qwitka?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText"
readingTime = true  # show reading time after article date
toc = true
comments = false
justify = false  # text-align: justify;
single = false  # display as a single page, hide navigation on bottom, like as about page.
license = ""  # CC License
draft = false
+++

## Summary

I use `kebab-case` instead of `camelCase` for file names because changing lower  
case letters to capital ones have caused problems in every one of my projects.

## Details

> - `kebab-case`: all words are lowercase separated with `-` (hyphen)
> - `PascalCase`: every word starts with capital letter, no space between words
> - `camelCase`: every word starts with capital letter,
>   except the first one, no space between words

The problem I always have with `camelCase` and `PascalCase` is that that git  
does not recognize the change from lowercase to capital.

For example, let's take the TypeScript component "Shopping Cart".  

I have a file called `Shoppingcart.ts` (typo is intended) and I import it in
another file:

```ts
import * as ShoppingCart from './Shoppingcart.ts'
```

Some time later, or immediately, I recognize the typo and I would fix it:

1. rename the file to `ShoppingCart.ts`
2. fix the import:

    ```ts
    import * as ShoppingCart from './ShoppingCart.ts'
    ```

3. commit code

Anybody else who pulls the new code would get the change in the file but  
 not renaming of the file.

```ts
import * as ShoppingCart from './ShoppingCart.ts'
```

```txt
components/
└─Shoppingcart.ts
```

Most of the the time the file won't be found.

There are of course solution to this like using `git mv` or changing git and/or  
file system configurations.  

I have found it us easier to use `kebab-case` file names than to have the same  
configurations in the teams.
