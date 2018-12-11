# recipeutil
A tool that helps manage and manipulate `autopkg` recipes.

```
recipeutil verb [--parent|--all-parents] [--names] recipename|recipeid
```

##  verbs

### `info`
synonym for autopkg info

### `id` or `identifier`
shows the identifier for the recipe

### `which` or `path`
prints the path to the recipe

### `open`
uses the `open` command to open the recipe with the default application

### `edit`
opens the recipe with the `$EDITOR`. If the variable `$RECIPE_EDITOR` is set, this will be used instead

### `reveal`
reveals the recipe in finder

### `status`
shows `git status` of the file

### `cache`
opens the cache folder for this recipe in the Finder

### `clearcache`
deletes the cache folder for this recipe

### `duplicate`
will prompt for a new recipe identifier fill that in a new recipe

### `newchild`
will create a new recipe pointed to the given recipe as parent. 


## options

### `--parent` or `-p`
applies the verb to the recipe's parent

### `--all-parents` or `-a`
recursively applies verb to the recipe and all its parents

### `--names` or `-n`
prints the name of the recipe before the information

### extra arguments
`recipeutil` will pass through extra arguments to the `edit` and `status` actions, e.g.

```
recipeutil status Firefox.download --short

recipeutil edit --all-parents Fetch.munki --new-window
```

### about generated identifiers

`duplicate` and `newchild` will attempt to generate a recipe identifier for you. To figure ou the right prefix, they will try the following:

First they will look for a key `RECIPE_ID_PREFIX` in the AutoPkg preference domain `com.github.autopkg`. You can set this with 

```
defaults write com.github.autopkg write RECIPE_ID_PREFIX com.scriptingosx
```

If that is unset, it will look for `RECIPE_ID_PREFIX` as an environment variable.

If the environment variable is also unset, it will attempt to get the `user.name` config setting from `git`. I.e. run this command:

```
git config --get user.name
```

and prefix `com.github.` to that.

If the git user name is unset it will fall back to the current user name and prefix `com.github.`

In most cases it should work just fine. If you do not like the default behavior, set the default in `com.github.autopkg`.

