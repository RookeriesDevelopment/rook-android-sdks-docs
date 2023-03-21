# Changelog

## 0.3.1

* Fixed logs duplication

## 0.3.0

* flutter flag in `AuthorizationProvider` constructor was changed to a `Platform` enum:
    * ANDROID - Default behavior
    * FLUTTER - Enable compatibility with flutter's `shared_preferences` package.
    * REACT_NATIVE - Enable compatibility with `react-native-shared-preferences`.

## 0.2.0

* Added flutter flag to `AuthorizationProvider` constructor, set it to true when using this SDK from
  a flutter plugin to enable compatibility with flutter's `shared_preferences` package.

## 0.1.0

* Removed `enableLogs` function from `AuthorizationProvider`.
* Added optional `logLevel` parameter to `AuthorizationProvider`'s constructor.
    * "ADVANCED" -> All logs from API. All logs from SDK.
    * "BASIC" -> Basic logs from API. All logs from SDK.
    * "NONE" -> No logs.

## 0.0.1

* Initial release.
