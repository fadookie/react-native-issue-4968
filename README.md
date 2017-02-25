# Repro case for react-native issue 4968
https://github.com/facebook/react-native/issues/4968

Reproduction was achieved with a local package one level up from the main app (this didn't happen when the shared package was a subfolder of the main app)

## Steps to reproduce
1. Clone this repo
2. `cd issue-4968`
3. `npm install`
4. `react-native run-ios`
5. Observe app loads normally
6. `npm run refresh-shared` (this deletes and re-installs the `4968-shared` package) 
7. Reload app

## Expected
`4968-shared` package is loaded successfully

## Actual
Error occurs during reload:

```
Unable to resolve module 4968-shared from /Users/eliot/Dev/pear/react-native-issue-4968/issue-4968/index.ios.js: Module does not exist in the module map or in these directories:
  /Users/eliot/Dev/pear/react-native-issue-4968/issue-4968/node_modules

This might be related to https://github.com/facebook/react-native/issues/4968
To resolve try the following:
  1. Clear watchman watches: `watchman watch-del-all`.
  2. Delete the `node_modules` folder: `rm -rf node_modules && npm install`.
  3. Reset packager cache: `rm -fr $TMPDIR/react-*` or `npm start -- --reset-cache`.

RCTFatal
-[RCTBatchedBridge stopLoadingWithError:]
__25-[RCTBatchedBridge start]_block_invoke_2
_dispatch_call_block_and_release
_dispatch_client_callout
_dispatch_main_queue_callback_4CF
__CFRUNLOOP_IS_SERVICING_THE_MAIN_DISPATCH_QUEUE__
__CFRunLoopRun
CFRunLoopRunSpecific
GSEventRunModal
UIApplicationMain
main
start
0x0
```

## Additional info
Restarting the packager seems to fix the issue until the next `npm run refresh-shared`.

I'm on macOS 10.11.5.

```
$ react-native -v
react-native-cli: 1.2.0
react-native: 0.41.2
```

```
$ watchman --version
4.7.0
```

```
$ node --version
v6.9.4
```