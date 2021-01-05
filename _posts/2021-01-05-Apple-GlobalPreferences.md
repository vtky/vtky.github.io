---
layout: post
title: Apple GlobalPreferences
description: 
summary: 
tags: [apple, logging, preferences, globalpreferences]
---

## Logging

In Apple binaries, code similar to the following may be present. 

```c++
 v12 = (wchar_t *)_CFStringMakeConstantString(".GlobalPreferences");
 v4 = _CFStringMakeConstantString("AKFileLoggingEnabled");
 byte_742105A1 = CFPreferencesGetAppBooleanValue(v4);
```

```c++
_CFStringMakeConstantString(".GlobalPreferences");
v2 = _CFStringMakeConstantString("AppleIDAuthSupportLogging");
result = CFPreferencesGetAppBooleanValue(v2);
```

In the above two cases it enables logging of internal application information to an output file. In some other cases, it can be used to disable
security controls such as SSL Pinning to help make analysis easier.

```c++
v1 = _CFStringMakeConstantString("AppleServerAuthenticationNoPinning%@");
v2 = CFStringCreateWithFormat(0, 0, v1);
if ( v2 )
{
    _CFStringMakeConstantString("com.apple.Security");
    v0 = (unsigned __int8)CFPreferencesGetAppBooleanValue(v2) == 0;
    CFRelease(v2);
}
```

```c++
v0 = _CFStringMakeConstantString("AppleIDAuthSupportNoPinning");
_CFStringMakeConstantString(".GlobalPreferences");
return (unsigned __int8)CFPreferencesGetAppBooleanValue(v0) == 0;
```

These are `Boolean` values that can be configured within the `.GlobalPreferences.plist` file, located at:

macOS: `/Library/Preferences`
Windows: `C:\Users\<USERNAME>\AppData\Roaming\Apple Computer\Preferences`

Create the file, if it doesn't exist. The contents would be similar to the following:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>AppleLanguages</key>
	<array>
		<string>en-US</string>
	</array>
    <key>AKFileLoggingEnabled</key>
    <true/>
    <key>AppleIDAuthSupportLogging</key>
    <true/>
</dict>
</plist>
```

If used to enable logging to better understand an application, the logs would usually be found,

macOS: `TBD`
Windows: `C:\Users\<USERNAME>\AppData\Roaming\Apple Computer\Logs`
