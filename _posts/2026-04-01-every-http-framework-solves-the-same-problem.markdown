---
layout: post
title: "Every HTTP Framework Solves the Same Problem"
date: 2026-04-01 12:00:00 +0900
categories: backend
---

I've been learning backend development by comparing how different frameworks handle the same task. Today I built a simple `GET /users/:id` endpoint in three frameworks — Go's `net/http`, Express, and Spring — and found something interesting.

They all do the same thing. The difference is just how much they do for you.

## The experiment

Here's the same endpoint in three frameworks.

**Go:**

```go
http.HandleFunc("/users/", func(w http.ResponseWriter, r *http.Request) {
    if r.Method != "GET" {
        w.WriteHeader(405)
        return
    }
    id := r.URL.Path[len("/users/"):]
    json.NewEncoder(w).Encode(map[string]string{"id": id})
})
```

**Express:**

```js
app.get('/users/:id', (req, res) => {
    const user = findUser(Number(req.params.id));
    if (!user) return res.status(404).json({ error: 'Not found' });
    res.json(user);
});
```

**Spring:**

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userRepository.findById(id)
        .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
}
```

At first they look completely different. Annotations, callbacks, raw if-statements. But when I traced what actually happens at runtime, a common structure showed up.

## The four-stage pipeline

Every HTTP framework processes a request through the same four stages:

1. **Route matching** — find which handler should process this request.
2. **Request parsing** — extract parameters, convert types.
3. **Business logic** — do the actual work.
4. **Response serialization** — turn the result into JSON and send it.

That's it. No framework skips any of these steps. The only question is: who does each step?

## Who does what

**Route matching**

Go only matches the path. It doesn't care about HTTP methods at all — you check `r.Method` yourself with an if-statement. Express matches both method and path through `app.get()`. Spring scans your classes at startup, finds `@GetMapping` annotations, and registers routes automatically.

Same problem, three levels of automation.

**Request parsing**

In Go, the path parameter doesn't even exist as a concept. You slice the URL string yourself: `r.URL.Path[len("/users/"):]`. Express gives you `req.params.id`, but it's always a string — you call `Number()` yourself. Spring sees `@PathVariable Long id` and converts `"42"` to `Long 42` automatically.

Again, same problem. Go makes you do everything, Express does half, Spring does all of it.

**Response serialization**

Go: you create a JSON encoder and write to the response writer manually. Express: you call `res.json()`. Spring: you just `return` an object and the framework serializes it.

**Business logic**

This is the one row where all three frameworks are identical: you write it yourself. No framework can know that you want to find user 42 in a database. Routing, parsing, serialization — that's all plumbing. Frameworks can automate plumbing. They can't automate your business logic.

## The pattern behind the pattern

| Stage | Go | Express | Spring |
|---|---|---|---|
| Route matching | path only, manual method check | method + path | annotation scan |
| Request parsing | manual string slicing | `req.params` (string) | auto type conversion |
| Business logic | you write it | you write it | you write it |
| Response serialization | manual encoding | `res.json()` | auto on return |

The columns go from left to right: raw → convenient → automated. This maps directly to a concept called **Inversion of Control**. In Go, you control everything. In Express, you control most things but the framework handles routing and provides helpers. In Spring, the framework controls the flow — it finds your code, calls it, converts types, and serializes responses. You just declare what you want with annotations.

The trade-off is clear. More automation means less boilerplate but more magic. Go gives you nothing you didn't ask for. Spring gives you everything, whether you understand it or not.

## What I took away

When I see a new framework now, I don't try to memorize its API. I ask four questions:

1. How does it register routes?
2. How does it parse requests?
3. Where do I put my logic?
4. How does it build responses?

Every HTTP framework is just a different answer to these four questions. The pipeline is always the same.
