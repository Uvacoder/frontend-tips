---
title: Transform values from a JSON representation
category: Tip
date: 2021-04-08 10:12:00 +7
layout: layouts/post.njk
topics: JavaScript
---

When transforming a given variable to the JSON representation, we might see unexpected results if the variable contains an unserializable property.

Let's consider a simple object that represents the information of a person. It has the `phones` property that is a set of different phone numbers.

```js
const person = {
    name: 'John Doe',
    ages: 42,
    phones: new Set(['123', '456', '789']),
};
```

The set is transformed to an empty object which is unexpected output:

```js
JSON.stringify(person);

// "{"name":"John Doe","ages":42,"phones":{}}"
```

We can solve the issue by passing the second parameter to the `stringify` function. It can be a function of two parameters representing the key and value of the current iterated item.

```js
JSON.stringify(variable, (key, value) => {
    // The return value will be used in final output

    // If the function returns `undefined`,
    // the item will be excluded from the output
    return ...;
});
```

Here is how the phone numbers are transformed when we see the `phones` key:

```js
JSON.stringify(person, (key, value) => (key === 'phones' ? [...value.values()] : value));

// "{"name":"John Doe","ages":42,"phones":["123","456","789"]}"
```

Want to transform all `Set` values? No problem!

```js
JSON.stringify(person, (key, value) => (value instanceof Set ? [...value.values()] : value));
```

## See also

-   [Log the full object in NodeJS](/log-the-full-object-in-nodejs)
-   [Pick given properties from a JSON representation](/pick-given-properties-from-a-json-representation)
-   [Pretty format JSON](/pretty-format-json)
