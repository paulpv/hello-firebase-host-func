# hello-firebase-host-func

## Create

```
% firebase init
(node:91842) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  /Users/pv/Dev/GitHub/paulpv/hello-firebase-host-func

? Which Firebase features do you want to set up for this directory? Press Space to select features, then Enter to confirm your choices. Functions: Configure a Cloud 
Functions directory and its files, Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys, Emulators: Set up local emulators for 
Firebase products

=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add, 
but for now we'll just set up a default project.

? Please select an option: Use an existing project
? Select a default Firebase project for this directory: hello-firebase-host-func (hello-firebase-host-func)
i  Using project hello-firebase-host-func (hello-firebase-host-func)

=== Functions Setup
Let's create a new codebase for your functions.
A directory corresponding to the codebase will be created in your project
with sample code pre-configured.

See https://firebase.google.com/docs/functions/organize-functions for
more information on organizing your functions using codebases.

Functions can be deployed with firebase deploy.

? What language would you like to use to write Cloud Functions? JavaScript
? Do you want to use ESLint to catch probable bugs and enforce style? No
✔  Wrote functions/package.json
✔  Wrote functions/index.js
✔  Wrote functions/.gitignore
? Do you want to install dependencies with npm now? Yes
npm warn deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
npm warn deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

added 486 packages, and audited 487 packages in 12s

54 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New minor version of npm available! 11.0.0 -> 11.2.0
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.2.0
npm notice To update run: npm install -g npm@11.2.0
npm notice

=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? hosting
? Configure as a single-page app (rewrite all urls to /index.html)? No
? Set up automatic builds and deploys with GitHub? Yes
✔  Wrote hosting/404.html
✔  Wrote hosting/index.html

i  Detected a .git folder at /Users/pv/Dev/GitHub/paulpv/hello-firebase-host-func
i  Authorizing with GitHub to upload your service account to a GitHub repository's secrets store.

Visit this URL on this device to log in:
https://github.com/login/oauth/authorize?client_id=89cf50f02ac6aaed3484&state=314041217&redirect_uri=http%3A%2F%2Flocalhost%3A9005&scope=read%3Auser%20repo%20public_repo

Waiting for authentication...

✔  Success! Logged into GitHub as paulpv

? For which GitHub repository would you like to set up a GitHub workflow? (format: user/repository) paulpv/hello-firebase-host-func

✔  Created service account github-action-952730808 with Firebase Hosting admin permissions.
✔  Uploaded service account JSON to GitHub as secret FIREBASE_SERVICE_ACCOUNT_HELLO_FIREBASE_HOST_FUNC.
i  You can manage your secrets at https://github.com/paulpv/hello-firebase-host-func/settings/secrets.

? Set up the workflow to run a build script before every deploy? Yes
? What script should be run before every deploy? npm ci && npm run build

✔  Created workflow file /Users/pv/Dev/GitHub/paulpv/hello-firebase-host-func/.github/workflows/firebase-hosting-pull-request.yml
? Set up automatic deployment to your site's live channel when a PR is merged? Yes
? What is the name of the GitHub branch associated with your site's live channel? main

✔  Created workflow file /Users/pv/Dev/GitHub/paulpv/hello-firebase-host-func/.github/workflows/firebase-hosting-merge.yml

i  Action required: Visit this URL to revoke authorization for the Firebase CLI GitHub OAuth App:
https://github.com/settings/connections/applications/89cf50f02ac6aaed3484
i  Action required: Push any new workflow file(s) to your repo

=== Emulators Setup
? Which Firebase emulators do you want to set up? Press Space to select emulators, then Enter to confirm your choices. Functions Emulator, Hosting Emulator
? Which port do you want to use for the functions emulator? 5001
? Which port do you want to use for the hosting emulator? 5000
? Would you like to enable the Emulator UI? Yes
? Which port do you want to use for the Emulator UI (leave empty to use any available port)? 
? Would you like to download the emulators now? Yes

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...
i  Writing gitignore file to .gitignore...

✔  Firebase initialization complete!
```

