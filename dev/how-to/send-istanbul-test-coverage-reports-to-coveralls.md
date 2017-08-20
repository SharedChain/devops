# Include Mocha and Istanbul test coverage reports in Coveralls

## Install coveralls

```
npm install coveralls --save-dev
```

## Add a `coverage` script in package.json

Update the project's `package.json`.

```
 "script": {
     "test": "nyc --reporter=html --reporter=text mocha",
     "coverage": "nyc report --reporter=text-lcov | coveralls"
```    

## Tell Travis CI to run coverage

Edit the projects `.travis.yml`.
Add the following at the end:

```
after_success: npm run coverage
```