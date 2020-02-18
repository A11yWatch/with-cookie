## with-cookie

A package that auto bind properties to cookies for persistance and easy manipulation. Great for dynamic SSR pages. Tiny only 2kb total.

## Installation Instructions

```bash
$ yarn add with-cookie
```

## Example

```typescript
import { withCookie } from "with-cookie";

const User = {
  isLoggedIn: false,
  email: "",
  sports: ["football"],
  health: { height: "6ft", eyeColor: "blue" },
  someMethod: () => {} // <-- func, getters, and setters are ignored
};

const user = Object.create(withCookie(User));

user.email = "someemail@gmail.com";

console.log(user.email); //  <-- 'someemail@gmail.com'
// RESTART application and remove setting your email above - try logging the same property
console.log(user.email); // <-- 'someemail@gmail.com' is still there

// DESTROY cookie simply just re-assign the value to "", undefined, or delete obj.key.
// setting value to undefined reverts to static defaults upon reload
user.email = "";
```

class example

```typescript
import { withCookie, getCookie } from "with-cookie";

class User {
  isLoggedIn: false,
  email: "",
  someMethod: () => {}
};

const user =  withCookie(new User())

user.email = "someemail@gmail.com";

// check to see if cookie exist with cookie util.
// All cookies are stored in the format `key` + `_ck`
console.log(getCookie("email_cookie")); // <-- 'someemail@gmail.com'

```

## Available Configuration

| param      | default | type   | description                                               |
| ---------- | ------- | ------ | --------------------------------------------------------- |
| defaultExp | 30      | number | Optional: A default expire date for all cookies in days   |
| noCookie   | [""]    | array  | Optional: A list of property keys as strings to not store |
| ssCookie   | ""      | string | Optional: A cookie if rendered on the server to extract   |

Example adjusting configuration. Simply pass in the object as the second param.

```typescript
import { withCookie } from "with-cookie";

const User = {
  isLoggedIn: false,
  email: ""
};

const user = Object.create(
  withCookie(User, { defaultExp: 10, noCache: ["email"] })
);
```

![example ssr](https://j.gifs.com/ZYDEZv.gif)

## About

The main purpose of this package is to control dynamic SSR pages without having to manage complicated logic. Simple just wrap your object and all of the properties are stored and retrieved as cookies. Make sure to adjust your fetch creds to prevent cookies being sent outside your domain for every resource. For more info checkout [mozilla-web-api](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)

For more help getting started take a look at the example using create-next-app with SSR dynamic pages [example-app](https://github.com/A11yWatch/with-cookie-example)

This package is actively being used on [a11ywatch](https://www.a11ywatch.com)
