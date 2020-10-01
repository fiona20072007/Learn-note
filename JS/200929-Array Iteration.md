# JS Array Iteration

forEach method:

```js
fruits.forEach(fruit => console.log(fruit));
```

![my-img](img/200929-1.png)
![my-img](img/200929-2.png)

```js
names.forEach(name => {
  if (name.charAt(0) === "S") {
    sNames.push(name);
  }
});
```

![my-img](img/200929-3.png)
![my-img](img/200929-4.png)

---

filter method:

You can use filter to remove items from an array. Filter doesn't affect the original array. Instead, it returns a newArray. the first parameter of the callback will be an item from the array. The callback is simply a function that returns either true or false. If the callback returns true, filter will add that item to the new array. If it returns false, the item won't be added.

```js
const startWithS = name => name.charAt(0) === "S";
const sNames = names.filter(startWithS);
```

---

map method:

You can use map to transform each item in an array into something else.
Like filter, map returns a new array leaving the original array unchanged.
However unlike filter it returns a new array with the same number of array
elements.

```js
const numbers = strings.map(string => parseInt(string, 10));

const displayPrices = prices.map(price => `$${price.toFixed(2)}`);

abbreviatedDays = daysOfWeek.map(day => day.slice(0, 3));
```

---

reduce method:

```js
const gNameCount = names.reduce((count, name) => {
  if (name[0] === "G") {
    return count + 1;
  }
  return count;
}, 0);
```

With the reduce method, you can use the elements of an array to compute and return any value you want. The reduce method lets you turn all the items in an array into one value.

The first parameter is called an accumulator, it contains the running total of the value the reduce method returns. The second parameter, cur, in this example, short for current, represents the current array item. Whatever the callback returns becomes the accumulator for the next time in the array. If an initial value for the accumulator is not given, reduce will take the first element of the array and use it as the initial value.

```js
numberOf503 = phoneNumbers.reduce((numbers, number) => {
  if (number.startsWith("(503)")) {
    return numbers + 1;
  }
  return numbers;
}, 0);
```

---

Removing Duplicates from an Array

To remove duplicate elements from an array, we can use filter(). Remember, the filter method provides three parameters to our callback function: the current element being processed in the array, the index of the current element being processed in the array, and the array filter() was called upon.

We can compare the index of the current element to the index of the current element in the array that filter() was called upon to determine if we've already encountered that element value. This works because the indexOf method will return the index of the first occurrence that is found in the array. So, indexes of duplicate elements will not equal the index of the first occurrence of their values.

```js
const numbers = [1, 1, 2, 3, 4, 3, 5, 5, 6, 7, 3, 8, 9, 10];

const unique = numbers.filter(function(number, index, array) {
  return index === array.indexOf(number);
});

console.log(unique); // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

---

method chaining:

```js
let displayYears = years
  .filter(year => year >= 2001)
  .map(year => `${year} A.D.`);
```

```js
const usersObject = users.reduce((usersObject, user) => {
  usersObject[user.name] = user.age;
  return usersObject;
}, {});
```

```js
fullAuthorNames = authors.map(
  author => `${author.firstName} ${author.lastName}`
);

fullAuthorNames = authors.map(author => {
  return `${author.firstName} ${author.lastName}`;
});
```

```js
const users = userNames
  .filter(name => name.charAt(0) === "S")
  .map(name => ({ name }));
// .map(name => ({name: name})) for create name property, and() for js to identify it's an object
```

```js
const userNames = users.filter(user => user.age >= 30).map(user => user.name);
```

```js
unfinishedTasks = todos
  .filter(todo => todo.done === false)
  .map(todo => todo.todo);
```

```js
const product = products
  .filter(product => product.price < 10)
  .reduce((highest, product) => {
    if (highest.price > product.price) {
      return highest;
    }
    return product;
  });
```

```js
const total = products
  .filter(product => product.price > 10)
  .reduce((sum, product) => sum + product.price, 0)
  .toFixed(2);
```

```js
groceryTotal = purchaseItems
  .filter(item => item.dept === "groceries")
  .reduce((sum, item) => sum + item.price, 0);
```

Using concat to Combine Arrays

In the video we use the spread operator to combine arrays. Here's the same example using the concat array method instead:

```js
//array of arrays
const flatMovies = movies.reduce(
  (arr, innerMovies) => arr.concat(innerMovies),
  []
);
```

```js
//array of arrays
const flatMovies = movies.reduce(
  (arr, innerMovies) => [...arr, ...innerMovies],
  []
);
```

```js
const users = [
  {
    name: "Samir",
    age: 27,
    favoriteBooks: [{ title: "The Iliad" }, { title: "The Brothers Karamazov" }]
  },
  {
    name: "Angela",
    age: 33,
    favoriteBooks: [
      { title: "Tenth of December" },
      { title: "Cloud Atlas" },
      { title: "One Hundred Years of Solitude" }
    ]
  },
  {
    name: "Beatrice",
    age: 42,
    favoriteBooks: [{ title: "Candide" }]
  }
];
//array of objects of array
const books = users
  .map(user => user.favoriteBooks.map(book => book.title))
  .reduce((arr, titles) => [...arr, ...titles], []);
```
