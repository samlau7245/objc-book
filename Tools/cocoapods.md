# Pod 命令

```sh
# + cache         Manipulate the CocoaPods cache
# + deintegrate   Deintegrate CocoaPods from your project
# + env           Display pod environment
# + init          Generate a Podfile for the current directory
# + install       Install project dependencies according to versions from a Podfile.lock
# + ipc           Inter-process communication
# + lib           Develop pods
# + list          List pods
# + outdated      Show outdated project dependencies
# + package       Package a podspec into a static library.
# + plugins       Show available CocoaPods plugins
# + repo          Manage spec-repositories
# + repo-svn      Manage your Cocoapod spec repositories using subversion -v3.0.0_
# + search        Search for pods
# + setup         Setup the CocoaPods environment
# + spec          Manage pod specs
# + trunk         Interact with the CocoaPods API (e.g. publishing new specs)
# + try           Try a Pod!
# + update        Update outdated project dependencies and create new Podfile.lock
```

## package

> 把 `podspec` 打包成一个静态库。

```sh
pod package NAME [SOURCE]

--force             # Overwrite existing files. 强制覆盖之前已经生成过的二进制库
--no-mangle         # Do not mangle symbols of depedendant Pods.
--embedded          # Generate embedded frameworks. 生成静态.framework
--library           # Generate static libraries. 生成静态.a
--dynamic           # Generate dynamic framework. 生成动态.framework
--bundle-identifier # Bundle identifier for dynamic framework. 动态.framework是需要签名的，所以只有生成动态库的时候需要这个BundleId
--exclude-deps      # Exclude symbols from dependencies. 不包含依赖的符号表，生成动态库的时候不能包含这个命令，静态库一定需要包含依赖的符号表。
--configuration     # Build the specified configuration (e.g. Debug). Defaults to Release. 生成的库是 Debug 还是 Release。
--subspecs          # Only include the given subspecs
--spec-sources=private,https://github.com/CocoaPods/Specs.git   # The sources to pull dependant pods from (defaults to https://github.com/CocoaPods/Specs.git)
```

# Podfile

## 版本

```ruby
pod 'Objection'
pod 'Objection', '0.9'
pod 'AFNetworking', '~> 1.0'
```

* `= 0.1` Version 0.1.
* `> 0.1` Any version higher than 0.1.
* `>= 0.1` Version 0.1 and any higher version.
* `< 0.1` Any version lower than 0.1.
* `<= 0.1` Version 0.1 and any lower version.
* `~> 0.1.2` Version 0.1.2 and the versions up to 0.2, not including 0.2. This operator works based on the last component that you specify in your version requirement. The example is equal to >= 0.1.2 combined with < 0.2.0 and will always match the latest known version matching your requirements.
* `~> 0.1.3-beta.0` Beta and release versions for 0.1.3, release versions up to 0.2 excluding 0.2. Components separated by a dash (-) will not be considered for the version requirement.







































