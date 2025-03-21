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
https://github.com/login/oauth/authorize?client_id=...&redirect_uri=http%3A%2F%2Flocalhost%3A9005&scope=read%3Auser%20repo%20public_repo

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
https://github.com/settings/connections/applications/...
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

## Deploy

### Automatic
* `hosting` is deployed automatically by GitHub Actions
* `functions` that are `"pinTag": true` are deployed automatically **on `main`** by `hosting`.  
   For PR/branch, https://firebase.google.com/docs/hosting/full-config#rewrites says:
   > `pinTag`:
   >
   > The pinTag feature is only available in Cloud Functions for Firebase (2nd gen). With this feature, you can ensure that each function for generating your site's dynamic content is kept in sync with your static Hosting resources and Hosting config. Also, this feature allows you to preview your rewrites to functions on Hosting preview channels.
   >
   > If you add "pinTag": true to a function block of the hosting.rewrites config, then the "pinned" function will be deployed along with your static Hosting resources and configuration, even when running firebase deploy --only hosting. If you roll back a version of your site, the "pinned" function is also rolled back.
   >
   > **Note: If you add a pinTag to an existing rewrite, you must first deploy the updated rewrite to your "live" channel. After that deploy, you can then preview changes to your function's code in Hosting preview channels without affecting production.**
   >
   > This feature relies on Cloud Run tags, which have a limit of 1000 tags per service and 2000 tags per region. This means that after hundreds of deploys, the oldest versions of a site may stop working.

#### GitHub Deploy Fail When `"pinTag": true`

GitHub Action Deploying `pinTag`s will fail on default `firebase init` projects.

##### Error #1
Build will at first fail with `functions` Node project not being initialized and missing dependencies.

You will need to add installing Node and running `npm ci` in the `functions` directory per [.github/workflows/firebase-hosting-merge.yml](.github/workflows/firebase-hosting-merge.yml).
```yaml
    ...
    runs-on: ubuntu-latest
    env:
      NODE_VERSION: 22
    steps:
      - uses: actions/checkout@v4
      - name: Set up Node ${{ env.NODE_VERSION }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
          cache-dependency-path: './functions/package-lock.json'
      - name: Install dependencies
        working-directory: ./functions
        run: npm ci
      ...
```

##### Error #2
```
  "error": "Request to https://firebaseextensions.googleapis.com/v1beta/projects/hello-firebase-host-func/instances?pageSize=100&pageToken= had HTTP Error: 403, The caller does not have permission"
```
Debug:
```
  [2025-03-19T08:34:13.021Z] [iam] error while checking permissions, command may fail: Authorization failed. This account is missing the following required permissions on project hello-firebase-host-func:
    firebaseextensions.instances.list
```
FIX: Enable missing IAM Permission on the `github-action-*@*.iam.gserviceaccount.com` service account following these steps:
1. Find any `403` missing permissions in the action log.
2. Look up the permission at:
  * https://cloud.google.com/iam/docs/understanding-roles#predefined_roles
  * https://cloud.google.com/iam/docs/service-agents
3. Add the permission to the service account.
4. Retry the build.
5. If fail, goto 1 and repeat until success.

Minimal roles that I came up with over several failed runs:
* `firebaseextensions.instances.list`  
  This one is not well documented and resolves to `Firebase Extensions Viewer` per:
  * https://github.com/firebase/firebase-tools/issues/7754#issuecomment-2380099006
  * https://github.com/firebase/firebase-tools/issues/7754#issuecomment-2402777248
  * https://github.com/firebase/firebase-tools/issues/7582#issuecomment-2381456497
  * https://github.com/firebase/firebase-tools/issues/7854#issuecomment-2470280854
  * https://github.com/firebase/firebase-functions/issues/1598#issuecomment-2374950954
  * https://github.com/firebase/firebase-tools/issues?q=%22firebaseextensions.instances.list%22
* `artifactregistry.packages.delete`  
  `artifactregistry.repositories.downloadArtifacts`  
  Resolves to `Cloud Functions Service Agent`  
  Seems a bit excessive, but the role name seems appropriate

References:
* https://firebase.google.com/docs/projects/iam/permissions#functions
  * `roles/cloudfunctions.admin` -> `Cloud Functions Admin`
  * `roles/iam.serviceAccountUser` -> `Service Account User`
* Related?
  * https://stackoverflow.com/a/77466191/252308
  * https://issuetracker.google.com/issues/374025430

##### Error #3
```
"error": "Request to https://cloudbilling.googleapis.com/v1/projects/hello-firebase-host-func/billingInfo had HTTP Error: 403, Cloud Billing API has not been used in project 493412871115 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/cloudbilling.googleapis.com/overview?project=493412871115 then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry."
```
FIX: Enable Billing API at https://console.developers.google.com/apis/api/cloudbilling.googleapis.com/overview?project=493412871115

### `FIREBASE_SERVICE_ACCOUNT_<PROJECT_ID>`
To re-create a `gserviceaccount` from scratch:
1. Go to https://console.cloud.google.com/iam-admin/serviceaccounts.
1. Locate your `github-action-<UNIQUE_ID>@<PROJECT_ID>.iam.gserviceaccount.com` account.
1. Make a note of the existing account's name and unique id.
1. Delete the existing account.
1. Create a new account with the same name and id.
1. Run `firebase init hosting:github`
   1. Enter your `username/reponame`

Alternatively, you can do this manually by following this guide:
* https://github.com/FirebaseExtended/action-hosting-deploy/blob/main/docs/service-account.md
