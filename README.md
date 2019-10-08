# Patch for XCode 10.x and react-native 0.55
This is specifically meant for use with `create-react-native-web-app`. The problem is caused by XCode changing their device list naming convention.  


**Before**: Something like `"iOS-12-2"`  

**After**: `"com.apple.CoreSimulator.SimRuntime.iOS-12-2"`. 


In `react-native` version 0.55 (the necessary version for compatibility with `react-native-web`), the file that scans the device list uses `version.startsWith('iOS')` to look for devices. Since it no longer starts with `'iOS'` it never finds a device.

## Doing it yourself
You could just go into `myapp/node_modules/react-native/local-cli/runIOS/findMatchingSimulator.js` and replace the method `startsWith` with `includes`. This is what the script does.

## Using the script

1. `git clone https://github.com/closetothe/crnwa-xcode-patch`
2. `npx create-react-native-web-app myapp`
3. `mv crnwa-xcode-patch myapp && cd myapp/crnwa-xcode-patch`
4. `sudo chmod +x patch.sh`
5. `./patch.sh`

You can delete the `crnwa-patch` folder afterwards. I also recommend adding `--simulator="iPhone X"` to the end of your `ios` script in your `package.json`, since that version of `react-native` defaults to iPhone 6.

## One more thing
You also need to make sure you're using the legacy build system for the project. Open up the `ios` folder created by `create-react-native-web-app` in XCode. From [`react-native` issue #19573](https://github.com/facebook/react-native/issues/19573):  



1. Go to File, then Project Settings or Workspace Settings.
2. Select Legacy Build System from the Build System dropdown.
