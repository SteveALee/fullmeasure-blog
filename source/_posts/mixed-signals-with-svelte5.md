---
title: Mixed Signals with Svlete 5
date: 2025-01-07 12:01:00
tags:
---

**Svelte 5 has changed it's reactivity architecture from compiler based to run-time signals. Here's some observations**

## Updating to Svelte 5

I took a bit of a break from web app development after crashing my bicycle 2 years ago. I knocked myself out and and developed post concussion syndrome (which I still have). I also retired from working with the W3C WAI with the plan of concentrating on a musical life with my double bass (which is progressing slowly).

I'm still coding a few personal musical companion web apps plus have a couple of larger projects using web audio. I've just started work on a local-first easy recording app for our Talking Newspapers for the Blind charity service. Svelte is my current goto framework for web apps (and probably Astro for content based sites) and they just released Svelte 5 with a new reactivity model. This change took me a little time to grok but I eventually realised that while it's quite a change from the Svelte 4 reactivity model, it's actually something I'm pretty familiar with.

It turns out that Svelte 5 uses Signals which in are an adaptation of Observables.

## Observables, Streams and Signals

Svelte 4's reactivity is compiler driven, with an impressive number of overrides for $. It's fairly easy to reason about. Apart from variables being reactive and their use in DOM updates and $: effects, stores are provided which are good old observables with an API contract similar to RXJS streams (which I have used via Circle).

Streams provide a programming model called Functional Reactive Programming which is an all encompassing but very clean model for event driven async programming. The idea is Observables (sources) generate data packets that then flow though a chain of pure functions which manipulate them until there is a terminating effect function providing real world updates (eg the DOM). A large suite of operator functions provide for very powerful manipulations. But it does require a mind shift to get to grip with the functional and data flow underpinnings.

Observables are the simpler mechanism underlying observables and are a very well established (old) model, probably originating in Smalltalk which is all about messages flowing between objects. They implement the common publish / subcribe architectural pattern. In this, something acts as a source of new data 'events' and multiple other things can subscribe in order to receive the updates when they occur. This can be viewed as a push from the observable to the subscribers or a pull in reverse, depending on the exact implementation.

In the JavaScript frontend world this ability to provide side effects based on observed data flows is called "reactivity". The DOM also uses this pattern in its event model. Firefox even had an observable element in it's now obsolete internal XUL language.

Somehow I'd missed the arrival of Signals, now used in most non React frontends for fine grained run-time reactivity with targeted updates to the DOM. I'd previously played with MobX and SolidJS with their observable based reactivity but missed the new concept name which I think first appeared in Knockout. While it's not immediately obvious from the documentation, Svelte 5 uses Signals, as I was advised on the Svelte Discord channel.

One of the pain points with using Observables that Signals addresses is the need to manually manage all the subscriptions at the points you want to use the Observable's data stream. Signals add the concept of an "effect" (sometimes called a "compute"). The idea is that a effect defines a scope where the observable subscription happens automatically whenever the observables value is referenced (at run time). This means all sideffects of responding to the observable happens inside the scope, usually a lambda function.

## Svelte Signals

It seems the Svelte Signals are rolled in a particular Sveltety way, using JavaScript Proxy objects for the observables and getters and setters to pass the reactivity in and out of effect functions, class instances and POJOs. But I've not looked to close at the details (yet}. I'm happy to have a usable conceptual model that lets me perform reliable work. And anyway, stores are still available if their reactive architectural model seems preferable.

The Svelte compiler provides targeted effect functions for updating parts of the DOM based on the svelte source code. These subscribe to observables created with the $state rune. In other words, each reference to a $props, $state or $derived in the Svelte template causes a subscription to it and the template code manipulates the dom according to the data then received.

You can also provide your own effect functions as well using the Svelte $effect rune. Ideal for integrating other non DOM effects, whether input or output. These can also be bound to DOM elements using Actions, allowing you to replace or extend the built in template effect behaviour. There is also the CreateSubscriber() helper function for more complex integration into an $effect. The Svelte docs sensibly advise not using effects when other approaches work better, but it doesn't take much more than very basic DOM updates before you need side effects to do work.

In addition, $derived runes are a sort of pure (in a functional sense) effect, more like Stream functional operators. They let a value be generated from others, and with strictly no side effects.

The Svelte compiler provides some syntatic sugar to simplify the common patterns of mapping received data to DOM updates, with additional user events. And the documentation talks about using the sugar, thus rather hiding the underlying signals.

While the declaration of an observable is explicit with $state() or $derived(), their use is implicit with no need to reference any value members. Rich stated that it was a design decision for Svelte 5 reactivity, making it "Magical but not magic". In contrast stores require a $ prefix at use so making reactive variable references explicit, which to be honest I like. I'm toying with the idea of using $ to prefix all rune variables.

As always, this best intentioned hiding can be a bit of a barrier to doing more complex things. In the worst cases causing a so called leaky abstraction. I think Svelte just avoids this mistake but the docs could clarify the Signal model for the (not so uncommon) cases when you need to understand it. Especially as now the reactivity is no long restricted to the component initialisation and can provide more "universal reactivity" across modules, possibly replacing stores.

All in all, I like the Svelte 5 reactivity model based as it is on Signals (observable, derived and effect).

## Points to Watch

Here's a quick list of possible "gotchas" I found whilst learning.

- Keep in mind the distinction between object or array variable reassignment and mutation.
- Also remember numbers and strings get passed by value, objects and arrays by reference. This can effect the upwards reactivity chain out of functions.
- ECM module imports cannot be reassigned - use an object or array and mutate it in the importing module.
- As a result of the last two, objects might be the best general type for reactive variables.
- You can have a promise as state and use the template #async support for async DOM updates. Otherwise integration of async and reactivity can require care. Stores might be easier if you are familiar with them.
- Getters and setters could be used for side effects but it's almost certainly better to keep them pure and use $effect as intended.
- When you bind a property or an attribute to a class instance with a $state member the compiler provides the getter and setter required for the reactivity to work. If you use a POJO you need to provide the getters and setters explicitly.
