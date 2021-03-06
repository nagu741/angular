# Gulp Tasks folder

This folder contains one file for each task (or group of related tasks) for the project's gulpfile.
The dependencies between the tasks is kept in the gulpfile.

## Task File Structure
Each task is defined by a factory function that accepts `gulp` as a parameter.
Each file exports either one factory or an object of factories.

E.g. The `build.js` file contains only one task:

```js
module.exports = (gulp) => (done) => {
  ...
};
```

E.g. The `format.js` file contains two tasks:

```js
module.exports = {
  // Check source code for formatting errors (clang-format)
  enforce: (gulp) => () => {
    ...
  },

  // Format the source code with clang-format (see .clang-format)
  format: (gulp) => () => {
    ...
  }
};

```

## Loading Tasks

The tasks are loaded in the gulp file, by requiring them. There is a helper called `loadTask(fileName, taskName)`
will do this for us, where the `taskName` is optional if the file only exports one task.

E.g. Loading the task that will run the build, from a task file that contains only one task.

```js
gulp.task('build.sh', loadTask('build'));
```

E.g. Loading the task that will enforce formatting, from a task file that contains more than one task:

```js
gulp.task('format:enforce', loadTask('format', 'enforce'));
```

E.g. Loading a task that has dependencies:

```js
gulp.task('lint', ['format:enforce'], loadTask('lint'));
```
