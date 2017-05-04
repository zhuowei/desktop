# How Four Native Developers Wrote An Electron App

Today we released the new [GitHub Desktop](https://desktop.github.com/), rewritten on [Electron](http://electron.atom.io/).

Electron is often seen as a way of enabling web developers to make desktop apps using the technologies they already know: HTML, CSS, Javascript, Node.js. But our situation was a little different.

Everyone on the GitHub Desktop team was a native developer by trade. Three from the .NET world and one from Cocoa. We knew how to make native apps. We didn't know web technologies. Yet here we are, [just over a year later](https://github.com/desktop/desktop/commit/7012c51cc29a4ddc5d0f00d3b9763cebab509e4a). Why and how did we end up here?

## Why Rewrite?

First, the elephant in the room: why? Rewrites are rarely a good idea. Why did we decide to walk away from two native apps and rewrite?

In order to make GitHub Desktop for macOS and Windows, we had to have two teams. We had to work in two separate tech stacks. We had to implement the same features twice. We had to design the same features twice. We had to try to maintain parity across two separate code bases. And if we ever wanted to add Linux support, we'd have to do it all a third time. Building native apps didn't scale.

## Why Electron?

This isn't a new or unique problem. Over the years, we'd talked about a lot of ways to share more effort across teams: [Electron](http://electron.atom.io/), [Xamarin](https://www.xamarin.com), a shared C/C++ core, or in our wildest dreams, Haskell.

C/C++ and Haskell would let us share our core logic, but we'd still have to write platform-specific UI. Xamarin would let us share logic and potentially UI, but was a less mature platform.

We had already [dipped our toes in the water](https://githubengineering.com/cross-platform-ui-in-github-desktop/) of using web technologies to share work. The gravitational pull of the web was too strong to resist.

Javascript is everywhere. If software is eating the world, then Javascript is eating software. Companies like Google, Microsoft, and Facebook are investing _incredible_ amounts of time, money, and engineering effort on the web as a platform.

Electron lets us leverage all of that investment.

## Tradeoffs

Making native apps involved one set of tradeoffs and rewriting on Electron meant swapping out for a different set of tradeoffs.

We could now share our logic and UI across all platforms, which was fantastic. But Windows and macOS are different and their users have different expectations. We wanted to meet those expectations as much as possible, while still sharing the same UI. This manifests in a number of ways, some big and some small.

In the small, some button behaviors vary depending on your platform. On macOS, buttons are _Title Case_. On Windows they are _Sentence case_. Or on Windows, the default button in dialogs is on the left, where on macOS it's the right. We enforce the latter convention both in [code](https://github.com/desktop/desktop/blob/master/app/src/ui/lib/button-group.tsx) and with a [custom lint rule](https://github.com/desktop/desktop/blob/master/tslint-rules/buttonGroupOrderRule.ts).

On macOS, Electron gives us access to the default app menu. But on Windows, the menu is shown in the window frame, which didn't work with our design. We [worked hard](https://github.com/desktop/desktop/pull/991) to recreate a Windows-appropriate menu in web technologies, complete with access keys and focus states.

There were also times when the web platform or Electron didn't provide us with the level of OS access we needed. We [added](https://github.com/electron/electron/pull/9099) [support](https://github.com/electron/electron/pull/9242) in Electron for importing certificates into the user's certificate store, using the appropriate platform-specific APIs. The web's internationalization support doesn't provide fine-grained localization information. For example, we can't format numbers in a [locale-aware manner](https://github.com/desktop/desktop/issues/1245).
