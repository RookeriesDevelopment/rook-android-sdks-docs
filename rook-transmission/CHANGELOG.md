# Changelog

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
