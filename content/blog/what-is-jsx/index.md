---
slug: "what-is-jsx""
title: "What is JSX?""
date: "2018-07-09""
author: "Kent C. Dodds"
description: "_You may use it every day, but have you seen what happens after Babel transpiles it?_"
keywords: ["React","JavaScript","Jsx"]
banner: ./images/banner.jpg
bannerCredit: "Photo by [Matt Bowden](https://unsplash.com/photos/Er3WYEslTBk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) (plus the react logo)"
---

I think a critical part of understanding how to use React effectively is
understanding JavaScript and JavaScript expressions. So I’m going to show you a
few examples of JSX and it’s transpiled version to help give you an idea of how
this all works. As soon as you can transpile JSX in your head, you can use the
abstraction more powerfully.

Here’s our simplest example:

```jsx
;<div id="root">Hello world</div>
React.createElement('div', {id: 'root'}, 'Hello world')
```

As shown above, the JSX is transpiled to `React.createElement`. The API to
`React.createElement` is:

```js
function createElement(elementType, props, ...children) {}
```

- `elementType` can be a string or a function (class) for the type of element to
  be created
- `props` is an object for the props we want applied to the element (or `null`
  if we specify no props)
- `...children` is all the children we want applied to the element too. This is
  just a convenience and we could write an equivalent to above with:

```js
React.createElement('div', {id: 'root', children: 'Hello world'})
```

If you have more than one child then you use an array:

```jsx
;<div>
  <span>Hello</span> <span>World</span>
</div>
React.createElement('div', {
  children: [
    React.createElement('span', null, 'Hello'),
    ' ',
    React.createElement('span', null, 'World'),
  ],
})

// Note: babel uses the third argument for children:
React.createElement(
  'div', // type
  null, // props
  // children are the rest:
  React.createElement('span', null, 'Hello'),
  ' ',
  React.createElement('span', null, 'World'),
)
```

What you get back from a `React.createElement` call is actually a simple object:

```
// <div id="root">Hello world</div>
{
  type: "div",
  key: null,
  ref: null,
  props: { id: "root", children: "Hello world" },
  _owner: null,
  _store: {}
};
```

When you pass an object like that to `ReactDOM.render` or any other renderer,
it’s the renderer’s job to interpret that element object and create DOM nodes or
whatever else out of it. Neat right?!

Here are a few more examples for you:

```jsx
;<div>Hello {subject}</div>
React.createElement('div', null, 'Hello ', subject)

;<div>
  {greeting} {subject}
</div>
React.createElement('div', null, greeting, ' ', subject)

;<button onClick={() => {}}>click me</button>
React.createElement('button', {onClick: () => {}}, 'click me')

;<div>{error ? <span>{error}</span> : <span>good to go</span>}</div>
React.createElement(
  'div',
  null,
  error
    ? React.createElement('span', null, error)
    : React.createElement('span', null, 'good to go'),
)

;<div>
  {items.map(i => (
    <span key={i.id}>{i.content}</span>
  ))}
</div>
React.createElement(
  'div',
  null,
  items.map(i => React.createElement('span', {key: i.id}, i.content)),
)
```

Notice that whatever you put inside `{` and `}` is left alone. This is called an
interpolation and allows you to dynamically inject variables into the values of
props and children. Because of the way this works, the contents of an
interpolation must be JavaScript expressions because they’re essentially the
right hand of an object assignment or used as an argument to a function call.

### Conclusion

If you’d like to play around with this some more, you can try online with
Babel’s online REPL.
[Start here](http://babeljs.io/repl/#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=DwEwlgbgfAEgpgGwQewAQHdkCcEmAenGgG4g&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=true&fileSize=false&sourceType=module&lineWrap=false&presets=react%2Cstage-2&prettier=true&targets=&version=6.26.0&envVersion=1.6.2).
Hopefully this helps you understand a little more about how JSX works and how
you can use it more effectively. Good luck!

**Looking for a job? Looking for a developer? Check out my job board:**

[**KCD Job Board**  
\_Find developers and find jobs.\_kcd.im](http://kcd.im/jobs 'http://kcd.im/jobs')[](http://kcd.im/jobs)

**Learn more about React from me**:

- [The Beginner’s Guide to React](http://kcd.im/beginner-react)
- [Advanced React Component Patterns](http://kcd.im/advanced-react) (also on
  [Frontend Masters](https://frontendmasters.com/courses/advanced-react-patterns/)).

**Things to not miss**:

- [“Headless User Interface Components](https://medium.com/merrickchristensen/headless-user-interface-components-565b0c0f2e18) — “A
  headless user interface component is a component that offers maximum visual
  flexibility by providing no interface. “Wait for a second, are you advocating
  a user interface pattern that doesn’t have a user interface?” Yes. That is
  exactly what I’m advocating.” Brilliant article by my friend
  [Merrick Christensen](https://twitter.com/iammerrick).
- [vscode-go-to-file](https://github.com/jackfranklin/vscode-go-to-file) — A
  plugin that aims to replicate some of Vim’s “go to file” (`gf`) functionality
  by the great [Jack Franklin](https://twitter.com/Jack_Franklin)
- [tabb](http://tabb-extension.com/) — A Chrome extension to search, save, and
  manage your tabs, history, and bookmarks written in
  [Reason](https://reasonml.github.io/) by my friend
  [Ethan Godt](https://twitter.com/ethangodt)
- [deps-report](https://github.com/pichillilorenzo/deps-report) — Generate
  reports about dependencies and dependents of your JavaScript/TypeScript files
  through an AST. It supports import and require statements. By the insightful
  [Lorenzo Pichilli](https://twitter.com/LorenzoPichilli).