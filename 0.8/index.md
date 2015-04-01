---
layout: default
type: guide
title: Polymer 0.8 Overview
subtitle: Introduction
shortname: Introduction
---

## Introducing Polymer 0.8

The 0.8 release of the Polymer core library is now out.

Consider the 0.8 release to be our proposed API for 1.0, an "alpha" release - we fully expect some breaking changes as a result of the feedback we get.

**Note:** This release is intended for early adopters who want to test out the new APIs and provide feedback. This release is optimized for performance and size, and is not yet a feature-complete replacement for 0.5. We're working hard on getting to feature parity.  See the [roadmap](#roadmap) for more detailed timelines.
{: .alert .alert-info }

** The highlights: **

* Dramatically faster startup time and runtime performance than 0.5, even in Chrome where web components are natively supported.
* Significantly smaller payload than 0.5.
* Completely refactored internally to be clearly layered and much less complex.
* Brand new data-binding system that is simpler, faster, offers tighter control, and is easier to debug.
* Brand new, lightweight shadow DOM shim called `shady DOM`, that lets you avoid the complexity, size, performance penalty, and invasiveness of the shadow DOM polyfill.
* Upper bound _and lower bound_ scoped styling, even without native shadow DOM: scoped styles don’t bleed out, and children in their own roots are protected from descendant selectors in a shadow root.

There's a lot more to 0.8 - check out the [API Documentation](TODO: Final link to 0.8 docs) for the full run-down of all the features.

## Benchmarks {#benchmarks}

As a sample of the performance difference between 0.5 and 0.8, below are the results from our `medium-list` benchmark. The benchmark instantiates and stamps out templates for a few thousand nested custom elements and binds data down through each of them. It measures time to first paint. You can poke around with this [benchmark](github.com/polymerlabs/benchmarks/) yourself - note that device specs vary wildly, so the only valid comparisons to draw from these numbers are between 0.5 and 0.8, and not across browsers.

|             | Chrome | Desktop Safari | Mobile Safari |
|-------------|--------|----------------|---------------|
| 0.5         | 1062ms | 1400ms         | 4030ms        |
| 0.8         | 317ms  | 323ms          | 684ms         |
| Improvement | 3.35x  | 4.3x           | 5.9x          |

As with all benchmarks, your mileage may vary, and please do poke around and create your own - we'd love to see them. But with this particular test it's clear that the same Polymer-based app written in 0.8 will be multiples faster to start up than one written in 0.5, across all environments.

## Roadmap {#roadmap}
The following is a high-level outline of our plans for the library toward 1.0 and beyond.

### Pre-1.0 Priorities

** Content Security Policy (CSP) Support **
In order to achieve some of the optimizations around data binding, 0.8 generates code that sugars the event listening => property change hook for property accessors. This makes data binding faster, but unfortunately breaks CSP. We believe it will be possible to get around this but it requires more work and investigation. We didn't want to delay the release of 0.8 for CSP, but it will be a high priority going forward. If your app needs to run under CSP, we’d recommend starting development outside of the CSP environment or just hanging tight until CSP support is released.

** Gestures **
We had a fairly robust gesture recognition system in 0.5 - we still have to figure out where it fits in 0.8, but plan to get it in as soon as possible. This may roll in feature-by-feature, beginning with mitigating any remaining click-delay issues on certain browsers.

** shady DOM system improvements **
The shady DOM system is new to Polymer in 0.8, and radically improves performance and decreases the size of the polyfill needed to run on browsers that don’t support shadow DOM natively. The shadow DOM polyfill was optimized for correctness, though it remained impossible to perfectly polyfill shadow DOM. Shady DOM is optimized for speed. In the 0.9 timeframe, we will explore possible ways to do better custom element interop for external frameworks not expecting the shady DOM system.

** Cross-scope styling **
The "theming problem" - you want to be able to easily style Polymer elements from the outside, but `::shadow` and `/deep/` are far too painful (we agree!). We are exploring a robust system for ergonomic cross-scope styling inspired by and based on [CSS Custom Properties](http://dev.w3.org/csswg/css-variables/) - look for this to be included around the 0.9 timeframe.

** Templating features **

* `x-autobind`, `x-repeat`, and others
We have a number of templating features in the 0.8 codebase that are still experimental - we'll keep the `x` prefix for any feature baked in the library but that we are still tinkering with. These template features in particular were useful in 0.5 - we intend to bring them to 0.8 in some fashion ASAP. Expect these `x-*` prefixed elements to be deprecated very soon in favor of standard versions.
* Support for parser-challenged elements like `<table>`, `<select>`, etc.
* Nested scope binding
* Compound binding: `<p> Dear {{ title }} {{ lastName }}, </p>`
* Exploring helpers for binding to class and style

** Binding developer tools **
We're investing heavily in tooling around Polymer, and many of the changes in 0.8 make it much more conducive to tooling. We intend to create a tool to make it easier to trace dataflow between bindings.

** Benchmark suite **
We've created some benchmarks around 0.8, but have many more that we'd like to create. We plan on releasing new benchmarks and tools for better performance measurement in general.

** Documentation **
We'll be dramatically growing our documentation around 0.8 - we're releasing it as soon as the codebase is stable and so haven't had time yet to complete a lot of the documentation that we want. Expect a full on-ramp for developers new to Polymer, a more in-depth user guide, and much more.

### 1.1 Priorities
_These haven't made the cut for the 1.0 featureset, though we are always open to feedback as well as pull requests!_

** Inheritance **
Inheritance is a great feature introduced with native Custom Elements. 0.8 supports extending native HTML elements using the `is` attribute, but doesn’t yet support extending custom elements. There are two main reasons that this has so far landed outside the 1.0 scope: 1) it is a more complex problem to solve than it was in 0.5 given some of the performance optimizations we have made, and 2) from building lots and lots of elements, we've found that you can get nearly 100% of the way using mixins and composition. That said, there are certainly some legitimate use-cases for true custom element inheritance, and we fully plan to support inheritance to the extent that it existed in 0.5.

** Build tools for cross-scope shimmed styles **
We're looking to create a tool to enable the shimming of cross-scope styles at build-time, to avoid having to shim the styles at runtime. This will always remain optional - we intend to never require a build step when working with the core library - but can be a performance optimization along with vulcanize.

## FAQ's
_We'll update this section as asked-questions become frequently-asked._

** Where did the elements go? **  
We're releasing 0.8 right away, to get feedback as soon as possible. The codebase has only been stable for a brief period of time, so we haven't yet had time to port all the previous elements to 0.8. That said, we have begun work on many of the previous elements in branches, so feel free to take a look and contribute!

We have grand plans for the element sets, and intend for them to stand on their own in terms of scope and roadmap as full-fledged "products". We'll be versioning them independently from the library, and likely keeping their documentation separate from the library documentation found on this site.  The distinction between Polymer-the-library and the elements-built-using-Polymer is an important one, and we aim to continue to reinforce this going forward.

** Will you continue to support Polymer 0.5? **  
We recognize that many projects rely on 0.5, and won’t be able to switch to 0.8 until the elements already. We’ll continue viewing and merging PR’s until the elements are ported.  We intend for 0.8 to be the new baseline though, and to work within this high-performance, production-ready mindset going forward. Any incremental 0.5 releases, if needed, will be available in a branch.

** Where and how can I give feedback on 0.8? **  
The best place for feedback is on Github, [by filing an issue](https://github.com/polymer/polymer/issues), adding to an existing one, or submitting a pull request.  Feedback and contributions will be critical to achieving a successful 1.0.  Pull requests tend to be the clearest way of expressing an idea or change, but short of a pull request, as much detail as you can add around your use-case will help us and the broader community better understand the question or suggestion.
