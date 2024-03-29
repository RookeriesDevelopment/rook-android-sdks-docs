# Changelog

## 0.11.1

* If the userID is not initialized a `UserNotInitializedException` will be thrown.

## 0.11.0

* Changed all `clientPassword` instances to `secretKey`.

## 0.10.0

* Added setUserID to configure the user of HC summaries and events, see [Initialization](README.md#initialization) for
  more information.
* Removed userID parameter in `RookHealthConnectConfiguration.initRookHealthConnect()`

## 0.9.6

* When requesting permissions the Health Connect data source status will be changed to active.

## 0.9.5

* Optimized the number of calls to Health Connect required to extract Temperature Events.

## 0.9.0

### Android 14

This version will only compile against Android 14 (SDK 34). Please follow the steps to migrate

1. Update your gradle version to 8.1.2
2. Update your `compileSdk` and `targetSdk` to 34
3. Update your `sourceCompatibility`, `targetCompatibility` and `jvmTarget` to Java 17

### Changes

* Permissions functions have changed, see [Check permissions](README.md#check-permissions) for more information.
* Privacy policy configuration has changed, see [Privacy policy](README.md#privacy-policy) for more information.
* Updated data access links, see [Privacy policy](README.md#request-data-access) for more information.
* Added new obfuscation rules, see [Obfuscation](README.md#obfuscation) for more information.

## 0.8.0

* Authorization it's now managed by this SDK (dependency on **rook-auth** is no longer required),
  see [Initialization](README.md#initialization) for more information.
* Changed parameter to enable logs from `String` to `Boolean`, see [Logging](README.md#logging) for more information.
* Added `RookHealthConnectEnvironment` to configure internal behaviour, see [Environment](README.md#environment) for
  more information.
* Changed `RookHealthConnectManager` constructor parameters,
  see [RookHealthConnectManager](README.md#rookhealthconnectmanager) for more information.
* Fixed documentation typos.
* Removed `getPhysicalStressEvents` and `getBodyMoodEvents` due to always being empty. Will be added in the future if
  Health Connect adds support for the data they require (The classes `HCMoodEvent` and `HCStressEvent` won't be
  removed).

## 0.6.4

* Added Time Zone extraction, see [Retrieving user timezone](README.md#retrieving-user-timezone) for more information.

## 0.6.3

* Optimized calls to the Health Connect API for all summaries and events
* Updated rook-auth to 0.4.0

## 0.6.0

* When retrieving events if the Event has no data `RecordsNotFoundException` will be thrown.
* Body pillar events will be divided in intervals of 1 hour.
* `lastExtractionDate` will follow a different set of rules for Summaries and Events,
  see [Keeping track of the last date a summary was retrieved](README.md#keeping-track-of-the-last-date-a-summary-was-retrieved)
  and [Keeping track of the last date an event was retrieved](README.md#keeping-track-of-the-last-date-an-event-was-retrieved)
  for more details.
* The ZonedDateTime provided to health data extraction has new rules,
  see [Retrieving health data](README.md#retrieving-health-data)
  and [Retrieving health data in UTC](README.md#retrieving-health-data-in-utc) for more details

## 0.5.4

* Fixed min values not calculated

## 0.5.3

The `Metadata.dateTime` of physical pillar events like:

* Physical Event
* Physical Heart Rate Event
* Physical Oxygenation Event
* Physical Stress Event

Will use the `startTime` of the `ExerciseSessionRecord` they belong to.

## 0.5.2

* Fixed Blood Pressure returning NaN values.

## 0.5.1

**Migrated to data matrix 2.0**

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

From this version onwards you need to include 4 new READ permissions.
Check [Android configuration](README.md#android-configuration) to see the full list of permissions
that you have to add to your manifest:

* READ_RESPIRATORY_RATE
* READ_NUTRITION
* READ_MENSTRUATION
* READ_POWER

**New permissions system**

If you are using the old health connect permissions declaration (meta-data tag), you MUST migrate otherwise
the `permissions` functions won't work.

**New timezone requirements**

The ZonedDateTime object provided when requesting health data must be in UTC,
see [Retrieving health data in UTC](README.md#retrieving-health-data-in-utc) for more
details.

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
