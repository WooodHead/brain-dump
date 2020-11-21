### Reducing to a boolean

I have a file path (id) and I want to know, if the path belongs to any of the directories or files from the watching array.

```javascript
return watching.reduce((acc, curr) => {
  return acc || id.startsWith(path.join(_dirname, curr));
}, false);
```

### Converting an array of objects into a map using a specific property / key of the objects

I have an array of objects that I received from a database. But I want to convert them into a simple map for later processing. All these objects have a common structure and a key that stores a unique identifier (primary key).

```javascript
// docs array
const docs = [
  {
    id: "id-1",
    name: "K Dilkington",
    style: "orange",
  },
  {
    id: "id-2",
    name: "Lanky Fellow",
    style: "googly",
  },
];

// result
const result = {
  "id-1": {
    id: "id-1",
    name: "K Dilkington",
    style: "orange",
  },
  "id-2": {
    id: "id-2",
    name: "Lanky Fellow",
    style: "googly",
  },
};

function makeMap(docs, key) {
  return docs.reduce((map, doc) => {
    map[doc[key]] = doc;
    return map;
  }, {});
}
```

### Flatten an array of arrays

A very common case. I have an array of arrays and I want to combine them into a single array.

```javascript
function flatten(arr) {
  return arr.reduce((acc, current) => {
    return acc.concat(current);
  }, []);
}

flatten([
  ["1", "2"],
  ["3", 4],
  [{}, []],
]);
// => [ '1', '2', '3', 4, {}, [] ]
```

### Doing the job of filter() - quite unnecessary :)

From an array of players, filter those with with valid ids (mongoId here).

```javascript
game.players.reduce((acc, val) => {
  if (is.existy(val.mongoId)) {
    acc.push(val.mongoId);
  }
  return acc;
}, []);
```

### A deep Object.assign

Object.assign copies values from source objects to given object, but it does a shallow copy and also mutates the given object.

I want a function (deepAssign), that would do a deep copy and would not mutate the given object.

```javascript
const source = {
  l1: {
    inside: true,
    prop: "in",
  },
  prop: "value",
};
const target = {
  prop: "out",
  l1: {
    prop: "inisde",
  },
};

const shallow = Object.assign(source, target);

shallow = {
  l1: {
    prop: "inisde",
  },
  prop: "out",
};

const deep = deepAssign(source, target);

deep = {
  l1: {
    inside: true,
    prop: "inisde",
  },
  prop: "out",
};
function deepAssign(object, update, level = 0) {
  if (level > 5) {
    throw new Error("Deep Assign going beyound five levels");
  }

  return Object.keys(update).reduce((acc, key) => {
    const updatewith = update[key];
    if (is.not.existy(updatewith)) {
      return acc;
    }

    // lets just suppose `is` exists
    if (is.object(updatewith) && is.not.array(updatewith)) {
      acc[key] = deepAssign(object[key], updatewith, level + 1);
      return acc;
    }

    acc[key] = updatewith;
    return acc;
  }, Object.assign({}, object));
}
```

We are using recursion here and don't want to kill the stack, hence a simple check for - how many levels deep inside the source object we should care about.

### Chaining Promises

I have four async functions that have to be executed in series, feeding the result of the previous function into next.

```javascript
const arr = [fetchData, updateData, postData, showData];
const response = arr.reduce((acc, current) => {
// (cue alarm sirens) no error handling
return acc.then(current));
}, Promise.resolve(userId));

response.then(data => {
// data is final response
});
```
