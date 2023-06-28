# Changelog

## 0.5.2

* Fixed Blood Pressure returning NaN values.

## 0.5.1

* Migrated to data matrix 2.0
* Added events extraction
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

**New permissions**

From this version onwards you need to include 4 new READ permissions (Check README to see the full list of permissions
that you have to add to your manifest):

* READ_RESPIRATORY_RATE
* READ_NUTRITION
* READ_MENSTRUATION
* READ_POWER

**User info**

Due to migration currently in progress UserInfo extraction has been disabled

## 0.4.2

* Fixed obfuscation rules.
* Added UserInfo extraction.

## 0.4.0

**Breaking changes:**

All dateTime properties have been changed from `String` to `ZonedDateTime`

All date properties have been changed from `String` to `LocalDate`

All data classes and enums now have HC at the beginning, e.g.:

* BodySummary → **HC**BodySummary
* BloodGlucoseGranularDataMgPerDL → **HC**BloodGlucoseGranularDataMgPerDL

The following functions have been removed:

* getSleepSummaryLastDate
* getPhysicalSummaryLastDate
* getPhysicalEventsLastDate
* getBodySummaryLastDate

Instead, you should use `getLastExtractionDate` and provide a `HCRookDataType`:

* SLEEP_SUMMARY
* PHYSICAL_SUMMARY
* PHYSICAL_EVENT
* BODY_SUMMARY

**Other changes:**

* Data extraction performance optimizations

## 0.3.3

* To avoid duplication when retrieving `PhysicalEvents` the data will be filtered to the origin that created
  the `ExerciseSessionRecord`.

## 0.3.2

* Fixed Crash when creating an instance of `RookHealthConnectManager` on devices with android 8 or devices without HC
  APK.
* Before creating an instance of `RookHealthConnectManager` check availability with `RookHealthConnectAvailability`.

## 0.3.1

* Fixed logs duplication
* Added sourceOfData to:
    * SleepSummary
    * PhysicalSummary
    * PhysicalEvent
    * BodySummary
* Changed name of the following classes and properties:
    * `FloorsClimbedGranularDataFloors` to `FloorsClimbedGranularData`
    * `BloodPressureDayAvgSystolicDiastolicBp` to `BloodPressureSystolicDiastolic`
    * `BloodPressureSystolicDiastolicBp` to `BloodPressureSystolicDiastolic`
    * `BloodPressureGranularDataSystolicDiastolicBp` to `BloodPressureGranularDataSystolicDiastolic`
    * `BloodPressureGranularDataSystolicDiastolicBp.bloodPressureSystolicDiastolicBp`
      to `BloodPressureGranularDataSystolicDiastolic.bloodPressureSystolicDiastolic`
    * `BodySummary.bloodPressureDayAvgSystolicDiastolicBp` to `BodySummary.bloodPressureDayAvgSystolicDiastolic`
    * `BodySummary.bloodPressureGranularDataSystolicDiastolicBp`
      to `BodySummary.bloodPressureGranularDataSystolicDiastolic`
    * `PhysicalEvent.floorsClimbedGranularDataFloors` to `PhysicalEvent.floorsClimbedGranularData`
    * `PhysicalSummary.floorsClimbedGranularDataFloors` to `PhysicalSummary.floorsClimbedGranularData`

## 0.3.0

* Updated rook-auth to 0.3.1

## 0.1.0

* Updated Health Connect
  to [1.0.0-alpha11](https://developer.android.com/jetpack/androidx/releases/health-connect#1.0.0-alpha11)
    * You may have to update your gradle plugin to **7.4.1**
    * You may have to update your kotlin plugin to **1.8.10**

## 0.0.2

* Fixed missing `BloodPressureGranularDataSystolicDiastolicBp` class.

## 0.0.1

* Initial release.
