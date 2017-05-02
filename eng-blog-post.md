1. OMG Choices
  1. Language
  1. UX
  1. View
1. Lessons learned

# How Four Native Developers Wrote A Javascript App

Today we released the new [GitHub Desktop](https://desktop.github.com/), rewritten on [Electron](http://electron.atom.io/).

We've been working on the new GitHub Desktop for [just over a year now](https://github.com/desktop/desktop/commit/7012c51cc29a4ddc5d0f00d3b9763cebab509e4a), and we've learned some things along the way. This is the tale of how 4 native developers bumbled our way through building a Javascript app.

Electron is often seen as a way of enabling web developers to make desktop apps using the technologies they already know: HTML, CSS, Javascript, Node.js. Our situation was a little different.

Everyone on the GitHub Desktop team is a native developer by trade. Three from the .NET world and one from Cocoa. We know how to make native apps. We didn't know those web technologies.

## Choices, Choices

Our first moment of culture shock came in how many choices we had to make. If you're making a native app, your choices are pretty limited. On the macOS side, you'll use Xcode, Swift, and AppKit. On the Windows side, you'll use Visual Studio, C#, and WPF.

But in the web world, choices abound. Browserify? Webpack? Gulp? Grunt? React? Angular? SASS? CSS in JS? Some compile-to-Javascript language?

## Language

Writing plain Javascript was never a real option. We were coming from C#, Objective-C, and Swift where their static type systems mean the compiler can watch our back and help us along the way. The question wasn't _if_ we'd choose a compile-to-Javascript language, but rather which one.

At the same time, one of the big benefits to writing an Electron app is Javascript itself. It's everywhere and everyone knows it. This lowers the barrier of entry for an open source project like ours. So while languages like [Elm](http://elm-lang.org/) and [PureScript](http://purescript.org/) are really interesting and would scratch our static types itch, they were too far outside the mainstream for us to seriously consider.

Our best two options were Flow and TypeScript.

### Flow

Flow is a type checker from Facebook. It's not so much a _language_ as a type annotation grammar bolted on to Javascript. There's no "compilation." Instead, Flow type checks your code and then strips out its annotations. If you want to use ES6+ features, you'd use something like Babel to transform your code.

We liked Flow's type inference, non-nullable types, and the way in which it fits with the rest of the Javascript ecosystem. Unfortunately, we could eliminate Flow pretty quickly. At the time we kicked off the Desktop rewrite, their Windows support was very poor and two of our three engineers were Windows users.

### TypeScript

TypeScript is a language from Microsoft which compiles to Javascript. It is conceptually larger than Flow. It has a traditional compilation step which includes translating or polyfilling any ES6+ constructs down into the target as needed.

It was also missing some features compared to Flow. The one that was most disappointing was that it didn't support non-nullable types. Thankfully the TypeScript team was already working on it and we were [able to update](https://github.com/desktop/desktop/pull/141) not too long into the project.

With Flow eliminated, TypeScript was the only option left. Thankfully we've been very happy with it. The type system is incredibly expressive, the team at Microsoft moves quickly and is responsive to the community, and the community seems to grow every day. It can't remove all the warts Javascript proudly wears, but it can cover most of them with bandaids.

## React

Of all the decisions we had to make, choosing React may have been the easiest. React's mental model is far better than anything we have in native development land. It's incredibly powerful to be able to treat UI as a function of its state.

Even though we loved React's model, we fully expected to hit a performance ceiling where we'd need to do some seriously optimizations or refactoring. Happily that has yet to happen. We've done some [performance](https://github.com/desktop/desktop/pull/1281) [work](https://github.com/desktop/desktop/pull/1305), but React has yet to be the bottleneck. If or when it does, we still have [levers](https://facebook.github.io/react/docs/react-api.html#react.purecomponent) we can pull to make it faster.

## User Interface

We also had choices to make that didn't involve code. Would we try to emulate native UI or create our own aesthetic?

We had some concerns with trying to emulate native UI:

* We'd have to do it for both macOS and Windows. That meant double the work, and it was unclear what it'd mean if we wanted to support Linux.
* It'd take a _lot_ of work and time to re-create native widgets in HTML and CSS.
* Even if we put in all that work and time, we'd still run into an uncanny valley problem where our controls looked and behaved _almost_ like native widgets, but not quite.

For all those reasons, we decided to create our own aesthetic. But that doesn't mean we neglected native UI _conventions_.

For example, on macOS, buttons are _Title Case_ but on Windows they are _Sentence case_. Or on Windows, the default button in dialogs is conventionally on the left, where on macOS it's on the right.

https://github.com/desktop/desktop/pull/1315
https://github.com/desktop/desktop/blob/master/app/src/ui/lib/button-group.tsx
