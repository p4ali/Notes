## [Find the include path on mac](https://andreasfertig.blog/2021/02/clang-and-gcc-on-macos-catalina-finding-the-include-paths/)
```
xcrun --show-sdk-path
export SDKROOT="`xcrun --show-sdk-path`"
```
