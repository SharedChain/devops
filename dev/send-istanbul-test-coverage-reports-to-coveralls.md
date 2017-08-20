# Send Mocha and Istanbul test coverage reports to Coveralls

## Install coveralls

```
npm install coveralls --save-dev
```

## Add Coverall to test process

Update the project's `package.json`.

```
 "script": {
     "test": "nyc --reporter=html --reporter=text mocha",
     "coverage": "nyc report --reporter=text-lcov | coveralls"
```    