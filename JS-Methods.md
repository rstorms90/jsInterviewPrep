## Filter vs Map vs Reduce vs Foreach (Array Methods)

#### Foreach

Foreach takes a callback function and run that callback function on each element of array one by one.

```
var sample = [1, 2, 3];
// es5
sample.forEach(function (elem, index){
   console.log(elem + ' comes at ' + index);
})

// es6
sample.forEach((elem, index) => `${elem} comes at ${index}`)
/*
output
1 comes at 0
2 comes at 1
3 comes at 2
*/
```

For every element on the array we are calling a callback which gets element & its index provided by foreach.
Basically forEach works as a traditional for loop looping over the array and providing you array elements to do operations on them.

#### Filter

The main difference between forEach and filter is that forEach just loops over the array and executes the callback but filter executes the callback and check its return value. If the value is true element remains in the resulting array but if the return value is false the element will be removed for the resulting array.

Filter does not update the existing array it will return a new filtered array every time.

```
var sample = [1, 2, 3];
// es5
var result = sample.filter(function(elem){
    return elem !== 2;
})
console.log(result)

// es6
var result = sample.filter(elem => elem !== 2)
/* output */
[1, 3]
```

We passed a callback to filter which got run against every element in the array. In the callback we checked if the element !== 2 if the condition fails ( when elem was equal to 1 or 3 ) include them into the resulting array else don’t include the element in the resulting array.

#### Map

Map like filter & foreach takes a callback and run it against every element on the array but what makes it unique is it generates a new array based on your existing array.

```
var sample = [1, 2, 3];
// es5
var mapped = sample.map(function(elem) {
    return elem * 10;
})

// es6
let mapped = sample.map(elem => elem * 10)
console.log(mapped);
/* output */
[10, 20, 30]
```

Map ran through every element of the array, multiplied it to 10 and returned the element which will be going to stored inside our resulting array.
Like filter, map also returns an array. The provided callback to map modifies the array elements and save them into the new array upon completion that array get returned as the mapped array.

#### Reduce

As the name already suggest reduce method of the array object is used to reduce the array to one single value.

```
var sample = [1, 2, 3];
// es5
var sum = sample.reduce(function(sum, elem){
    return sum + elem;
})

// es6
var sum = sample.reduce((sum, elem) => sum + elem)
console.log(sum)
```

Reduce takes a callback ( like every function above ). Inside this callback we get two arguments: sum & elem. The sum is the last returned value of the reduce function. For example initially the sum value will be 0 then when the callback runs on the first element it will add the elem to the sum and return that value. On second iteration the sum value will be first elem + 0, on third iteration it will be 0 + first elem + second elem.
Reduce works by reducing the array into one single value and returns it upon completion.
