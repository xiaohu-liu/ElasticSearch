# field datatypes

## Array Datatype
there is not dedicated `array` type in es. any field can contains 0 or multiple values by default.
however , all values in the same array must be of same datatype. for instance:
```
an array of strings: [ "one", "two" ]
an array of integers: [ 1, 2 ]
an array of arrays: [ 1, [ 2, 3 ]] which is the equivalent of [ 1, 2, 3 ]
an array of objects: [ { "name": "Mary", "age": 12 }, { "name": "John", "age": 10 }]
```
