# Rook Health Connect SDK

This SDK enables apps to extract data from Google Health Connect. **rook_health_connect** is part
of Rook Extraction, a series of SDKs dedicated to extracting Health Data from a variety of
[Data Sources](https://docs.tryrook.io/docs/Definitions#data-sources).

* This SDK was developed with the Health
  Connect [native SDK](https://developer.android.com/guide/health-and-fitness/health-connect), so
  all its limitations and requirements apply to this SDK as well.

## Features

* Check for Health Connect APK availability.
* Check and request permissions.
* Retrieve health data:
    * [Sleep Summary](https://docs.tryrook.io/docs/ROOKConnect/DataStructure/#sleep-health)
    * [Physical Summary](https://docs.tryrook.io/docs/ROOKConnect/DataStructure/#physical-health)
    * [Physical Event](https://docs.tryrook.io/docs/ROOKConnect/DataStructure/#physical-health)
    * [Body Summary](https://docs.tryrook.io/docs/ROOKConnect/DataStructure/#body-health)
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

## Installation

![Maven Central](https://img.shields.io/maven-central/v/com.rookmotion.android/rook-health-connect?style=for-the-badge&logo=gradle&label=rook-health-connect&color=7200F7)
![Maven Central](https://img.shields.io/maven-central/v/com.rookmotion.android/rook-bom?style=for-the-badge&logo=gradle&label=rook-bom&color=7200F7)

In your **build.gradle** (app module) add the required dependencies. We recommend using our BoM to get latest compatible
versions:

```groovy
implementation platform("com.rookmotion.android:rook-bom:bom-version")
implementation 'com.rookmotion.android:rook-health-connect'
```

## Getting started

To get authorization to use this SDK, you'll need to install and configure
the [rook-auth](https://mvnrepository.com/artifact/com.rookmotion.android/rook-auth) SDK.

### Android configuration

In your build.gradle (app) set your min and target sdk version like below:

```groovy
minSdk 26
targetSdk 33
```

* This SDK will only work with devices of a minSdk 28 or later. The `minSdk 26` is to keep
  compatibility with other Rook SDKs that can be used with older SDKs.

In your **AndroidManifest.xml**, add a query for Health Connect:

```xml

<manifest>
    <queries>
        <package android:name="com.google.android.apps.healthdata"/>
    </queries>
</manifest>
```

Then declare the health permissions used by this SDK:

```text
<uses-permission android:name="android.permission.health.READ_SLEEP" />
<uses-permission android:name="android.permission.health.READ_STEPS" />
<uses-permission android:name="android.permission.health.READ_DISTANCE" />
<uses-permission android:name="android.permission.health.READ_FLOORS_CLIMBED" />
<uses-permission android:name="android.permission.health.READ_ELEVATION_GAINED" />
<uses-permission android:name="android.permission.health.READ_OXYGEN_SATURATION" />
<uses-permission android:name="android.permission.health.READ_VO2_MAX" />
<uses-permission android:name="android.permission.health.READ_TOTAL_CALORIES_BURNED" />
<uses-permission android:name="android.permission.health.READ_ACTIVE_CALORIES_BURNED" />
<uses-permission android:name="android.permission.health.READ_HEART_RATE" />
<uses-permission android:name="android.permission.health.READ_RESTING_HEART_RATE" />
<uses-permission android:name="android.permission.health.READ_HEART_RATE_VARIABILITY" />
<uses-permission android:name="android.permission.health.READ_EXERCISE" />
<uses-permission android:name="android.permission.health.READ_SPEED" />
<uses-permission android:name="android.permission.health.READ_WEIGHT" />
<uses-permission android:name="android.permission.health.READ_HEIGHT" />
<uses-permission android:name="android.permission.health.READ_BLOOD_GLUCOSE" />
<uses-permission android:name="android.permission.health.READ_BLOOD_PRESSURE" />
<uses-permission android:name="android.permission.health.READ_HYDRATION" />
<uses-permission android:name="android.permission.health.READ_BODY_TEMPERATURE" />
<uses-permission android:name="android.permission.health.READ_RESPIRATORY_RATE" />
<uses-permission android:name="android.permission.health.READ_NUTRITION" />
<uses-permission android:name="android.permission.health.READ_MENSTRUATION" />
<uses-permission android:name="android.permission.health.READ_POWER" />
```

### Privacy policy

Health Connect requires a privacy policy where you inform your users how you will handle and use their data.

To comply with this you'll need an intent filter. You can use a dedicated Activity, or if you are using a **Single
activity architecture**, you can use Deeplinks. You can find an example of the first approach in our demo app.

Inside the Activity that you use to display your app's privacy policy, add an intent filter for the Health Connect
permissions action:

```xml

<activity android:name=".ui.health_connect.HCPrivacyPolicyActivity" android:enabled="true"
          android:exported="true">

    <intent-filter>
        <action android:name="androidx.health.ACTION_SHOW_PERMISSIONS_RATIONALE"/>
    </intent-filter>
</activity>
```

### Logging

If you want to see the logs generated by this SDK

When creating an instance of `RookHealthConnectManager` provide a logLevel:

```kotlin
RookHealthConnectManager(logLevel = "ADVANCED")
```

Available levels:

* "ADVANCED" -> All logs from API. All logs from SDK.
* "BASIC" -> Basic logs from API. All logs from SDK.
* "NONE" -> No logs.

## Usage

Create an instance of `RookHealthConnectManager` providing a context:

```kotlin
val manager = RookHealthConnectManager(context)
```

**WARNING:**

* Before creating any instances, check [availability](#availability).

### Availability

Before using any of the features of *rook-health-connect*, you need to ensure the user's device is compatible with
Health
Connect and check if the [APK](https://play.google.com/store/apps/details?id=com.google.android.apps.healthdata) is
installed.

Call `checkAvailability` on `RookHealthConnectAvailability`, this will return an `AvailabilityStatus`.

| Status        | Description                                 | What to do                                      |
|---------------|---------------------------------------------|-------------------------------------------------|
| INSTALLED     | APK is installed                            | Proceed to check permissions                    |
| NOT_INSTALLED | APK is not installed                        | Prompt the user to install Health Connect.      |
| NOT_SUPPORTED | This device does not support Health Connect | Take the user out of the Health Connect section |

### Permissions

#### Check permissions

There are dedicated functions for each [Health Pillar](https://docs.tryrook.io/docs/Definitions#health-data-pillars)
(Sleep, Physical, and Body) to check permissions. These functions follow the convention: `has_data_type_Permissions`

You can also call `hasAllPermissions` to check if your app has all the permissions required to
extract data from all health pillars.

```kotlin
fun checkPermissions() {
    scope.launch {
        val result = manager.hasAllPermissions()

        if (result) {
            // The app has permissions.
        } else {
            // The app does not have permissions.
        }
    }
}
```

#### Request permissions

There are dedicated functions for each [Health Pillar](https://docs.tryrook.io/docs/Definitions#health-data-pillars)
(Sleep, Physical and Body) to request permissions. These functions follow the
convention: `request_data_type_Permissions`

You can also call `requestAllPermissions` and provide an `Activity` instance to request all health
pillar permissions.

```kotlin
fun requestPermissions(activity: Activity) {
    val result = manager.requestAllPermissions(activity)

    if (result) {
        // A request to Health Connect to open the permissions screen was sent.
        // Health Connect can receive the request but refuse to open the permissions screen. In those cases, 'result' will also be true
    } else {
        // The request was not sent.
    }
}
```

**Permissions denied**

If the user clicks cancel or navigates away from the permissions screen, Health Connect will take it as if the user
denied the permissions.

If the user 'denies' the permissions 2 times, your app will be blocked by Health Connect. This is
permanent and cannot be undone even if the user uninstalls your app.

When your app is blocked, any permissions request will be ignored.

To solve this problem, we recommend you also include an `Open Health Connect` button in your
permissions UI. This button will call `openHealthConnectSettings`, there your users can manually
grant permissions to your app.

```kotlin
fun openHealthConnectSettings() {
    val result = manager.openHealthConnectSettings()

    if (result) {
        // A request to open Health Connect was sent.
    } else {
        // The request was not sent.
    }
}
```

### Retrieving user information

To retrieve user information call `getUserInfo` It will return a HCUserInfo instance.

```kotlin
fun getUserInfo() {
    scope.launch {
        try {
            val result = manager.getUserInfo(date)

            // Success
        } catch (e: Exception) {
            // Manage error
        }
    }
}
```

All properties (except for `dateTime` and `sourceOfData`) are nullable because there will be cases where we won't be
able to extract such information, in those cases you can check if the properties are null, and change them (all
nullable properties are also mutable) before enqueuing your HCUserInfo
to [rook-transmission](https://mvnrepository.com/artifact/com.rookmotion.android/rook-transmission).

```text
fun fillNullProperties(userinfo: HCUserInfo) {
    if (userinfo.userInformation.userDemographics.sex == null) {
        userinfo.userInformation.userDemographics.sex = // Provide sex 
    }

    if (userinfo.userInformation.userDemographics.city == null) {
        userinfo.userInformation.userDemographics.city = // Provide city 
    }

    if (userinfo.userInformation.userDemographics.education == null) {
        userinfo.userInformation.userDemographics.education = // Provide education 
    }

    if (userinfo.userInformation.userBodyMetrics.dateOfBirth == null) {
        userinfo.userInformation.userBodyMetrics.dateOfBirth = // Provide dateOfBirth 
    }

    // Enqueue with rook-transmission
}
```

### Retrieving health data

There are 2 types of health data **Summaries** and **Events**.

| Health Data | Timezone (Input / Output) | Oldest date of retrieval | Soonest date of retrieval |
|-------------|---------------------------|--------------------------|---------------------------|
| Summary     | UTC                       | 29 days                  | Yesterday                 |
| Event       | UTC                       | 29 days                  | Today                     |

#### Retrieving health data in UTC

All health data types require a ZonedDateTime instance of the datetime you want to retrieve data from. When
using `get_health_data_type(date: ZonedDateTime)` the provided `date` is used to create a range
between `date` (start) and the end of the day (end). This can become tricky specially with
all the different timezones your users may be in.

For that reason we **only allow UTC when retrieving data**, follow the following examples:

##### Retrieve health data for the whole day of yesterday

Today is **2023-05-26** in Mexico City (**UTC-6**)

To retrieve health data for the whole day of yesterday (**2023-05-25**) call `get_health_data_type(date: ZonedDateTime)`
where date is obtained with:

```kotlin
val date = ZonedDateTime.now() // Today 2023-05-26T15:12:20
    .minusDays(1) // Yesterday 2023-05-25T15:12:20
    .truncatedTo(ChronoUnit.DAYS) // Yesterday 2023-05-25T00:00:00
    .withZoneSameInstant(ZoneId.of("UTC")) // Yesterday 2023-05-25T06:00:00Z
```

As you can see we started obtaining **today's** then subtracted one day to get **yesterday's**, and because we want
to cover the whole day, all fields after `DAYS` were truncated to zero.

Finally, the date was converted to UTC resulting in **2023-05-25T06:00:00Z** it has **06:00:00** because
**2023-05-25T00:00:00** in Mexico City is **2023-05-25T06:00:00Z** in UTC.

Internally the SDK will perform the following operations:

1. Use 2023-05-25T06:00:00Z as `start`
2. Create the range between `start` and the end of the day `start` belongs to.

So the time range will end up like:

`start`: 2023-05-25T06:00:00Z < --- >`end`: 2023-05-26T06:00:00Z

Which in **Mexico City (UTC-6)** is equivalent to:

`start`: 2023-05-25T00:00:00 < --- >`end`: 2023-05-26T00:00:00

##### Retrieve health data for the whole day of today

Today is **2023-05-26** in Mexico City (**UTC-6**)

To retrieve health data for the whole day of today (**2023-05-26**) call `get_health_data_type(date: ZonedDateTime)`
where date is obtained with:

```kotlin
val date = ZonedDateTime.now() // Today 2023-05-26T15:12:20
    .truncatedTo(ChronoUnit.DAYS) // Today 2023-05-26T00:00:00
    .withZoneSameInstant(ZoneId.of("UTC")) // Today 2023-05-26T06:00:00Z
```

As you can see we started obtaining **today's**, and because we want health data from the entire day, all fields
after `DAYS` were truncated to zero.

Finally, the date was converted to UTC resulting in **2023-05-26T00:00:00Z** it has **06:00:00** because
**2023-05-26T00:00:00** in Mexico City is **2023-05-26T06:00:00Z** in UTC.

Internally the SDK will perform the following operations:

1. Use 2023-05-26T06:00:00Z as `start`
2. Create the range between `start` and the end of the day `start` belongs to.

So the time range will end up like:

`start`: 2023-05-26T06:00:00Z < --- >`end`: 2023-05-27T06:00:00Z

Which in **Mexico City (UTC-6)** is equivalent to:

`start`: 2023-05-26T00:00:00 < --- >`end`: 2023-05-27T00:00:00

##### Retrieve health data for the rest of the day of today

Today is **2023-05-26** in Mexico City (**UTC-6**)

To retrieve health data for the rest of the day of today (**2023-05-26**)
call `get_health_data_type(date: ZonedDateTime)` where date is obtained with:

```kotlin
val date = ZonedDateTime.now() // Today 2023-05-26T12:00:00
    .withZoneSameInstant(ZoneId.of("UTC")) // Today 2023-05-26T18:00:00Z
```

In this case the date was only converted to UTC resulting in **2023-05-26T18:00:00Z** it has **18:00:00** because
**2023-05-26T12:00:00** in Mexico City is **2023-05-26T18:00:00Z** in UTC.

Internally the SDK will perform the following operations:

1. Use 2023-05-26T18:00:00Z as `start`
2. Create the range between `start` and the end of the day `start` belongs to

So the time range will end up like:

`start`: 2023-05-26T18:00:00Z < --- >`end`: 2023-05-27T06:00:00Z

Which in **Mexico City (UTC-6)** is equivalent to:

`start`: 2023-05-26T12:00:00 < --- >`end`: 2023-05-27T00:00:00

#### Retrieve summaries

To retrieve any type of summary, you need to provide a date. This date cannot be the current
day and cannot be older than 29 days. See the examples below:

| Current date | Provided date | Is valid?                          |
|--------------|---------------|------------------------------------|
| 2023-01-08   | 2023-01-08    | No, the date is from today         |
| 2023-01-08   | 2023-01-07    | Yes, the date is from yesterday    |
| 2023-01-08   | 2022-11-01    | No, the date is older than 29 days |
| 2023-01-08   | 2023-01-01    | Yes, the date is 7 days old        |

To get health data, call `get_data_type` and provide a ZonedDateTime instance of the day you want to retrieve the data
from.

For example, if you want to get yesterday's sleep summary, call `getSleepSummary`. It will return
an `SleepSummary` instance or throw an exception if an error happens.

```kotlin
fun getSleepSummary() {
    scope.launch {
        try {
            val date = ZonedDateTime.now()
                .minusDays(1)
                .truncatedTo(ChronoUnit.DAYS)
                .withZoneSameInstant(ZoneId.of("UTC"))

            val result = manager.getSleepSummary(date)

            // Success
        } catch (e: Exception) {
            // Manage error
        }
    }
}
```

##### Keeping track of the last date a summary was retrieved

Health Connect does not allow retrieving data on background, so every time your users open your app
you should retrieve the data manually to help you retrieve the data of the days the user did not
open your app. We store in preferences the last date data was retrieved.

Call `getLastExtractionDate(rookDataType: HCRookDataType)` providing a `HCRookDataType`, e.g. if you want to retrieve
the last date a `SleepSummary` was retrieved use `HCRookDataType.SLEEP_SUMMARY`.

It will return a ZonedDateTime instance.

Rules:

* The returned ZonedDateTime will be in UTC.
* The returned ZonedDateTime will be the same you provided when retrieving health data.
* If the stored date is older than 29 days it will be ignored it and a DateNotFoundException will be thrown.
* If your users are more likely to change locations (timezones), you may have to instead convert the result
  of `getLastExtractionDate(rookDataType: HCRookDataType)` to your user's new timezone, add one day, truncate (if
  necessary) and convert again to UTC.

##### Last extraction date of a sleep summary example

Let's suppose that one of your users opens the app on `2023-01-10`, the app then retrieves a sleep
summary from yesterday (`2023-01-09`) with `getSleepSummary`.

Then the user forgets to open the app until `2023-01-15`, then you'll
call `getLastExtractionDate(rookDataType: HCRookDataType)`it will return `2023-01-09` in a ZonedDateTime instance. Now,
in a loop, you can recover data from the days the user did not open the app (`2023-01-10` to `2023-01-14`).

An example using sleep summaries is detailed below:

```kotlin
fun recoverLostDays() {
    scope.launch {
        val today = ZonedDateTime.now()
            .truncatedTo(ChronoUnit.DAYS)
            .withZoneSameInstant(ZoneId.of("UTC"))

        var date = manager.getLastExtractionDate(HCRookDataType.SLEEP_SUMMARY)

        date = date.plusDays(1)

        while (date.isBefore(today)) {
            try {
                val result = manager.getSleepSummary(date)

                // Success
            } catch (e: Exception) {
                // Manage error
            }

            date = date.plusDays(1)
        }
    }
}
```

#### Retrieve events

To retrieve any type of event, you need to provide a date. This date cannot be older than 29 days. See the examples
below:

| Current date | Provided date | Is valid?                          |
|--------------|---------------|------------------------------------|
| 2023-01-08   | 2023-01-08    | Yes, the date is from today        |
| 2023-01-08   | 2023-01-07    | Yes, the date is from yesterday    |
| 2023-01-08   | 2022-11-01    | No, the date is older than 29 days |
| 2023-01-08   | 2023-01-01    | Yes, the date is 7 days old        |

To get health data, call `get_data_type` and provide a ZonedDateTime instance of the day you want to retrieve the data
from.

For example, if you want to get today's physical events, call `getPhysicalEvents`. It will return
an `List<HCPhysicalEvent>` or throw an exception if an error happens or `RecordsNotFoundException` if there is no
physical data on that day.

```kotlin
fun getPhysicalEvents() {
    scope.launch {
        try {
            val date = ZonedDateTime.now()
                .truncatedTo(ChronoUnit.DAYS)
                .withZoneSameInstant(ZoneId.of("UTC"))

            val result = manager.getPhysicalEvents(date)

            // Success
        } catch (e: Exception) {
            // Manage error
        }
    }
}
```

##### Keeping track of the last date an event was retrieved

Health Connect does not allow retrieving data on background, so every time your users open your app
you should retrieve the data manually to help you retrieve the data of the days the user did not
open your app. We store in preferences the last date data was retrieved.

Call `getLastExtractionDate(rookDataType: HCRookDataType)` providing a `HCRookDataType`, e.g. if you want to retrieve
the last date a `PhysicalEvent` was retrieved use `HCRookDataType.PHYSICAL_EVENT`.

It will return a ZonedDateTime instance.

Rules:

* The returned ZonedDateTime will be in UTC.
* The returned ZonedDateTime will be equal to the date of the last event found.
* If the stored date is older than 29 days it will be ignored it and a DateNotFoundException will be thrown.
* If your users are more likely to change locations (timezones), you may have to instead convert the result
  of `getLastExtractionDate(rookDataType: HCRookDataType)` to your user's new timezone, add one day, truncate (if
  necessary) and convert again to UTC.

##### Last extraction date of a physical event example

Let's suppose that one of your users opens the app on `2023-01-10`, the app then retrieves physical events from
today (`2023-01-10`) with `getPhysicalEvents`.

Then the user forgets to open the app until `2023-01-15`, then you'll
call `getLastExtractionDate(rookDataType: HCRookDataType)`it will return `2023-01-10` in a ZonedDateTime instance. Now,
in a loop, you can recover data from the days the user did not open the app (`2023-01-10` to `2023-01-14`).

An example using physical events is detailed below:

```kotlin
fun recoverLostDays() {
    scope.launch {
        val today = ZonedDateTime.now()
            .truncatedTo(ChronoUnit.DAYS)
            .withZoneSameInstant(ZoneId.of("UTC"))

        var date = manager.getLastExtractionDate(HCRookDataType.PHYSICAL_EVENT)

        // date = date.plusDays(1) Not necessary the returned date belongs to the last event found

        while (date.isBefore(today)) {
            try {
                val result = manager.getPhysicalEvents(date)

                // Success
            } catch (e: Exception) {
                // Manage error
            }

            date = date.plusDays(1)
        }
    }
}
```

## Other resources

* See a complete list of `RookHealthConnectManager` functions in
  the [Javadoc](https://www.javadoc.io/doc/com.rookmotion.android/rook-health-connect/latest/com/rookmotion/rook/health_connect/RookHealthConnectManager.html)
