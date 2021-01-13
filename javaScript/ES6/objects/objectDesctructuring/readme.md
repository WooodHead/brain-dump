### How to Solve the Accessibility Issue

```javascript
const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
  family: {
    sister: {
      firstName: "Maria",
    },
  },
};

// We access to the nested object `sister`
// and we extract the `firstName` property
const { firstName } = user.family.sister;

console.log(firstName);
// Output: 'Maria'
```

### How to Solve the Usage Issue

Now that you know how to destructure an object, let me show you how to extract properties directly in your function parameter definition.

If you know React, you're probably already familiar with it.

```javascript
function getUserFirstName({ firstName }) {
  return firstName;
}

const user = {
  firstName: "John",
  lastName: "Doe",
  password: "123",
};

console.log(getUserFirstName(user));
// Output: 'John'
```
