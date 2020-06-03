+++
title = "UI Components' File Structure"
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
      ├─ <component>.tsx                  # main component
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
>   - [Structure](#structure)

## Details

The following is a styleguide that I use when working on web UIs.  
Most parts are framework/library independent but usually I use the following the tech-stack:

- {{< ext-link href="https://www.typescriptlang.org" >}}
    <svg class="icon" role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>TypeScript icon</title><path d="M0 12v12h24V0H0zm19.341-.956c.61.152 1.074.423 1.501.865.221.236.549.666.575.77.008.03-1.036.73-1.668 1.123-.023.015-.115-.084-.217-.236-.31-.45-.633-.644-1.128-.678-.728-.05-1.196.331-1.192.967a.88.88 0 0 0 .102.45c.16.331.458.53 1.39.933 1.719.74 2.454 1.227 2.911 1.92.51.773.625 2.008.278 2.926-.38.998-1.325 1.676-2.655 1.9-.411.073-1.386.062-1.828-.018-.964-.172-1.878-.648-2.442-1.273-.221-.243-.652-.88-.625-.925.011-.016.11-.077.22-.141.108-.061.511-.294.892-.515l.69-.4.145.214c.202.308.643.731.91.872.766.404 1.817.347 2.335-.118a.883.883 0 0 0 .313-.72c0-.278-.035-.4-.18-.61-.186-.266-.567-.49-1.649-.96-1.238-.533-1.771-.864-2.259-1.39a3.165 3.165 0 0 1-.659-1.2c-.091-.339-.114-1.189-.042-1.531.255-1.197 1.158-2.03 2.461-2.278.423-.08 1.406-.05 1.821.053zm-5.634 1.002l.008.983H10.59v8.876H8.38v-8.876H5.258v-.964c0-.534.011-.98.026-.99.012-.016 1.913-.024 4.217-.02l4.195.012z"/></svg>
    TypeScript
  {{< /ext-link >}} (Language)
- {{< ext-link href="https://reactjs.org/" >}}
    <svg class="icon" role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>React icon</title><path d="M12 9.861A2.139 2.139 0 1 0 12 14.139 2.139 2.139 0 1 0 12 9.861zM6.008 16.255l-.472-.12C2.018 15.246 0 13.737 0 11.996s2.018-3.25 5.536-4.139l.472-.119.133.468a23.53 23.53 0 0 0 1.363 3.578l.101.213-.101.213a23.307 23.307 0 0 0-1.363 3.578l-.133.467zM5.317 8.95c-2.674.751-4.315 1.9-4.315 3.046 0 1.145 1.641 2.294 4.315 3.046a24.95 24.95 0 0 1 1.182-3.046A24.752 24.752 0 0 1 5.317 8.95zM17.992 16.255l-.133-.469a23.357 23.357 0 0 0-1.364-3.577l-.101-.213.101-.213a23.42 23.42 0 0 0 1.364-3.578l.133-.468.473.119c3.517.889 5.535 2.398 5.535 4.14s-2.018 3.25-5.535 4.139l-.473.12zm-.491-4.259c.48 1.039.877 2.06 1.182 3.046 2.675-.752 4.315-1.901 4.315-3.046 0-1.146-1.641-2.294-4.315-3.046a24.788 24.788 0 0 1-1.182 3.046zM5.31 8.945l-.133-.467C4.188 4.992 4.488 2.494 6 1.622c1.483-.856 3.864.155 6.359 2.716l.34.349-.34.349a23.552 23.552 0 0 0-2.422 2.967l-.135.193-.235.02a23.657 23.657 0 0 0-3.785.61l-.472.119zm1.896-6.63c-.268 0-.505.058-.705.173-.994.573-1.17 2.565-.485 5.253a25.122 25.122 0 0 1 3.233-.501 24.847 24.847 0 0 1 2.052-2.544c-1.56-1.519-3.037-2.381-4.095-2.381zM16.795 22.677c-.001 0-.001 0 0 0-1.425 0-3.255-1.073-5.154-3.023l-.34-.349.34-.349a23.53 23.53 0 0 0 2.421-2.968l.135-.193.234-.02a23.63 23.63 0 0 0 3.787-.609l.472-.119.134.468c.987 3.484.688 5.983-.824 6.854a2.38 2.38 0 0 1-1.205.308zm-4.096-3.381c1.56 1.519 3.037 2.381 4.095 2.381h.001c.267 0 .505-.058.704-.173.994-.573 1.171-2.566.485-5.254a25.02 25.02 0 0 1-3.234.501 24.674 24.674 0 0 1-2.051 2.545zM18.69 8.945l-.472-.119a23.479 23.479 0 0 0-3.787-.61l-.234-.02-.135-.193a23.414 23.414 0 0 0-2.421-2.967l-.34-.349.34-.349C14.135 1.778 16.515.767 18 1.622c1.512.872 1.812 3.37.824 6.855l-.134.468zM14.75 7.24c1.142.104 2.227.273 3.234.501.686-2.688.509-4.68-.485-5.253-.988-.571-2.845.304-4.8 2.208A24.849 24.849 0 0 1 14.75 7.24zM7.206 22.677A2.38 2.38 0 0 1 6 22.369c-1.512-.871-1.812-3.369-.823-6.854l.132-.468.472.119c1.155.291 2.429.496 3.785.609l.235.02.134.193a23.596 23.596 0 0 0 2.422 2.968l.34.349-.34.349c-1.898 1.95-3.728 3.023-5.151 3.023zm-1.19-6.427c-.686 2.688-.509 4.681.485 5.254.987.563 2.843-.305 4.8-2.208a24.998 24.998 0 0 1-2.052-2.545 24.976 24.976 0 0 1-3.233-.501zM12 16.878c-.823 0-1.669-.036-2.516-.106l-.235-.02-.135-.193a30.388 30.388 0 0 1-1.35-2.122 30.354 30.354 0 0 1-1.166-2.228l-.1-.213.1-.213a30.3 30.3 0 0 1 1.166-2.228c.414-.716.869-1.43 1.35-2.122l.135-.193.235-.02a29.785 29.785 0 0 1 5.033 0l.234.02.134.193a30.006 30.006 0 0 1 2.517 4.35l.101.213-.101.213a29.6 29.6 0 0 1-2.517 4.35l-.134.193-.234.02c-.847.07-1.694.106-2.517.106zm-2.197-1.084c1.48.111 2.914.111 4.395 0a29.006 29.006 0 0 0 2.196-3.798 28.585 28.585 0 0 0-2.197-3.798 29.031 29.031 0 0 0-4.394 0 28.477 28.477 0 0 0-2.197 3.798 29.114 29.114 0 0 0 2.197 3.798z"/></svg>
   ReactJS
  {{< /ext-link >}} (View Library)
- {{< ext-link href="https://storybook.js.org/" >}}
    <svg class="icon" role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>Storybook icon</title><path d="M16.34.24l-.12 2.71a.18.18 0 0 0 .29.15l1.06-.8.9.7a.18.18 0 0 0 .28-.14L18.65.1l1.33-.1a1.2 1.2 0 0 1 1.28 1.2v21.6A1.2 1.2 0 0 1 20 24l-16.1-.72a1.2 1.2 0 0 1-1.15-1.16L2 2.32a1.2 1.2 0 0 1 1.13-1.27l13.2-.83.01.02zM13.27 9.3c0 .47 3.16.24 3.59-.08 0-3.2-1.72-4.89-4.86-4.89-3.15 0-4.9 1.72-4.9 4.29 0 4.45 6 4.53 6 6.96 0 .7-.32 1.1-1.05 1.1-.96 0-1.35-.49-1.3-2.16 0-.36-3.65-.48-3.77 0-.27 4.03 2.23 5.2 5.1 5.2 2.79 0 4.97-1.49 4.97-4.18 0-4.77-6.1-4.64-6.1-7 0-.97.72-1.1 1.13-1.1.45 0 1.25.07 1.19 1.87z"/></svg>
   Storybook
  {{< /ext-link >}} (UI-Component-Lib Framework)
- {{< ext-link href="https://github.com/puppeteer/puppeteer" >}}
    <svg class="icon" role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title>Google Chrome icon</title><path d="M16.214 8.69l6.715-1.679A12.027 12.027 0 0 1 24 11.972C24 18.57 18.569 24 11.968 24c-.302 0-.605-.011-.907-.034l4.905-8.347c.356-.376.655-.803.881-1.271a5.451 5.451 0 0 0-.043-4.748 5.156 5.156 0 0 0-.59-.91zm-3.24 8.575l-2.121 6.682C4.738 23.345 0 18.14 0 11.977 0 9.592.709 7.26 2.038 5.279l4.834 8.377c.18.539 1.119 2.581 3.067 3.327.998.382 2.041.481 3.035.282zM11.973 7.62c-2.006.019-3.878 1.544-4.281 3.512a4.478 4.478 0 0 0 1.237 4.032c1.214 1.186 3.14 1.578 4.734.927 1.408-.576 2.47-1.927 2.691-3.431.272-1.856-.788-3.832-2.495-4.629a4.413 4.413 0 0 0-1.886-.411zM7.046 9.962L2.259 4.963A12.043 12.043 0 0 1 11.997 0c4.56 0 8.744 2.592 10.774 6.675H12.558c-1.811-.125-3.288.52-4.265 1.453a5.345 5.345 0 0 0-1.247 1.834z"/></svg>
   Chrome Puppeteer
  {{< /ext-link >}} (Browser Automation)

### Naming

> **Info:** More about naming here: [File Naming Convention](/posts/file-naming-convention)

### Structure

```sh
src/
└─ components/
   └─ <component>/
      ├─ index.ts                         # entrypoint
      ├─ <component>.tsx                  # main component
      ├─ <sub-component>.tsx              # any sub component
      ├─ <component>.stories.tsx          # stories about the main component
      ├─ <component>.spec.stories.tsx     # stories for testing purposes
      └─ <component>.spec.ts              # component tests
```

Components are located in `/src/components` folder.  
Each component has a folder with the name of the component (e.g.: `button/`)

In this folder there are required files:

- `<component.tsx>` is where the component lives (e.g.: `button.tsx`)  
  > **Note:** If the component gets too big, split it up into sub-components: `<sub-component>.tsx`

  ✏️ Example: `button.tsx`

  ```tsx
  import * as React from 'react'

  export function Button(props: React.PropsWithChildren<Props>) {

      const {
          onClick = noopOnClick,
          children,
      } = props;

      return (
          <button
              access-id="button"
              onClick={onClick}
            >
              {children}
          </button>
      )
  }

  interface Props {
      onClick?: () => void
  }

  function noopOnClick() { }
  ```

- `index.ts`: we think of the folder as a package and `index.ts` exports everything
  that is meant for public usage.  

  ✏️ Example: `index.ts`

  ```js
  export { default as Button } from './button'
  ```

- `<component>.stories.tsx` creates style guide stories for the component.
  Style guide stories demonstrate, among other thins, the different visual appearances.  

  ✏️ Example: `button.stories.tsx`

  ```tsx
  import * as React from 'react';
  import { Button } from "./button"

  export default {
      component: Button,
      title: 'Components|Button',
  };

  export const text = () => <Button>Hello, Button!</Button>;

  ```

- `<component>.spec.stories.tsx` contain the stories that setup components for the tests
  > **Note:** Why not `<component>.stories.spec.tsx`?  
  > Everything ending with `*.spec` are considered to be tests and would be confusing for developers  
  > See `<component>.spec.ts`

  ✏️ Example: `button.spec.stories.tsx`
  
  ```tsx
  import * as React from 'react';
  import { Button } from "./button"

  export default {
      component: Button,
      title: 'Components|Button/Test',
  };

  export const TestOnClick = () => {

      const [text, setText] = React.useState("not clicked")

      return (
          <div>
              <Button
                  onClick={() => setText("clicked")}
              >
                  Click to set text 1
                  </Button>
              <div access-id="text-target">{text}</div>
          </div>
      )
  }
  ```

- `<component>.spec.ts` contains the component tests  
  In case the spec gets too large one can split up and prefix specs e.g.:
  - `<component>.awesome-feature.spec.tsx`
  - `<component>.nasty-bugs.spec.tsx`
  - `<component>.crazy-edge-cases.spec.tsx`

  ✏️ Example: `button.spec.tsx`

  ```ts
  import { describe, it } from 'mocha'
  import { expect } from 'chai'
  import { startBrowser, baseUrl } from 'testing'

  describe('Button', () => {
      describe('click action', () => {
          const clickTests: TestCase[] = [
              {
                  desc: 'has effect',
                  expectedText: 'clicked'
              },

          ]

          clickTests.forEach(testClick)

          function testClick(tc: TestCase) {
              it(tc.desc, async () => {
                  const buttonSelector = 'button[access-id="button"]'
                  const textSelector = '[access-id="text-target"]'

                  const b = await startBrowser(0)
                  await b.open(`${baseUrl}/iframe.html?id=components-button--test-on-click`)
                  await b.click(buttonSelector)

                  const text = await b.fetchText(textSelector)
                  expect(text).to.be.equal(tc.expectedText)
              })
          }

          interface TestCase {
              desc: string,
              expectedText: string,
          }

      })
  })

  ```
