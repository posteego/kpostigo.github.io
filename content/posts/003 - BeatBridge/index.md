---
title: "Adding a New Feature to BeatBridge"
summary: Rethinking BeatBridge from the user's perspective
date: 2024-03-01
draft: true
tags: ["BeatBridge", "UX"]
hidemeta: false
hideFooter: true
---

## BeatBridge needs better UX for user's *receiving* music links

BeatBridge was originally designed from the perspective of the person trying to share their music link, so copying a new link to their clipboard made sense initially.

After user feedback, it was clear that I had ignored users that receive links. The best way to include those users in the app is to add a feature that would allow the user to open their preferred platform directly instead of copying a new link to their clipboard.

### Feature Implementation Idea

In order to keep track of what the user wants to do (get a new link or open their link in a new platform) I initially decided to add a boolean value to toggle the copy or open URL option with `useState()`.

I quickly discovered that the problem with this is that I would have to modify a handful of UI components to add the feature.

Another solution was to implement `onLongPress` within the `TouchableOpacity` component of each rendered item in the list of available platforms for a given music entity.

Tapping on the desired platform would open the link on the user's phone, and long pressing would copy the link to the user's phone instead.
