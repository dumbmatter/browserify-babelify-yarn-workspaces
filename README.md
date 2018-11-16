# Yarn Workspaces and Browserify - package.json in subfolder breaks the build

My ultimate goal is to use Yarn Workspaces in a project using Browserify and Babel. This is a minimal reproduction of a problem I'm having. Basically it seems that the presence of a package.json file in a subfolder (which is one of the things that you have when using Yarn Workspaces) breaks my Browserify build.

Here's how you can reproduce this error.

First, install the dependencies (you can use yarn or npm, doesn't matter):

    $ npm install

Then confirm the Browserify+Babel build works:

    $ npm run build

    > browserify-babelify-yarn-workspaces@1.0.0 build /home/user/projects/browserify-babelify-yarn-workspaces
    > browserify a/index.js -t babelify --outfile bundle.js

Yay, all is good! My compiled code is in bundle.js.

Now let's make a dummy package.json within the `a` folder:

    $ echo "{}" > a/package.json

That shouldn't change the build, right? Wrong:

    $ npm run build

    > browserify-babelify-yarn-workspaces@1.0.0 build /home/user/projects/browserify-babelify-yarn-workspaces
    > browserify a/index.js -t babelify --outfile bundle.js


    /home/user/projects/browserify-babelify-yarn-workspaces/a/index.js:1
    import lib from "./lib.js";
    ^
    ParseError: 'import' and 'export' may appear only with 'sourceType: module'
    npm ERR! code ELIFECYCLE
    npm ERR! errno 1
    npm ERR! browserify-babelify-yarn-workspaces@1.0.0 build: `browserify a/index.js -t babelify --outfile bundle.js`
    npm ERR! Exit status 1
    npm ERR! 
    npm ERR! Failed at the browserify-babelify-yarn-workspaces@1.0.0 build script.
    npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

    npm ERR! A complete log of this run can be found in:
    npm ERR!     /home/user/.npm/_logs/2018-11-16T15_58_43_540Z-debug.log

Why??
