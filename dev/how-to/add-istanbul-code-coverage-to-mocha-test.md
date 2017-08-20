# Add code coverage reporting with Mocha

Install Istanbul command line interface

```
npm install nyc --save-dev
```

Update the project `package.json` test script.

Change:

```
 "scripts": {
    "test": "mocha"
```

To:

```
  "scripts": {
    "test": "nyc --reporter=text mocha"
```
