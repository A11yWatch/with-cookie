## with-cookie

A package that auto binds object or class properties to cookies for persistance and easy manipulation. Great for dynamic SSR pages. So simple you do not even have to think about it. Tiny only 2kb total.

## Installation Instructions

```bash
$ yarn add with-cookie
```

## Example

```typescript
import { withCookie } from "with-cookie";

const user = withCookie({
  isLoggedIn: false,
  email: "",
  sports: ["football"],
  health: { height: "6ft", eyeColor: "blue" },
  updateName: () => {} // <-- funcs are ignored
});

user.email = "somemail@gmail.com"; // <-- cookie created

console.log(user.email); //  <-- 'somemail@gmail.com'
// RESTART application and remove setting your email above - try logging the same property
console.log(user.email); // <-- 'somemail@gmail.com' is still there

// DESTROY cookie simply just re-assign the value to "", undefined, or delete obj.key.
// setting value to undefined reverts to static defaults upon reload
user.email = "";
```

class example

```typescript
import { withCookie, getCookie } from "with-cookie";

@withCookie
class User {
  isLoggedIn: false,
  email: "",
};

const user =  new User()

user.email = "somemail@gmail.com";

// check to see if cookie exist with cookie util.
// cookies are stored formatted `${constructor.name || config.name}_{$key}`
console.log(getCookie("User_email")); // <-- 'somemail@gmail.com'
```

## Available Configuration

| param      | default          | type   | description                                                 |
| ---------- | ---------------- | ------ | ----------------------------------------------------------- |
| defaultExp | 30               | number | Optional: A default expire date for all cookies in days     |
| noCookie   | [""]             | array  | Optional: A list of property keys as strings to not store   |
| ssCookie   | ""               | string | Optional: A cookie if rendered on the server to extract     |
| name       | constructor.name | string | Optional: A keyname for cookie storage if anonymous object. |

Example adjusting configuration. Simply pass in the object as the second param.

```typescript
const User = {
  isLoggedIn: false,
  email: "",
  card: {
    number: "",
    exp: "",
    cvc: ""
  }
};

const user = withCookie(User, { defaultExp: 360 * 10, noCookie: ["card"] });
```

![example ssr](https://j.gifs.com/ZYDEZv.gif)

## About

The main purpose of this package is to control dynamic SSR pages without having to manage complicated logic. Simple just wrap your object and all of the properties are stored and retrieved as cookies. Make sure to adjust your fetch creds to prevent cookies being sent outside your domain for every resource. For more info checkout [mozilla-web-api](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters). Be careful about storing all props to cookies due to cookies being transfered per request, only store the properties you need to be dynamic.

For more help getting started take a look at the example using create-next-app with SSR dynamic pages [example-app](https://github.com/A11yWatch/with-cookie-example)

This package is actively being used on [a11ywatch](https://www.a11ywatch.com)

## More Info

Currently all cookies are created after you set your properties to a new value to keep things dynamic to the program.

## TODO

1. Add util method examples on README. Currently util methods include `setCookie` and `getCookie` which can be imported with the package. Check the `src/utils` folder for more details on usage.
2. Add option to create cookie upon instantiation.
3. Add ability to wrap key with-cookie like this for a key level cookie ex below

```typescript
const user = {
  @withCookie
  isLoggedIn: false,
  email: "",
  card: {
    number: "",
    exp: "",
    cvc: ""
  }
};

```

4. Look into adding method -> cookie purposes
