
```sh
# Don't consult the cache when looking up values. In effect, causes the cache entry to be refreshed.
-n, --no-cache

# Removes the cache. Causes all values to be re-cached.
-k, --kill-cache

# Specifies which SDK to search for tools. If no --sdk argument is provided, then the SDK used will be taken from the SDKROOT environment variable, if present.
# Use xcodebuild -showsdks to list the available SDK names.
--sdk  

# Specifies which toolchain to use to perform the lookup. If no --toolchain argument is provided, then the toolchain to use will be taken from the TOOLCHAINS environment variable, if present.
--toolchain

# Print the full command line that is invoked.
-l, --log

# Enable "find" mode, in which the resolved tool path is printed instead of the tool being executed.
-f, --find

# Enable "run" mode, in which the resolved tool path is executed with any provided additional arguments. This is the default mode.
-r, --run

# Print the path to the selected SDK.
--show-sdk-path

# Print the version number of the selected SDK.
--show-sdk-version

# Print the build version number of the selected SDK.
--show-sdk-build-version

# Print the path to the platform for the selected SDK.
--show-sdk-platform-path

# Print the version number of the platform for the selected SDK.
--show-sdk-platform-version
```

```sh
% xcrun --find clang
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang

% xcrun --sdk iphoneos --find texturetool
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/texturetool

xcrun --sdk macosx --show-sdk-path
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk
```

```sh
% xcrun git status
# On branch master
# Your branch is up to date with 'origin/master'.
# 
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
# 	modified:   Podfile
# 
# no changes added to commit (use "git add" and/or "git commit -a")
```