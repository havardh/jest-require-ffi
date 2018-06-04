# Test Cases for require ffi issue in jest

jest tests fail when the second file requires `ffi` when run with `jest` or `jest --runInBank`. `jest --no-cache` passes consistently. 

## How to reproduce

### Running without `--runInBand` passes the first time, and fails on subsequent runs. 

```
$ yarn

$ yarn test
yarn run v1.7.0
$ jest
 PASS  ./m1_tests.js
 PASS  ./m2_tests.js

Test Suites: 2 passed, 2 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        3.175s
Ran all test suites.
Done in 4.80s.

$ yarn test
yarn run v1.7.0
$ jest
 PASS  ./m2_tests.js
 FAIL  ./m1_tests.js
  ● Test suite failed to run

    RangeError: Maximum call stack size exceeded

      at debug (node_modules/debug/src/debug.js:1:1)

Test Suites: 1 failed, 1 passed, 2 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.479s, estimated 2s
Ran all test suites.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

### Using `--no-cache` makes it pass without `--runInBand`

```
$ yarn test --no-cache
yarn run v1.7.0
$ jest --no-cache
 PASS  ./m1_tests.js
 PASS  ./m2_tests.js

Test Suites: 2 passed, 2 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.432s
Ran all test suites.
Done in 3.63s.
```

### Running with `--runInBand` parameter fails consistently. 

```
$ yarn test --runInBand
yarn run v1.7.0
$ jest --runInBand
 PASS  ./m2_tests.js
 FAIL  ./m1_tests.js
  ● Test suite failed to run

    RangeError: Maximum call stack size exceeded

      at debug (node_modules/debug/src/debug.js:1:1)

Test Suites: 1 failed, 1 passed, 2 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.951s, estimated 3s
Ran all test suites.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

### Running with `--no-cache --runInBand` will also fail.

```
$ yarn test --no-cache --runInBand
yarn run v1.7.0
$ jest --no-cache --runInBand
 PASS  ./m1_tests.js
 FAIL  ./m2_tests.js
  ● Test suite failed to run

    RangeError: Maximum call stack size exceeded

      at debug (node_modules/debug/src/debug.js:1:1)

Test Suites: 1 failed, 1 passed, 2 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        3.625s
Ran all test suites.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```