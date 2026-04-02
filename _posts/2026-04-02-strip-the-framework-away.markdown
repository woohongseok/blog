---
layout: post
title: "Strip the Framework Away"
date: 2026-04-02 12:00:00 +0900
categories: backend
---

In my [previous post](/blog/backend/2026/04/01/every-http-framework-solves-the-same-problem.html), I compared how Go, Express, and Spring handle the same endpoint. I found a four-stage pipeline that every framework follows: route matching, request parsing, business logic, response serialization.

Today I want to go one level deeper. Not what the stages are, but what they reveal about frameworks themselves.

## Two kinds of code

Look at those four stages again. Three of them — route matching, request parsing, response serialization — are HTTP plumbing. They exist because HTTP speaks text, and your code speaks objects. Someone has to translate.

The fourth — business logic — is yours. It's the reason the server exists. Find user 42. Check if the email is taken. Calculate the total.

No framework can write that part for you, because it changes with every application. But the plumbing? That's the same every time. Parse JSON, match a URL, serialize a response. Repetitive work that never changes regardless of what you're building.

That's what a framework is. It automates the repetitive HTTP plumbing so you can focus on the part that actually matters — your business logic.

## The automation spectrum

But frameworks don't all automate the same amount. Here's what I found when I mapped out who does what:

**Go (net/http)** — route matching only. You parse the request body yourself with `json.Decoder`. You serialize the response yourself with `json.Encoder`. You check HTTP methods with an if-statement.

**Express** — route matching and response serialization. `app.get()` handles both method and path. `res.json()` serializes for you. But request parsing? You need to register `express.json()` middleware yourself. It's not on by default.

**Spring** — everything. `@GetMapping` scans and registers routes at startup. `@RequestBody` deserializes JSON into a typed object. Returning an object from a handler automatically serializes it. You write a method, add some annotations, and the framework handles the rest.

Three tools, same four stages, different levels of automation.

## Opinionated vs unopinionated

This difference has a name.

Spring is **opinionated**. It assumes you'll use JSON. It assumes you want automatic type conversion. It assumes convention over configuration. The framework makes decisions for you.

Express is **unopinionated**. It doesn't assume you want JSON parsing — maybe you're handling XML, or form data, or raw binary. It gives you the pieces and lets you assemble them.

Go's standard library isn't even a framework. It gives you the bare minimum: a way to listen for requests and send responses. Everything else is your problem.

## The trade-off

More automation means more convenience. Spring lets you write a three-line handler and get a fully working endpoint. You ship faster. You write less boilerplate.

But more automation also means less flexibility. When Spring auto-serializes your response, you don't control the JSON field order, or how nulls are handled, or whether empty arrays become `[]` or get omitted. You can configure these things, but now you're fighting the defaults instead of just writing code.

Express sits in the middle. It helps where you ask it to, stays out of the way otherwise. You get convenience where you want it and control where you need it.

Go gives you nothing you didn't ask for. Verbose, but transparent. When something breaks, there's no magic to debug — just your code.

## What I actually learned

The insight isn't that Spring is better or Go is purer. It's that the pipeline is constant:

**Route matching → Request parsing → Business logic → Response serialization.**

Every HTTP framework is just a different answer to one question: how much of that pipeline should be automated?

When I pick up a new framework now, I don't memorize syntax. I ask: where on the spectrum does this sit? What does it automate, and what does it leave to me? The answer tells me almost everything I need to know about how it will feel to work with.
