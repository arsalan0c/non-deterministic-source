# non-deterministic-source
Non-deterministic metacircular evaluator for the [Source](https://sicp.comp.nus.edu.sg/source/) §1 programming language. <br />
Implementation is in: [evaluator.js](evaluator.js)

## Run using `parse_and_eval`

Example:

Find integers between 0 and 12 (inclusive) that are divisible by 2 or 3.

```js
parse_and_eval("function int_between(low, high) {\
                    return low > high ? amb() : amb(low, int_between(low + 1, high));\
                }\
                function is_even(x) { return (x % 2) === 0;}\
                function divisible_by_three(x) { return (x % 3) === 0;}\
                let integer = int_between(0, 12);\
                require(is_even(integer) || divisible_by_three(integer));\
                integer;");

// result: 0
```
Subsequent values can be generated by typing:
```js
try_again(); // result: 2
try_again(); // result: 3
try_again(); // result: 4
try_again(); // result: 6
try_again(); // result: 8
try_again(); // result: 9
try_again(); // result: 10
try_again(); // result: 12
try_again(); // result: null
```

### Other quick examples
```js
/* SAT solving */
parse_and_eval("\
let P = amb(true, false); \
let Q = amb(true, false); \
let R = amb(true, false); \
let S = amb(true, false); \
\
let formula = (P || !Q || R) && (!P || Q || S) && (Q || !S) && (R || S) && (P || !R); \
require(formula === true); \
\
display('Satisfying Assignment:');\
display(P, 'P:'); \
display(Q, 'Q:'); \
display(R, 'R:'); \
display(S, 'S:'); \
");

// use `try_again()` to view all satisfying assignments.
```

```js
parse_and_eval("const integer_from = n => amb(n, integer_from(n + 1)); integer_from(1);");
// result: 1
try_again(); // result: 2
try_again(); // result: 3
try_again(); // result: 4 ... and so on...
```

```js
parse_and_eval("const f = amb(1, 2, 3); const g = amb(5, 6, 7); f;");
// Result: 1, 1, 1, 2, 2, 2, 3, 3, 3 (use the try_again() function)
```

```js
parse_and_eval("const f = amb(1, 2, 3); const g = amb(5, 6, 7); g;");
// Result: 5, 6, 7, 5, 6, 7, 5, 6, 7 (use the try_again() function)
```

## Running Tests

1. Copy the code from the following files into the [playground](https://sourceacademy.nus.edu.sg/playground):
    * `evaluator.js`
    * `test.js`
    * `source-test/main.js`

2. Remove one of the two instances of the repeated function `variable_declaration_name` (from `evaluator.js` and `source-test/main.js`)

## Changes from SICP JS
This implementation of the evaluator contains several changes from that shown in the textbook. These consist of enhancements as well as bug fixes: <br />

From oldest to newest:
* Added `analyze_require` - SICP JS exercise 4.45
* Corrected evaluation of return statements [#3](https://github.com/anubh-v/non-deterministic-source/pull/3)
* Enabled evaluation of lists [#5](https://github.com/anubh-v/non-deterministic-source/pull/5)
* Enabled usage with `parse_and_eval`[#6](https://github.com/anubh-v/non-deterministic-source/pull/6)
* Added tests for deterministic and non-deterministic functionality [#8](https://github.com/anubh-v/non-deterministic-source/pull/8)
* Added logical operators [#9](https://github.com/anubh-v/non-deterministic-source/pull/9)
* Removed unused code [#10](https://github.com/anubh-v/non-deterministic-source/pull/10)
* Improved documentation of some functions [#11](https://github.com/anubh-v/non-deterministic-source/pull/11)
* Made constants immutable [#13](https://github.com/anubh-v/non-deterministic-source/pull/13)
* Made `Try Again` return a value [#16](https://github.com/anubh-v/non-deterministic-source/pull/16)
* Added the unary minus operator [#22](https://github.com/anubh-v/non-deterministic-source/pull/22)
* Fixed a bug with return statements [#26](https://github.com/anubh-v/non-deterministic-source/pull/26)
* Added memory to the driver loop [#14](https://github.com/anubh-v/non-deterministic-source/pull/14)

## Acknowledgements
This metacircular evaluator is built based on [SICP JS, Chapter 4.3](https://sicp.comp.nus.edu.sg/chapters/85)

It also uses the `parse` function of the [Source Academy](https://github.com/source-academy/js-slang)
