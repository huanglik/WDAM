# axd  (Just testing for likunhuang)

## External dependencies

In addition to the git submodules mentioned below (see [Development](#development)), this package currently depends
on `libimobiledevice` to do certain things. Install it with [Homebrew](http://brew.sh/),

```
brew install libimobiledevice --HEAD  # install from HEAD to get important updates
brew install ideviceinstaller         # only works for ios 9. for ios 10, see below
```

There is also a dependency, made necessary by Facebook's [WebDriverAgent](https://github.com/facebook/WebDriverAgent),
for the [Carthage](https://github.com/Carthage/Carthage) dependency manager. If you
do not have Carthage on your system, it can also be installed with
[Homebrew](http://brew.sh/)

```
brew install carthage
```

`ideviceinstaller` doesn't work with iOS 10 yet. So we need to install [ios-deploy](https://github.com/phonegap/ios-deploy)

```
npm install -g ios-deploy
```

For real devices we can use [xcpretty](https://github.com/supermarin/xcpretty) to make Xcode output more reasonable. This can be installed by

```
gem install xcpretty
```



## Desired Capabilities

Should be the same for [Appium](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/caps.md)

Differences noted here

|Capability|Description|Values|
|----------|-----------|------|
|`noReset`|Do not destroy or shut down sim after test. Start tests running on whichever sim is running, or device is plugged in. Default `false`|`true`, `false`|
|`processArguments`|Process arguments and environment which will be sent to the WebDriverAgent server.|`{ args: ["a", "b", "c"] , env: { "a": "b", "c": "d" } }` or `'{"args": ["a", "b", "c"], "env": { "a": "b", "c": "d" }}'`|
|`wdaLocalPort`|This value if specified, will be used to forward traffic from Mac host to real ios devices over USB. Default value is same as port number used by WDA on device.|e.g., `8100`|
|`showXcodeLog`|Whether to display the output of the Xcode command used to run the tests. If this is `true`, there will be **lots** of extra logging at startup. Defaults to `false`|e.g., `true`|
|`iosInstallPause`|Time in milliseconds to pause between installing the application and starting WebDriverAgent on the device. Used particularly for larger applications. Defaults to `0`|e.g., `8000`|
|`xcodeOrgId`|Apple developer team identifier string. Must be used in conjunction with `xcodeSigningId` to take effect.|e.g., `JWL241K123`|
|`xcodeSigningId`|String representing a signing certificate. Must be used in conjunction with `xcodeOrgId`. This is usually just `iPhone Developer`, so the default (if not included) is `iPhone Developer`|e.g., `iPhone Developer`|
|`xcodeConfigFile`|Full path to an optional Xcode configuration file that specifies the code signing identity and team for running the WebDriverAgent on the real device.|e.g., `/path/to/myconfig.xcconfig`|
|`keychainPath`|Full path to the private development key exported from the system keychain. Used in conjunction with `keychainPassword` when testing on real devices.|e.g., `/path/to/MyPrivateKey.p12`|
|`keychainPassword`|Password for unlocking keychain specified in `keychainPath`.|e.g., `super awesome password`|
|`scaleFactor`|Simulator scale factor. This is useful to have if the default resolution of simulated device is greater than the actual display resolution. So you can scale the simulator to see the whole device screen without scrolling. |Acceptable values are: `'1.0', '0.75', '0.5', '0.33' and '0.25'`. The value should be a string.|
|`usePrebuiltWDA`|Skips the build phase of running the WDA app. Building is then the responsibility of the user. Only works for Xcode 8+. Defaults to `false`.|e.g., `true`|
|`preventWDAAttachments`|Sets read only permissons to Attachments subfolder of WebDriverAgent root inside Xcode's DerivedData. This is necessary to prevent XCTest framework from creating tons of unnecessary screenshots and logs, which are impossible to shutdown using programming interfaces provided by Apple.|Setting the capability to `true` will set Posix permissions of the folder to `555` and `false` will reset them back to `755`|
|`webDriverAgentUrl`|If provided, Appium will connect to an existing WebDriverAgent instance at this URL instead of starting a new one.|e.g., `http://localhost:8100`|
|`useNewWDA`|If `true`, forces uninstall of any existing WebDriverAgent app on device. This can provide stability in some situations. Defaults to `false`.|e.g., `true`|
|`wdaLaunchTimeout`|Time, in ms, to wait for WebDriverAgewnt to be pingable. Defaults to 60000ms.|e.g., `30000`|




### Watch

```
npm run watch
```


### Test

```
npm test
```

There are also a number of environment variables that can be used when running
the tests locally. These include:

* `REAL_DEVICE` - set to anything truthy, makes the tests use real device capabilities
* `_FORCE_LOGS` - set to `1` to get the log output, not just spec
* `PLATFORM_VERSION` - change the version to run the tests against (defaults to `9.3`)
* `XCCONFIG_FILE` - specify where the xcode config file is for a real device run (if
  blank, and running a real device test, it will search for the first file in
  the root directory of the repo with the extension "xcconfig")
* `UICATALOG_REAL_DEVICE` - path to the real device build of UICatalog, in case
  the npm installed one is not built for real device

