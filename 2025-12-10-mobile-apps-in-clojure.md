title: Mobile apps in Clojure
categories: [clojure, BS]
---

# Building Cross-Platform Mobile Apps in Clojure with React Native

Over the past few months at work I’ve been exploring how to build mobile applications using **Clojure/ClojureScript** — not just as a side project, but as a concrete experiment to see whether we could rely on it for real mobile development. The goal was simple: write a small but non-trivial app, ship it to both **iOS and Android**, and see how the experience compares to our usual web stack.

This post is a short write-up of what I built and what I learned along the way.

---

## Why Clojure for Mobile?

We already use Clojure and ClojureScript heavily, so the idea of rewriting mobile logic in another language wasn’t very appealing. I wanted to see if we could keep the same functional architecture, the same mental model, and ideally even reuse parts of the code.

React Native felt like the obvious choice for the UI layer — native performance, shared code, and a React-style programming model that fits well with ClojureScript.

---

## React Native + ClojureScript in Practice

To get started, I used **rn-rf-shadow**, which combines:

- **React Native**
- **Re-Frame** for state management
- **Shadow CLJS** for building and hot reloading
- A sane project structure for mobile apps

Repo:
https://github.com/PEZ/rn-rf-shadow

Ths setup was crucial. It let me focus on building features instead of fighting the toolchain, and once everything was running, development felt surprisingly smooth.

---

## Thanks to Toni 🙌

A quick shoutout to my friend **Toni**, whose project helped a lot:

https://github.com/areina/elfeed-cljsrn

Seeing a real, non-trivial React Native app written in ClojureScript answered many “is this actually doable?” questions early on. I borrowed ideas around project layout, navigation, and native interop — it definitely saved me time.

---

## The App: Data Visualizations on Mobile

The app itself is an internal tool focused on **data visualizations**. Nothing fancy, but realistic enough to stress the stack:

- Fetching and transforming data
- Rendering charts and visual components
- Updating views based on user interaction
- Sharing logic between iOS and Android

This was important, because visualizations tend to surface performance issues quickly — especially on lower-end devices.

The good news: with React Native doing the heavy lifting and ClojureScript handling the logic, performance was more than acceptable for our use case.

---

## You *Do* Need Physical Devices

One thing that became very clear early on: **you really need physical phones**.

Simulators and emulators are great for quick iteration, but for this app — especially with charts and gestures — testing on real hardware was essential. Differences showed up in:

- Rendering performance
- Touch responsiveness
- Font sizing and layout
- Memory behavior

I ended up testing on both an actual Android phone and an iPhone, and I’m glad I did. Some issues simply don’t appear in simulators, and having the real devices made the experiment much more trustworthy.

---

## What Worked Well

### 🚀 Developer Experience
Once set up, Shadow CLJS + hot reloading felt excellent. Iterating on UI and data transformations was fast and enjoyable.

### 🔁 Shared Code
A single codebase for iOS and Android dramatically reduced duplication and kept behavior consistent across platforms.

### 🧠 Familiar Architecture
Using Re-Frame meant I didn’t have to relearn how to structure the app — events, effects, subscriptions all carried over nicely from the web.

---

## Rough Edges

There are still a few trade-offs:

- Some React Native libraries assume JavaScript/TypeScript and require a bit of interop.
- Native crashes still mean opening Xcode or Android Studio occasionally.
- The ecosystem is smaller than the JS-first world, so examples can be harder to find.

None of these were blockers, but they’re worth keeping in mind.

---

## Final Thoughts

This experiment convinced me that **ClojureScript + React Native** is a viable option for real mobile apps — especially if your team already lives in the Clojure ecosystem.

For internal tools, prototypes, and even production apps with shared logic and data-heavy UIs, this approach works surprisingly well. Just don’t skip testing on real devices — that part really matters.

If you’re curious about trying this yourself, I highly recommend starting with `rn-rf-shadow` and browsing Toni’s repo for inspiration.

Happy hacking 🚀
