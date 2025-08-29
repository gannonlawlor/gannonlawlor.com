+++
date = '2025-08-29T10:23:40-04:00'
title = 'Requesting macOS Privacy and Security Permissions for Your App'
+++

As I was building a simple app, I struggled with getting my app to show up in the privacy and security permissions sidebar. I need access the **Input Monitoring** or **Accessibility** APIs to listen to keyboard clicks and mouse movements.

---

### 1\. The Code Signing Requirement ✍️

You **cannot** use a build signed with "Sign to run locally." macOS's security model requires your application to be properly signed with a valid **Developer ID** or **Mac App Store** certificate. For testing, this means your app must be part of a team and signed with a development certificate.

---

### 2\. Checking for Permissions with Code ✅

Once your app is properly signed, you'll need to check if the user has granted the necessary permissions. These permissions should be added to the permissions app list when you first request them.

#### **For Accessibility**

To check for this permission, you'll use the [`AXIsProcessTrusted`](https://developer.apple.com/documentation/applicationservices/1460720-axisprocesstrusted) function from the ApplicationServices framework.

```swift
import ApplicationServices

func checkAccessibilityPermission() -> Bool {
    return AXIsProcessTrusted()
}
```

Another option is to use `AXIsProcessTrustedWithOptions`, which can automatically open the Accessibility pane in System Settings for the user.

```swift
func promptForAccessibilityPermission() {
    let options: CFDictionary = [kAXTrustedCheckOptionPrompt.takeUnretainedValue() as NSString: true] as CFDictionary
    AXIsProcessTrustedWithOptions(options)
}
```

#### **For Input Monitoring**

The Input Monitoring API is used for monitoring keyboard events across the entire system. You'll check for this permission using [`IOHIDCheckAccess`](https://developer.apple.com/documentation/iokit/3181573-iohidcheckaccess) from the IOKit framework. It returns an `IOHIDAccessType<type>` which can be `granted`, `denied`, or `unknown`.

```swift
import IOKit

func checkInputMonitoringPermission() -> IOHIDAccessType {
    return IOHIDCheckAccess(kIOHIDRequestTypeListenEvent)
}
```

If permission is not granted, you can programmatically open the Input Monitoring pane in System Settings to make it easy for the user to grant access.

```swift
func openInputMonitoringSettings() {
    let url = URL(string: "x-apple.systempreferences:com.apple.preference.security?Privacy_ListenEvent")!
    NSWorkspace.shared.open(url)
}
```
