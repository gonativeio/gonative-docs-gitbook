# Jailbreak / root detection

_Once the premium module has been added to your app, you may use the following APIs to access its functionality._

## Overview

This module allows you to detect that your user is running on a rooted Android or jailbroken iOS device. It uses a variety of methods, including the presence of certain binaries, apps, and supported URL protocols.

Note that no root or jailbreak detection can be 100% foolproof. Having root or jailbreaking the device by its nature provides a level of access that can be used trick any attempt to detect it. This module is intended to provide a reasonable level of detection that you can use to warn your users or to disable certain functionality.

When the app launches, if the app detects a rooted device, it will load the initial URL with an additional query parameter, which may be either `rootDetected=true` or `isRooted=true` . Please check for both query parameter values. For example, instead of loading [https://example.com](https://example.com)/, the app will load [https://example.com/?rootDetected=true](https://example.com/?rootDetected=true)

## Libraries used

iOS jailbreak detection: [https://github.com/thii/DTTJailbreakDetection](https://github.com/thii/DTTJailbreakDetection)

Android root detection: [https://github.com/scottyab/rootbeer](https://github.com/scottyab/rootbeer)

\_\_

