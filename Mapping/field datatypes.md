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
<strong>Note: </strong>
* the first value in the array determines the field type when adding a field dynamically
* all subsequent values must be of the same datatype or it must at least be possible to coerce subsequent values to the same datatype
* an array can contains null value
* an empty array [] is treated as a missing field — a field with no values
* it's not necessary to pre-configure in the document to use array, it's out of the box.
