## [Find the include path on mac](https://andreasfertig.blog/2021/02/clang-and-gcc-on-macos-catalina-finding-the-include-paths/)
```
xcrun --show-sdk-path
export SDKROOT="`xcrun --show-sdk-path`"
```

## [Where Does GCC Look to Find its Header Files](https://commandlinefanatic.com/cgi-bin/showarticle.cgi?article=art026)

```
cpp -v
```
