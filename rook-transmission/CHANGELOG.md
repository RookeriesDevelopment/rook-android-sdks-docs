# Changelog

## 0.6.0

* Authorization it's now managed by this SDK (dependency on **rook-auth** is no longer required),
  see [Authorization](README.md#authorization) for more information.
* Changed `RookTransmissionManager` constructor parameters,
  see [RookTransmissionManager](README.md#rooktransmissionmanager) for more information.
* Added `RookHealthConnectEnvironment` to configure internal behaviour, see [Environment](README.md#environment) for
  more information.

## 0.5.1

* Improved security of `uploadUserTimeZone`.

## 0.5.0

* Added `uploadUserTimeZone` to update users timezone, see [Updating user timezone](README.md#updating-user-timezone) for more information.
* `UploadException` was moved to `com.rookmotion.rook.transmission.domain.exception`

## 0.4.3

* Updated rook-auth to 0.4.0

## 0.4.1

**Migrated to data matrix 2.0**

* Added events transmission
    * Blood Glucose Event
    * Blood Pressure Event
    * Body Metrics Event
    * Heart Rate Event
    * Hydration Event
    * Mood Event
    * Nutrition Event
    * Oxygenation Event
    * Stress Event
    * Temperature Event

## 0.3.5

* Performance optimizations

## 0.3.4

* Changed data types:
    * BodySummaryItem
        * temperatureDeltaCelsius → Double?
    * SleepSummaryItem
        * temperatureMinimumCelsius → Double?
        * temperatureAvgCelsius → Double?
        * temperatureMaxCelsius → Double?
        * temperatureDeltaCelsius → Double?
        * saturationAvgPercentage → Double?
        * saturationMinPercentage → Double?
        * saturationMaxPercentage → Double?

## 0.3.2

* Added `Platform` parameter in `RookTransmissionManager` constructor:
    * ANDROID - Enabled by default
    * FLUTTER - Enable compatibility with flutter's `shared_preferences` package.
    * REACT_NATIVE - Enable compatibility with `react-native-shared-preferences`.

## 0.3.1

* Fixed logs duplication
* Changed url to apiUrl in `RookTransmissionManager` constructor.

## 0.3.0

* Updated rook-auth to 0.3.1
* Added granular data to:
    * SleepSummaryItem
    * PhysicalSummaryItem
    * PhysicalEventItem
    * BodySummaryItem
* Changed String date and dateTime parameters to LocalDate and ZonedDateTime.

## 0.2.0

* Updated rook-auth to 0.2.0

## 0.1.0

* Added hrAvgRestingBpm (Int) to `BodySummaryItem`
* The following properties were changed to Double
    * temperatureMinimumCelsius
    * temperatureAvgCelsius
    * temperatureMaxCelsius

## 0.0.1

* Initial release.
