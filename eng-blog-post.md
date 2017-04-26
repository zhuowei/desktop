# How Four Native Developers Wrote A Javascript App

Some amount of time ago, we released the new GitHub Desktop, rewritten on Electron.

Electron is often seen as a way of enabling web developers to make desktop apps using the technologies they already know: HTML, CSS, Javascript, Node.js. Our situation was a little different.

Everyone on the GitHub Desktop team was a native developer. Three from the .NET world and one from Cocoa. We knew how to make native apps. We didn't know those web technologies.

This is our journey.

## Language

We knew from the start that we wouldn't write plain Javascript. We were coming from C#, Objective-C, and Swift where static typing lets the compiler watch our back and help us along. The question wasn't whether we'd choose a compile-to-Javascript language, but rather which one. The two contenders were Flow and TypeScript.

### Flow

Flow is a type checker from Facebook. We liked its type inference, non-nullable types, and the decoupled way in which it fit with the rest of the Javascript ecosystem. It's not a _language_ so much a type annotation grammar bolted on to Javascript. There's no "compilation." Instead, Flow type checks your code and then strips out its annotations. If you want to use ES6+ features, you'd use something like Babel to transform your code.

Unfortunately, we could eliminate Flow pretty quickly. At the time their Windows support was very poor and two of our three engineers were Windows users.

### TypeScript

TypeScript is a language from Microsoft which compiles to Javascript. It is less decoupled compared to Flow. It has a traditional compilation step which includes translating or polyfilling any ES6+ constructs down into the target. When we first started using it, it was also missing a few features compared to Flow. Most notably, it didn't support non-nullable types. Thankfully the TypeScript team was already working on it and we were [able to update](https://github.com/desktop/desktop/pull/141) not too long into the project.

We've been very happy with TypeScript. The type system is incredibly expressive, the team at Microsoft moves quickly and is responsive to the community, and the community seems to grow every day.

## User Interface

One of the earliest decisions we had to make was whether we'd try to emulate native UI or create our own UI aesthetic. We had some concerns with trying to emulate native UI:

* We'd have to do it for both macOS and Windows. That meant double the work, and it was unclear what it'd mean if we wanted to support Linux.
* It'd take a _lot_ of work and time to re-create native widgets in HTML and CSS.
* Even if we put in all that work and time, we'd still run into an uncanny valley problem where our controls looked and behaved _almost_ like native widgets, but not quite.

For all those reasons, we decided to create our own aesthetic. But that doesn't mean we neglected native UI _conventions_.

For example, on macOS, buttons are Title Case but on Windows they aren't. Or on Windows, the default button in dialogs is conventionally on the left, where on macOS it's on the right.
