---
id: introduction
title: Introduction
sidebar_label: Introduction
---

Unmock is a JavaScript library for API integration testing.


Unmock focuses on three essential questions:

1. Does my code correctly compose the input and transform the output of network calls?
2. Does my code account for all the ways an external API or my network connection may behave?
3. Does my code trigger the correct passthrough integrations, like analytics, logging, etc?

Here's an example of how it works.

```javascript
import unmock, { compose, u } from "unmock";

unmock("https://www.myapi.com/users/{id}")
  .serve({
    id: u._`id`, // uses `id` from the path
    name: u`name`, // generates a fake name
    age: u.$`age`, // optionally generates a fake age
    type: 'user', // the literal word "user"
  });

test("user from backend is correct as UI object", async () => {
  stack = unmock.on();
  const { myapi } = stack;
  compose(myapi.success(), [u.int], async (id) => { /* 2 below */
    const user = await userAsUIObject(id);
    stack(expect).getOnce("https://www.example.com/api/users/");
    const { body } = myapi.response;
    stack(expect)(user).toExtend(body)); /* 1 below */
    stack(expect).postOnce("https://log.io", { id }); /* 3 below */
    stack(expect)(user.welcomeMessage).toBe(`Hello ${user.name}!`)
  });
});
```

This documentation covers these questions in three separate sections.

You'll also learn about Unmock's different methods for defining external APIs (like Stripe or another microservice in your company) and the various test reports Unmock can produce.

## Next steps

1. [Install](installation.md) unmock.
1. Define a [service](unmock.md).
1. Write a [test](expectations.md).