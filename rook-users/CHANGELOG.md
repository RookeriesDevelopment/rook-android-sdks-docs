# Changelog

## 0.6.0

### Android 14

This version will only compile against Android 14 (SDK 34). Please follow the steps to migrate

1. Update your gradle version to 8.1.2
2. Update your `compileSdk` and `targetSdk` to 34
3. Update your `sourceCompatibility`, `targetCompatibility` and `jvmTarget` to Java 17

## 0.5.0

* Authorization it's now managed by this SDK (dependency on **rook-auth** is no longer required),
  see [Initialization](README.md#initialization) for more information.
* Changed parameter to enable logs from `String` to `Boolean`, see [Logging](README.md#logging) for more information.
* Added `RookUsersEnvironment` to configure internal behaviour, see [Environment](README.md#environment) for
  more information.
* Changed `RookUsersManager` constructor parameters, see [RookUsersManager](README.md#rookusersmanager) for more
  information.

## 0.3.2

* Updated rook-auth to 0.4.0

## 0.3.1

* Fixed logs duplication
* Changed url to apiUrl in `RookUsersManager` constructor.

## 0.3.0

* Updated rook-auth to 0.3.1

## 0.1.0

* Updated rook-auth to 0.2.0

## 0.0.2

* Updated rook-auth to 0.1.0

## 0.0.1

* Initial release.
