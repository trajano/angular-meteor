# Official Meteor tutorials

This repository contains the content and view code for the official Meteor tutorials at [meteor.com](https://www.meteor.com/tutorials/blaze/creating-an-app).

Feel free to submit a pull request to improve the content!

### Tutorial content

1. [Blaze tutorial](https://www.meteor.com/tutorials/blaze/creating-an-app): [`/steps/blaze`](https://github.com/meteor/tutorials/tree/master/steps/blaze)
2. [Angular tutorial](https://www.meteor.com/tutorials/angular/creating-an-app): [`/steps/angular`](https://github.com/meteor/tutorials/tree/master/steps/angular)
3. React tutorial, work in progress (not live yet): [`/steps/react`](https://github.com/meteor/tutorials/tree/master/steps/react)

### Tutorial step-by-step repositories

We also maintain all of the tutorials as step-by-step git repositories here:

1. [Blaze](https://github.com/meteor/simple-todos)
2. [Angular](https://github.com/meteor/simple-todos-angular)
3. [React](https://github.com/meteor/simple-todos-react)

### Tutorial viewer

If you are editing the tutorials, use this simple app to view them: https://github.com/meteor/tutorial-viewer

## Tutorial workflow

### Editing the prose

Just edit the markdown files in `/steps/`.

### Editing code snippets

The code snippets are generated from the step-by-step git repositories which are git submodules in `/repos`. Each code snippet is its own commit. Commit messages follow the following format:

```
Step 3.1: Add some feature
```

After using `git rebase -i --root` to massage the repository into the desired state, run the script to update the generated files:

```sh
./scripts/process-repo.rb blaze
```

The commit with this message can then be included in the content with the following code snippet:

```html
{{> CodeBox step="3.1" view="blaze"}}
```

You should replace `blaze` with the correct view name (currently this string is automatically transformed to refer to the correct data structures).

You're done! Make sure to commit the changes to all of the generated files.

## Repository layout

This repository is a Meteor package; it's currently not published, but you can clone it and use it as a local package in an app.

The different parts of the repository have quite different responsibilities, but they are somewhat tightly coupled so it doesn't make sense to split them into separate packages at this point.

#### /git-patch-viewer/

This directory contains a Blaze component and build plugin for viewing git patches, tailored specifically to the needs of these tutorials. The build plugin reads files with `.patch` and `.multi.patch` file extensions, and puts data on a package-local object named `GitPatches`. You can access the data for a specific commit on `GitPatches[commitSha]`. The Blaze view component reads this data structure and displays a diff with context and a link to GitHub. Don't use the `GitPatch` component directly; use it by calling the more convenient `CodeBox` component from the `/shared` directory, as described below.

#### /generated/ (don't edit manually)

This directory contains files generated by `/scripts/process-repo.rb`:

1. `*-commits.js` are lists of commits keyed by the number of the step in the tutorial, with a parsed commit message.
2. `*.multi.patch` are concatenated patch files of the step-by-step tutorial repositories. The patch files are in the same format as the result of `git format-patch --stdout`. The build plugin in `/git-patch-viewer/patch-plugin.jsx` knows how to read these files.

#### /repos/

This directory contains git submodules of all three step-by-step tutorial repositories.

#### /routes/

This directory contains JavaScript data structures describing the different tutorial steps. These are exported from the package; the meteor.com website and tutorial-viewer app know how to read them.

#### /scripts/

This contains a script that is used to update `/generated/` from the repositories in `/repos/`.

#### /shared/

Tutorial-specific templates. Some of them are shared content for the different tutorials in markdown format, others are useful UI widgets. Includes the useful `CodeBox` template, which wraps the `GitPatch` template with more convenient inputs.

#### /steps/

The actual tutorial prose content, in Markdown format.