# Development

## Getting Started

All of these commands require you to run `npm install` first. To see a full list of the gulp commands, run `gulp`.


### Installing Nightly Version

The latest nightly version can be installed via npm.

1. Run `npm install --save ionic-angular@nightly`
2. Your `package.json` file's `dependencies` will be updated with the nightly version.
3. Restart any `watch` or `serve` commands that may be already running.


### Building Ionic Source

Run `gulp build` or `gulp watch` to watch for changes.


### Building & Running e2e Tests

#### Development

1. Run `gulp e2e` or `gulp e2e.watch` to watch for changes.
2. Navigate to `http://localhost:8000/dist/e2e`

#### Validation

The following commands take longer to run because they use AoT compilation. They should really only be used to validate that our components work with AoT, and fix them if not.

1. Run `gulp e2e.prod` to bundle all e2e tests, or pass a folder for a specific test. For example, `gulp e2e.prod --f=alert/basic` will build the test in `src/components/alert/test/basic`.
2. Run `gulp e2e.watchProd` with a folder passed to watch a test. For example, `gulp e2e.watchProd --f=select/single-value` will watch the test in `src/components/select/test/single-value`.
3. Navigate to `http://localhost:8000/dist/e2e`


### Building & Running API Demos

1. Run `gulp demos` or `gulp demos.watch` to watch for changes.
2. Navigate to `http://localhost:8000/dist/demos`


### Building API Docs

1. `gulp docs` to build the nightly version
2. `gulp docs --doc-version=2.0.0` to build a specific API version


### Developing against Ionic locally

From `ionic` directory:

1. `gulp package build` or (`gulp package watch`, then open another terminal for step 2)
2. `cd dist`
3. `npm link` (may require `sudo`)

From your app directory:

1. `npm link ionic-angular`
2. `ionic serve` or `ionic run` or `ionic emulate`

To remove the linked version of `ionic-angular` do `npm rm ionic-angular`, and then reinstall using `npm install ionic-angular`.


### Running Snapshot

#### Setup

1. Install [Protractor](https://angular.github.io/protractor/#/): `npm install -g protractor@2.5.1`
2. Run `webdriver-manager update`
3. Export `IONIC_SNAPSHOT_KEY` (get from someone)

#### Commands

- `gulp snapshot` will run the `gulp e2e.prod` task with AoT compilation.
- `gulp snapshot.skipBuild` will skip the `gulp e2e.prod` task with AoT compilation.
- `gulp snapshot.dev` will run a development build using the `gulp e2e` task.
- `gulp snapshot.quick` will skip the build and run snapshot without uploading to the server.

#### Flags

- `--f | -folder` will run the command with a test folder. For example, `gulp snapshot --f=action-sheet/basic` will run snapshot for the test at `src/components/action-sheet/test/basic`.

### Running Tests

1. `gulp validate`


### Running Sass Linter

**Requires Ruby. Skip this step entirely if you are unable to install Ruby.**

1. See the [Sass Guidelines](https://github.com/driftyco/ionic/blob/master/.github/CONTRIBUTING.md#sass-changes) for editing the Sass.
2. Install the linter: `gem install scss_lint`
3. Run `gulp lint.sass` and fix any linter errors.


### Running TypeScript Linter

1. Run `gulp lint.ts` and fix any errors.


# Releasing

### Releasing Ionic Source

1. Run [snapshot](#running-snapshot) & verify all changes are intentional, update master snapshot if so
2. Run `gulp release`
  - Pulls latest from GitHub
  - Runs `gulp validate`
  - Builds npm package files into dist
  - Updates package.json version
  - Removes debug statements
  - Updates changelog
  - Publishes to npm
  - Creates a new tag and release on Github
3. Verify that the `changelog` changes are accurate and the `package.json` version is correct (`git status` && `git diff`)
4. Commit and push
5. Sit back and have a beer :beers: (or wine :wine_glass:)

### Publish a nightly release
1. Run `gulp nightly`
  - Pulls latest from GitHub
  - Runs `gulp validate`
  - Builds npm package files into dist
  - Removes debug statements
  - Publishes to npm using the `nightly` tag with the date/time of publish added to the version: `2.0.0-rc.0` results in `2.0.0-rc.0-201610131811`
2. `npm install ionic-angular@nightly` will now install the latest nightly release
3. Run `npm view ionic-angular` to see the latest nightly release


### Releasing Component Demos

Ionic Component demos are automatically compiled and deployed to the [ionic staging site](http://ionic-site-staging.herokuapp.com/) on every commit in [ionic-preview-app](https://github.com/driftyco/ionic-preview-app). No action is necessary.

If you'd like to manually update the demos, follow the steps on the preview app for [running locally on the site](https://github.com/driftyco/ionic-preview-app#running-locally-on-the-site).


### Releasing API Demos

Ionic API demos are automatically compiled and deployed to the [ionic staging site](http://ionic-site-staging.herokuapp.com/) on every commit. No action is necessary.

If you'd like to manually update the demos, clone the [`ionic-site`](https://github.com/driftyco/ionic-site) repo as a sibling of `ionic`. From `ionic` run `gulp demos` and then `gulp docs`, and it'll compile and copy the demos to the `ionic-site` repo, ready for testing.
