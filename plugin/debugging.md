# Debugging

## Understanding cloud logs:

### Send from the backend

* **InvalidIp** => the user is located in Google data center and the update is less than 4 hours old, this have been made to prevent Google bots device to count as device in your account
* **needPlanUpgrade** (previously **needUpgrade**) => Mean you have reach the limit of your plan, and device will not receive update until you upgrade or next month.
* **noNew** => The device has the latest version available
* **disablePlatformIos** => The device is on ios platform but that disable in channel settings
* **disablePlatformAndroid** => The device is on android platform but that disable in channel settings
* **disableAutoUpdateToMajor** => The device has a version (**1**.2.3) and the channel has major update (**2**.0.0) to send but that disable in channel settings
* **disableAutoUpdateUnderNative** =>The device has a version (1.2.**3**) and the channel has  update (1.2.**2**) under the device version to send but that disable in channel settings
* **disableDevBuild** => The device has a dev build but that disable in channel settings
* **disableEmulator** => The device is an emulator but that disable in channel settings

### Send from the device

* **get** => Info for download new version has been send to device
* **delete** => one bundle has been delete in the device
* **set** => bundle has been set on the device
* **set\_fail** => bundle failed to set&#x20;
* **reset** => device reset ton **builtin** bundle
* **download\_XX** => new bundle has been downloat from xx % ( every 10%)
* **download\_complete** => new bundle finish download
* **download\_fail** => new bundle fail download
* **update\_fail** => new bundle has been installed but failed to call notifyAppReady
* **checksum\_fail** => new bundle fail to validate checksum

### Bundle status

* SUCCESS: install bundle done
* ERROR: install or download failed
* PENDING: Download done, pending release
* DELETED: Bundle deleted still present for stats
* DOWNLOADING: Curently downloading a bundle



## Understanding device logs:

### Ios

to find your logs on Xcode&#x20;

{% embed url="https://intercom.help/deploygate/en/articles/4682692-getting-the-device-log-in-xcode" %}

### Android:

to find your logs on Android studio

{% embed url="https://developer.android.com/studio/debug/am-logcat" %}

### Explainations

* `Failed to download from` **=>** same as **download\_fail**
* `notifyAppReady was not called, roll back current bundle` => same as as **update\_fail**

## Finding the downloaded bundle in your devide

### iOS

To debug on iOS, you need to dump the app on your computer, you can do it like that :

Xcode has a built-in feature for inspecting the file system of developer installed apps on an iOS device.

To achieve this:

* Connect your device to your Mac and select Window > Devices in the Xcode menubar.
* Select your device in the left pane under the Devices section.
* &#x20;This will show a list of developer installed apps for that device.&#x20;
* Select the app you want to inspect and then the select the gear icon near the bottom of the screen.
* &#x20;Here you can view the current file system by selecting Show Container or download a snapshot of it.&#x20;

Selecting Download Container... will download and export a snapshot of the file system as a .xcappdata file that you can browse through.

Right-click on this file and select Show Package Contents to open the folder.

Open the App Data folder, and you should now see a few folders like Documents, Library, tmp, etc.

![image](https://user-images.githubusercontent.com/4084527/166708589-8d500351-e140-41c3-bea2-a037fe35243e.png)

Then you will find version in 2 folders:

`library/NoCloud/ionic_built_snapshots` who is necessary after app reboot

and `documents/versions` for hot reload

### Android

To debug on Android, you need to access the device from Android Studio:

* Click View > Tool Windows > Device File Explorer or click the Device File Explorer button in the tool window bar to open the Device File Explorer.&#x20;
* Select a device from the drop down list.
* Open the path **data/data/APP\_NAME/** where **APP\_NAME is your app ID.**

![image](https://user-images.githubusercontent.com/4084527/166708728-8f96fc73-5d90-426f-8d27-301697347a5f.png)

Then Find the `versions` folder to see all the folder versions

{% hint style="info" %}
On android all version are stored in one folder, unlike IOS where it has to be duplicated in two location.
{% endhint %}



## Understand ios production crash

{% embed url="https://developer.apple.com/news/?id=nra79npr" %}
