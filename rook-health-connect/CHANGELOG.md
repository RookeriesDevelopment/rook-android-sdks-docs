# Changelog

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
