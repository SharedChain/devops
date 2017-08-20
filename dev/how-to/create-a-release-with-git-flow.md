# Start a Release with git flow

We will create release `3.0.2` in this example.


## Start the release

```
git flow release start 3.0.2
```


## Publish a feature

```
git flow release publish 3.0.2
```


## Bump up the version number 

Edit the verison in `package.json` to match the release version

```
  "version": "3.0.2",
```


## Finish the release

```
git flow release finish '3.0.2'
``` 

## Push changes to master

```
git checkout master
git push
```

## Re-Publish NPM (optional)

```
npm publish
```