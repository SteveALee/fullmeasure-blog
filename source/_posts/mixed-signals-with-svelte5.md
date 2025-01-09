---
title: Mixed Signals with Svelte 5
date: 2025-01-07 12:01:00
tags:
---

**Svelte 5 has changed it's reactivity architecture from compiler based to run-time signals. Here's some observations on the intended "Magical" experience**

## Updating to Svelte 5

I took a bit of a break from web app development after crashing my bicycle 2 years ago. I knocked myself out and and developed post concussion syndrome (which I still have). I also retired from working with the W3C WAI with the plan of concentrating on a musical life with my double bass (which is progressing slowly).

I'm still coding a few personal musical companion web apps plus have a couple of larger projects using web audio. I've just started work on a local-first easy recording app for our Talking Newspapers for the Blind charity service. Svelte is my current goto framework for web apps (and probably Astro for content based sites) and they just released Svelte 5 with a new reactivity model. This change took me a little time to grok but I eventually realised that while it's quite a change from the Svelte 4 reactivity model, it's actually something I'm pretty familiar with. Even though not immediately obvious from scanning the new documentation.

It turns out that Svelte 5 uses Signals which in turn are an adaptation of Observables. The later I'm familiar with, yet I still got a bit confused with the Svelte 5 approach to reactivity at first.

## Observables, Streams and Signals

Svelte 4's reactivity is compiler driven, complete with an impressive number of overrides for $. It's fairly easy to reason about. Top level component variables are reactive and they are easy to use in the component HTML template providing targeted DOM updates. They can also be used in $: statements for side effects Plus stores are provided which are good old observables with an API contract similar to RxJS streams (which I have used in Cycle.js).

Observables are a very well established (ie old) concept, probably originating in Smalltalk which is all about messages flowing between objects. They implement the common publish / subscribe architectural pattern. In this, something acts as a source of new data 'events' and multiple other things can subscribe in order to receive the updates when they occur. This can be viewed as a push from the observable to the subscribers or a pull in reverse, depending on the exact implementation.

In the JavaScript frontend world the ability to generate side effects based on observed data changes is called "reactivity". The HTML DOM also uses this pattern in its event support. Firefox even had an observable element in it's now obsolete internal XUL language.

RxJS extends this basic concept my merging in iterables to provide for a constant stream of events. Streams provide a programming model called Functional Reactive Programming which is an all encompassing but very clean model for event driven async programming. The idea is Observables (sources) generate data packets that then flow though a chain of pure functions to manipulate and transform the data until there is a terminating effect function (sinks) providing real world updates (eg to the DOM). A large suite of operators provide for a very powerful abstraction. But it does require a mind shift to get to grip with the functional and data flow underpinnings.

Signals build on Observables by making subscription automatic. One of the pain points with using Observables and that Signals attempt to address is the need to manually manage all the subscriptions. Signals add the concept of an "effect" (sometimes called a "compute"). The idea is that an effect defines a scope where the observable subscription happens automatically whenever the observables value is referenced (at run time). This means all side effects from responding to the observable updates happens inside the scope, usually a lambda function.

Somehow I'd missed the arrival of Signals, now used in most non React front ends for fine grained run-time reactivity giving targeted updates to the DOM (React at least pretends to regenerate the entire DOM with each update). I'd previously played with MobX and SolidJS with their Observable based reactivity but missed the new concept name which I think first appeared in Knockout. While it's not immediately obvious from the documentation, Svelte 5 uses Signals, as I was advised on the Svelte Discord channel.

## Svelte Signals

The Svelte compiler provides syntactic sugar to simplify common signal use when mapping received data to DOM updates, and also DOM events. The documentation describes using this sugar, thus rather hiding the underlying Signals and their use. Richard Harris stated that it was a design decision for Svelte 5 reactivity, making it "Magical, but not magic". The Svelte 4 auto reactivity was felt to be to much magic.

However as it stands I think the way the new Signal based model is sparsely documented makes it appear to be magic.

As always, this best intentioned detail hiding can be a bit of a barrier to doing more complex things. In the worst cases this approach can cause so called leaky abstractions, where you are forced to use internal details. I think Svelte just about avoids this mistake but the docs could clarify the Signal model for the (not so uncommon) cases when you need to understand it. Especially as now the reactivity is no longer restricted to the component initialisation and can provide more "universal reactivity" across modules, possibly replacing stores.

It seems the Svelte Signals are rolled in a particular Svelty way, using JavaScript Proxy objects for the observables and getters and setters to pass changes in and out of effect functions, class instances and POJOs. But I've not looked too close into the details (yet}. I'm happy to have a usable conceptual model that lets me perform reliable work. And anyway, stores are still available if their reactive architectural model seems preferable.

The $state and $props runes create a Signal observable either for simple javascript types (numbers or strings) which can only be reassigned, or deep reactivity to mutations of compound types (object and array). Other types like Set and Map are not handled this way but Svelte does provides reactive versions of these. This seems like a reasonable design decision. You have to stop somewhere.

In addition, $derived runes, AKA "compute" in other frameworks, are a sort of pure (in a functional sense) effect, like Stream functional operators. They let a value be generated from others as they change, and with strictly no side effects.

The $bind rune provides several mechanisms to link elements and components to observables.

To update the DON the Svelte compiler generates targeted effect functions for updating small parts of the DOM based on the svelte source code. These subscribe to observables created with the $state rune. In other words, each reference to a $props, $state or $derived Observable in the Svelte template causes a subscription to it and the template code them manipulates the DOM according to the data as it is received. Neat.

You can also provide your own effect functions using the Svelte $effect rune. This is required for integrating other non DOM side effects, whether input or output. Effects can also be bound to DOM elements using Actions, allowing you to replace or extend the built in template behaviour. There is also the CreateSubscriber() helper function for more complex integration into an $effect.

The Svelte docs sensibly advise not using effects when other approaches like $derived are more appropriate , but it doesn't take much more than very basic DOM updates before you need side effects to do real work.

While the declaration of an Observable is explicit with $state() or $derived(), their use is implicit with no need to reference any get() or set() methods to access the internal value. In contrast, stores require a $ prefix at use so making reactive variable subscription explicit, which to be honest I like. I'm toying with the idea of using $ to prefix all rune variables.

All in all, I do like the Svelte 5 reactivity model based as it is on Signals with the core $state/$props, $derived and $effect runes.

## Points to Watch

Here's a short list of "gotchas" that I found whilst learning to use Svelete 5 reactivity

- Keep in mind the distinction between object or array variable reassignment and mutation.
- Also remember numbers and strings get passed by value, objects and arrays by reference. This can effect the upwards reactivity chain out of functions.
- ECM module imports cannot be reassigned - for imported runes use an object or array so you can mutate it in the importing module.
- As a result of the last two, objects might be the best general type for reactive $state variables.
- You can have a promise as state and use the template #async support for async DOM updates. Otherwise integration of async and reactivity can require care. Stores might be easier if you are familiar with them.
- Getters and setters could be used for side effects but it's almost certainly better to keep them pure and use $effect as intended.
- When you bind a property or an attribute to a class instance with a $state member the compiler provides the getter and setter required for the reactivity to work. If you use a POJO you need to provide the getters and setters explicitly.
- Closures provide flexibility in accessing runes from getters and setters in POJOs
