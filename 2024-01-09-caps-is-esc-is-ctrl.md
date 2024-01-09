---
title: caps lock is escape is ctrl
categories: [macos]
---

Still configuring my "new" Macbook...

I'm trying to combine using a 60% keyboard, which works pretty well for its price along with my shortcut maximalism.

This leads to some collisions that I never experimented, but I'm kind of solving them with 2 things:

# Karabiner

Allows you to remap keys PER DEVICE. That's super cool, as different
keyboards have different layouts, so some remaps only make sense in a single keyboard.

I also imported a script from [this blog](https://medium.com/@pechyonkin/how-to-map-capslock-to-control-and-escape-on-mac-60523a64022b) that maps caps lock to escape when pressed alone, and to ctrl when pressed in "chord".

That's great, because the major drawback of the 60% kbd IMO is the key
to the left of "1", which is `esc, backtick, and tilde`.  The way to
make one or the other involve a tiny `fn` key in the keyboard and
maybe `shift` also.

All in all, between the usual `imap jk <esc>` and this karabiner config, I'm quite satisfied with the keyboard usage.

# Rectangle

To manage windows and sizes and screens, `rectangle` is a nice free app that provides a few useful shortcuts.

# Hammerspoon & Ratspoon

The ultimate configuration layer is Hammerspoon, which is some sort of
AutoIt for MacOS, and it's configured in Lua.

The same way I created [ratemu](https://github.com/kidd/ratemu) about
15 years ago, now it's time to create ratemu.lua.

I'm taking https://github.com/jorenvo/ratspoon as a base, and my plan
is to only add things on a need basis.
