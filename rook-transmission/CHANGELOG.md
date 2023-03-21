# Changelog

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
