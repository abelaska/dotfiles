# Build failures after upgrading to Xcode 7.2

https://trac.macports.org/wiki/ProblemHotlist#xcode7.2

After installing the update to Xcode 7.2, port installations or upgrades using xcodebuild may fail with the following error due to an error in the Xcode installation. This is a problem that should be addressed by Apple in a future Xcode update.

    Could not find service "com.apple.CoreSimulator.CoreSimulatorService" in domain for uid: 502
    2015-12-10 17:21:27.407 xcodebuild[7363:75765] launchctl print returned an error code: 28928
    2015-12-10 17:21:27.407 xcodebuild[7363:75765] Failed to locate a valid instance of CoreSimulatorService in the bootstrap.  Adding it now.
    Could not find service "com.apple.CoreSimulator.CoreSimulatorService" in domain for uid: 502
    2015-12-10 17:21:27.431 xcodebuild[7363:75765] launchctl print returned an error code: 28928
    2015-12-10 17:21:27.431 xcodebuild[7363:75765] *** Assertion failure in -[SimServiceContext reloadServiceIfMovedOrAbortIfWeAreInvalid], /BuildRoot/Library/Caches/com.apple.xbs/Sources/CoreSimulator/CoreSimulator-201.3/CoreSimulator/SimServiceContext.m:451
    ** INTERNAL ERROR: Uncaught exception **
    Exception: Unable to lookup com.apple.CoreSimulator.CoreSimulatorService in the bootstrap.  This can happen if running with a sandbox profile.  When running with a sandbox profile, make sure that com.apple.CoreSimulator.CoreSimulatorService.xpc is owned by root, not group writable, and not world writable.  See <rdar://problem/22142915>.

The workaround is to manually fix the permissions of the path mentioned in the error message. User should be 'root', group should be 'wheel', and write permissions only granted for the owner:

    sudo chmod -R g-w $(xcode-select -p)/Library/PrivateFrameworks/CoreSimulator.framework/Versions/A/XPCServices/com.apple.CoreSimulator.CoreSimulatorService.xpc
    sudo chown -R root:wheel $(xcode-select -p)/Library/PrivateFrameworks/CoreSimulator.framework/Versions/A/XPCServices/com.apple.CoreSimulator.CoreSimulatorService.xpc
