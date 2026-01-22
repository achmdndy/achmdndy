---
title: 'The Fetishization of Clean Code'
date: '2026-03-20'
excerpt: 'We obsess over making code "clean" and "DRY" at the expense of making it readable and changeable. A look at why duplication is cheaper than the wrong abstraction.'
thumbnail: '/assets/posts/the-fetishization-of-clean-code.svg'
---

There is a specific kind of dopamine hit that comes from deleting code. It feels like cleaning a room—stripping away the clutter, organizing the chaos, and leaving behind something sleek, minimal, and "perfect." We look at a file that has been reduced from 500 lines to 50, and we feel a sense of professional accomplishment. We have tamed the beast.

But often, we haven't tamed it. We've just hidden it in the closet.

In our pursuit of "Clean Code," we frequently sacrifice clarity for brevity, and maintainability for aesthetics. We have fetishized the *look* of code over the *utility* of code. And in doing so, we often build systems that are brittle, coupled, and nightmare-ishly difficult to change.

### The Siren Song of DRY

The principle of "Don't Repeat Yourself" (DRY) is one of the first things we learn. It makes intuitive sense: if you see the same logic in two places, extract it into a function. If you see the same markup, make a component.

But DRY is a double-edged sword. Taken to its logical extreme, it leads to premature abstraction. We spot two pieces of code that *look* the same, and we immediately merge them. But we often fail to ask: do these two pieces of code change for the same reason?

If they don't, we have just created a coupling where none existed.

I once worked on a codebase where every list in the application was rendered by a single `GenericList` component. It handled user lists, product inventories, transaction histories, and notification logs. At first, it was elegant. It took an array of items and a render prop.

But then the product requirements drifted. The user list needed a "ban" button. The transaction history needed sticky headers. The notification log needed infinite scroll.

The `GenericList` component grew. It acquired props like `hasBanButton`, `isInfinite`, `stickyHeaderOffset`. Inside, it was a warzone of ternary operators and conditional hooks. To understand how the user list rendered, you had to mentally parse logic meant for the transaction history.

We had followed the rules. We hadn't repeated ourselves. But we had created a monster.

### The Cost of Indirection

"Clean" code often relies heavily on abstraction and indirection. We break big functions into small ones. We split components into sub-components. We move logic into hooks, services, and helpers.

The goal is separation of concerns, which is noble. But the result is often a fragmentation of context.

To understand a feature, you have to jump between five different files. You have to hold the state of the entire call stack in your head because the logic is smeared across the codebase. A single linear script of 200 lines is often infinitely easier to debug than ten "clean" functions of 20 lines each, scattered across the directory tree.

We forget that **locality is a virtue**. Code that changes together should live together. When we aggressively separate concerns based on technical layers (controllers, services, views) rather than feature domains, we force ourselves to context-switch constantly.

### A Plea for "Messy" Code

I am not advocating for spaghetti code. I am not saying we should copy-paste 1000 lines of logic across the app.

I am advocating for **Resilient Code**.

Sometimes, resilient code looks a bit messy. It might have some duplication. It might have a function that is "too long" by linter standards. But it is explicit. It wears its heart on its sleeve.

If you need to change the checkout flow, everything you need is *right there* in the `CheckoutFlow` component. You don't need to worry that modifying a helper function will accidentally break the `ProfileSettings` page, because they aren't sharing a premature abstraction.

### WET: Write Everything Twice

A better heuristic than rigid DRY is WET: Write Everything Twice. Or even thrice.

Don't abstract until you have three examples of duplication, and—crucially—until you have seen how those examples diverge over time. The wrong abstraction is far more expensive than duplication. Duplication is easy to fix: you delete one copy or edit both. The wrong abstraction is hard to fix: you have to untangle dependencies, migrate call sites, and handle backward compatibility.

### Conclusion

As we grow as engineers, our definition of "good code" shifts. Junior engineers often optimize for syntax: using the shortest array method, the cleverest one-liner. Mid-level engineers optimize for architecture: design patterns, strict typing, maximum reuse.

But senior engineers? We often optimize for **deletability**.

We want code that is easy to delete. Code that is isolated. Code that doesn't require a map of the entire system to modify. If that means my component has a little bit of duplicated styling or logic, so be it.

Let's stop worshiping the aesthetic of cleanliness and start valuing the reality of maintenance. A little bit of dust on the floor is better than a house of cards.
