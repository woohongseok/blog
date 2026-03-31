---
layout: post
title: "REST Principles: From Chaos to Structure"
date: 2026-03-31 12:00:00 +0900
categories: backend
---

## What is a Resource?

When you build an API, the URL should point to a thing, not an action. That thing is called a resource.

- `/users` → the user collection
- `/users/3` → the specific user with ID 3

You might be tempted to name your endpoints `/getUsers` or `/createUser`. I did that too. But the problem is that you're baking the action into the URL. REST says: let the URL just describe what you're talking about. The how part? That's someone else's job.

## URI Design Rules

A few conventions that keep URIs sane:

- Use nouns: `/users`, `/posts`, `/comments`
- Plural for collections: `/users` not `/user`
- Nest for relationships: `/users/3/posts` — posts that belong to user 3
- No verbs: `/users`, never `/getUsers`

Think of a URI as a street address. It tells you where something is, not what to do with it.

## HTTP Method Meaning

So if the URL doesn't say what to do, what does? The HTTP method.

- **GET** — read
- **POST** — create
- **PUT** — update
- **DELETE** — remove

They all work on the same URL:

```
GET    /users     → list all users
POST   /users     → create a user
PUT    /users/3   → update user 3
DELETE /users/3   → delete user 3
```

Here's something worth knowing: idempotency. If you accidentally send the same request twice, what happens?

- `PUT /users/3` with `{ name: "Alice" }` twice → still Alice. Same result. That's idempotent.
- `POST /users` with `{ name: "Alice" }` twice → now you have two Alices. Not idempotent.

GET, PUT, and DELETE are idempotent. POST is not. This actually matters — when the network is flaky and a request fires twice, idempotent methods are safe to retry. POST needs extra care.

## Status Code Classification

When the server responds, it sends back a number. You've seen 404 on broken web pages. That number is the status code, and it follows a simple pattern by the hundreds digit:

- **1xx** — Informational
- **2xx** — Success
- **3xx** — Redirect
- **4xx** — Client messed up
- **5xx** — Server messed up

You don't need to memorize all of them. Just a handful comes up over and over:

- **200** — all good
- **201** — all good, and something new was created (typically after POST)
- **204** — all good, but there's nothing to send back (typically after DELETE)
- **400** — bad request, something's wrong with what you sent
- **404** — that resource doesn't exist
- **500** — something broke on the server side

The point is: the client can understand the result from the number alone, before even reading the body.

## HATEOAS

This one sounds fancy. HATEOAS — Hypermedia As The Engine Of Application State.

The idea is simple: what if API responses included links, like a website does? You land on a page, you see links, you click one and go somewhere new. What if APIs worked the same way?

```json
{
  "id": 3,
  "name": "Alice",
  "links": {
    "posts": "/users/3/posts",
    "delete": "/users/3"
  }
}
```

The client wouldn't need to know URLs in advance. Just follow the links.

Sounds great, right? In practice, almost nobody does this. Frontend developers already know exactly which endpoints to hit — they hardcode those calls for each screen. Parsing links from every response just to figure out where to go next adds complexity for no real gain. Elegant idea, rarely worth it.

## Richardson Maturity Model

There's a model that grades how RESTful your API is, from Level 0 to Level 3.

**Level 0** — One URL, one method. Everything goes through a single endpoint.

```
POST /api  { "action": "getUsers" }
POST /api  { "action": "deleteUser", "id": 3 }
```

Brutal, but some APIs actually look like this.

**Level 1** — You split things into separate resources. `/users`, `/posts`. But you're still using POST for everything.

**Level 2** — Now you're using the right HTTP methods and returning proper status codes. GET for reading, POST for creating, 201 for created, 404 for not found. This is where things start feeling right.

**Level 3** — HATEOAS. Responses include links. The full REST vision.

---

Most APIs in the wild sit at Level 2, and that's perfectly fine. Level 3 is more academic than practical. When people say an API is "RESTful," they usually mean it uses resources, proper HTTP methods, and meaningful status codes.

One last thing: frameworks like Express or Spring Boot don't make your API RESTful. They give you the tools — `app.get`, `app.post`, route parameters. But nothing stops you from writing `app.get('/deleteUser', ...)`. REST is a design choice you make, not something a framework enforces.

REST is not the only way. There are alternatives like GraphQL, WebSocket, and SSE, each solving problems REST doesn't handle well.
