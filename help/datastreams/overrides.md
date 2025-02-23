---
title: Configure datastream overrides
description: Learn how to configure datastream overrides in the Datastreams UI and activate them via the Web SDK.
exl-id: 3f17a83a-dbea-467b-ac67-5462c07c884c
---
# Configure datastream overrides

Datastream overrides allow you to define additional configurations for your datastreams, which get passed to the Edge Network via the Web SDK.

This helps you trigger different datastream behaviors than the default ones, without creating a new datastream or modifying your existing settings.

Datastream configuration override is a two step process:

1. First, you must define your datastream configuration overrides in the [datastream configuration page](configure.md).
2. Then, you must send the overrides to the Edge Network in one of the following ways:
    * Through the `sendEvent` or `configure` [Web SDK](#send-overrides-web-sdk) commands.
    * Through the Web SDK [tag extension](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).
    * Through the Mobile SDK [sendEvent API](#send-overrides-mobile-sdk) call.

This article explains the end-to-end datastream configuration override process for every type of supported override.

>[!IMPORTANT]
>
>Datastream overrides are only supported for [Web SDK](../edge/home.md) and [Mobile SDK](https://developer.adobe.com/client-sdks/documentation/) integrations. [Server API](../server-api/overview.md) integrations do not currently support datastream overrides.
><br>
>Datastream overrides should be used when you need different data sent to different datastreams. You should not use datastream overrides for personalization use cases or consent data.

## Use cases {#use-cases}

To help you better understand how and when to use datastream overrides, here are some use cases that Adobe Experience Platform customers can solve by using this feature.

**Multi-region data collection**

A company has different websites or subdomains for different countries in which they operate. They have [configured](configure.md) separate datastreams with corresponding analytics-specific report suites, country-specific Adobe Target property tokens, country-specific schemas, datasets, Journey Optimizer configurations, and so on. The company also has a global set of configurations where all country-specific data is aggregated.

By using datastream overrides, the company can dynamically switch the flow of data to different datastreams, instead of the default behavior of sending data to one datastream.

A common use case could be to send data to a country-specific datastream and also send data to a global datastream where customers perform an important action, such as placing an order or updating their user profile.

**Differentiating profiles and identities for different business units**

A company with multiple business units wants to use multiple Experience Platform sandboxes to store data specific to each business unit.

Instead of sending data to a default datastream, the company can use datastream overrides to make sure each business unit has its own datastream to receive data through.

## Configure datastream overrides in the Datastreams UI {#configure-overrides}

Datastream configuration overrides allow you to modify the following datastream configurations:

* Experience Platform event datasets
* Adobe Target property tokens
* Audience Manager ID sync containers
* Adobe Analytics report suites

### Datastream overrides for Adobe Target {#target-overrides}

To configure datastream overrides for an Adobe Target datastream, you must first have an Adobe Target datastream created. Follow the instructions to [configure a datastream](configure.md) with the [Adobe Target](configure.md#target) service.

Once you've created the datastream, edit the [Adobe Target](configure.md#target) service that you have added and use the **[!UICONTROL Property Token Overrides]** section to add the desired datastream overrides, as shown in the image below. Add one property token per line.

![Datastreams UI screenshot showing the Adobe Target service settings, with the property token overrides highlighted.](assets/overrides/override-target.png)

After you've added the desired overrides, save your datastream settings.

You should now have the Adobe Target datastream overrides configured. Now you can [send the overrides to the Edge Network via the Web SDK](#send-overrides).

### Datastream overrides for Adobe Analytics {#analytics-overrides}

To configure datastream overrides for an Adobe Analytics datastream, you must first have an [Adobe Analytics](configure.md#analytics) datastream created. Follow the instructions to [configure a datastream](configure.md) with the [Adobe Analytics](configure.md#analytics) service.

Once you've created the datastream, edit the [Adobe Analytics](configure.md#target) service that you have added and use the **[!UICONTROL Report Suite Overrides]** section to add the desired datastream overrides, as shown in the image below.

Select **[!UICONTROL Show Batch Mode]** to enable batch editing of the report suite overrides. You can copy and paste a list of report suite overrides, entering one report suite per line.

![Datastreams UI screenshot showing the Adobe Analytics service settings, with the report suite overrides highlighted.](assets/overrides/override-analytics.png)

After you've added the desired overrides, save your datastream settings.

You should now have the Adobe Analytics datastream overrides configured. Now you can [send the overrides to the Edge Network via the Web SDK](#send-overrides).

### Datastream overrides for Experience Platform event datasets {#event-dataset-overrides}

To configure datastream overrides for Experience Platform event datasets, you must first have an [Adobe Experience Platform](configure.md#aep) datastream created. Follow the instructions to [configure a datastream](configure.md) with the [Adobe Experience Platform](configure.md#aep) service.

Once you've created the datastream, edit the [Adobe Experience Platform](configure.md#aep) service that you have added and select the **[!UICONTROL Add Event Dataset]** option to add one or more override event datasets, as shown in the image below.

![Datastreams UI screenshot showing the Adobe Experience Platform service settings, with the event dataset overrides highlighted.](assets/overrides/override-aep.png)

After you've added the desired overrides, save your datastream settings.

You should now have the Adobe Experience Platform datastream overrides configured. Now you can [send the overrides to the Edge Network via the Web SDK](#send-overrides).

### Datastream overrides for third party ID sync containers {#container-overrides}

To configure datastream overrides for third party ID sync containers, you must first have a datastream created. Follow the instructions to [configure a datastream](configure.md) to create one.

Once you've created the datastream, go to **[!UICONTROL Advanced Options]** and enable the **[!UICONTROL Third Party ID Sync]** option.

Then, use the **[!UICONTROL Container ID Overrides]** section to add the container IDs that you want to override the default setting, as shown in the image below.

>[!IMPORTANT]
>
>Container IDs must be numeric values, like `1234567`, and not strings, such as `"1234567"`. If you send a string value through the Web SDK as a container ID override, you will receive an error.

![Datastreams UI screenshot showing the datastream settings, with the third party ID sync container overrides highlighted.](assets/overrides/override-container.png)

After you've added the desired overrides, save your datastream settings.

You should now have the ID sync container overrides configured. Now you can [send the overrides to the Edge Network via the Web SDK](#send-overrides).

## Send the overrides to the Edge Network via the Web SDK {#send-overrides-web-sdk}

>[!NOTE]
>
>As an alternative to sending the configuration overrides via Web SDK commands, you can add the configuration overrides to the Web SDK [tag extension](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

After [configuring the datastream overrides](#configure-overrides) in the Data Collection UI, you can now send the overrides to the Edge Network, via the Web SDK.

If you are using Web SDK, sending the overrides to the Edge Network via the `edgeConfigOverrides` command is the second and final step of activating datastream configuration overrides.

The datastream configuration overrides are sent to the Edge Network through the `edgeConfigOverrides` Web SDK command. This command creates datastream overrides that are passed on to the [!DNL Edge Network] on the next command, or, in the case of the `configure` command, for every request.

The `edgeConfigOverrides` command creates datastream overrides that are passed on to the [!DNL Edge Network] on the next command, or, in the case of `configure`, for every request.

When a configuration override is sent with the `configure` command, it is included on the following Web SDK commands.

* [sendEvent](../edge/fundamentals/tracking-events.md)
* [setConsent](../edge/consent/iab-tcf/overview.md)
* [getIdentity](../edge/identity/overview.md)
* [appendIdentityToUrl](../edge/identity/id-sharing.md#cross-domain-sharing)
* [configure](../edge/fundamentals/configuring-the-sdk.md)

Options specified globally can be overridden by the configuration option on individual commands.

### Sending configuration overrides via the Web SDK `sendEvent` command {#send-event}

The example below shows what a configuration override could look like on a `sendEvent` command.

```js {line-numbers="true" highlight="5-25"}
alloy("sendEvent", {
  xdm: {
    /* ... */
  },
  edgeConfigOverrides: {
    datastreamId: "{DATASTREAM_ID}"
    com_adobe_experience_platform: {
      datasets: {
        event: {
          datasetId: "SampleEventDatasetIdOverride"
        }
      }
    },
    com_adobe_analytics: {
      reportSuites: [
        "MyFirstOverrideReportSuite",
        "MySecondOverrideReportSuite",
        "MyThirdOverrideReportSuite"
        ]
    },
    com_adobe_identity: {
      idSyncContainerId: "1234567"
    },
    com_adobe_target: {
      propertyToken: "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
    }
  }
});
```

|Parameter|Description|
|---|---|
|`edgeConfigOverrides.datastreamId`| Use this parameter to allow a single request to go to a different datastream than the one defined by the `configure` command. |

### Sending configuration overrides via the Web SDK `configure` command {#send-configure}

The example below shows what a configuration override could look like on a `configure` command.

```js {line-numbers="true" highlight="8-30"}
alloy("configure", {
  defaultConsent: "in",
  edgeDomain: "etc",
  edgeBasePath: "ee",
  datastreamId: "{DATASTREAM_ID}",
  orgId: "org",
  debugEnabled: true,
  edgeConfigOverrides: {
    "com_adobe_experience_platform": {
      "datasets": {
        "event": {
          datasetId: "SampleProfileDatasetIdOverride"
        }
      }
    },
    "com_adobe_analytics": {
      "reportSuites": [
        "MyFirstOverrideReportSuite",
        "MySecondOverrideReportSuite",
        "MyThirdOverrideReportSuite"
      ]
    },
    "com_adobe_identity": {
      "idSyncContainerId": "1234567"
    },
    "com_adobe_target": {
      "propertyToken": "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
    }
  },
  onBeforeEventSend: function() { /* … */ });
};
```

## Send the overrides to the Edge Network via the Mobile SDK {#send-overrides-mobile-sdk}

After [configuring the datastream overrides](#configure-overrides) in the Data Collection UI, you can now send the overrides to the Edge Network, via the Mobile SDK.

If you are using the Mobile SDK, sending the overrides to the Edge Network via the `sendEvent` API is the second and final step of activating datastream configuration overrides.

For more information about the Experience Platform Mobile SDK, see the [Mobile SDK documentation](https://developer.adobe.com/client-sdks/edge/edge-network/).

### Datastream ID override via Mobile SDK {#id-override-mobile}

The examples below show what a datastream ID override could look like on a Mobile SDK integration. Select the tabs below to see the [!DNL iOS] and [!DNL Android] examples.

>[!BEGINTABS]

>[!TAB iOS (Swift)]

This example shows what a datastream ID override looks like in a Mobile SDK [!DNL iOS] integration.

```swift
// Create Experience event from dictionary
var xdmData: [String: Any] = [
  "eventType": "SampleXDMEvent",
  "sample": "data",
]
let experienceEvent = ExperienceEvent(xdm: xdmData, datastreamIdOverride: "SampleDatastreamId")

Edge.sendEvent(experienceEvent: experienceEvent) { (handles: [EdgeEventHandle]) in
  // Handle the Edge Network response
}
```

>[!TAB Android (Kotlin)]

This example shows what a datastream ID override looks like in a Mobile SDK [!DNL Android] integration.

```kotlin
// Create experience event from Map
val xdmData = mutableMapOf < String, Any > ()
xdmData["eventType"] = "SampleXDMEvent"
xdmData["sample"] = "data"

val experienceEvent = ExperienceEvent.Builder()
    .setXdmSchema(xdmData)
    .setDatastreamIdOverride("SampleDatastreamId")
    .build()

Edge.sendEvent(experienceEvent) {
    // Handle the Edge Network response
}
```

>[!ENDTABS]

### Datastream configuration override via Mobile SDK {#config-override-mobile}

The examples below show what a datastream configuration override could look like on a Mobile SDK integration. Select the tabs below to see the [!DNL iOS] and [!DNL Android] examples.

>[!BEGINTABS]

>[!TAB iOS (Swift)]

This example shows what a datastream configuration override looks like in a Mobile SDK [!DNL iOS] integration.

```swift
// Create Experience event from dictionary
var xdmData: [String: Any] = [
  "eventType": "SampleXDMEvent",
  "sample": "data",
]

let configOverrides: [String: Any] = [
  "com_adobe_experience_platform": [
    "datasets": [
      "event": [
        "datasetId": "SampleEventDatasetIdOverride"
      ]
    ]
  ],
  "com_adobe_analytics": [
  "reportSuites": [
        "MyFirstOverrideReportSuite",
          "MySecondOverrideReportSuite",
          "MyThirdOverrideReportSuite"
      ]
  ],
  "com_adobe_identity": [
    "idSyncContainerId": "1234567"
  ],
  "com_adobe_target": [
    "propertyToken": "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
 ],
]

let experienceEvent = ExperienceEvent(xdm: xdmData, datastreamConfigOverride: configOverrides)

Edge.sendEvent(experienceEvent: experienceEvent) { (handles: [EdgeEventHandle]) in
  // Handle the Edge Network response
}
```

>[!TAB Android (Kotlin)]

This example shows what a datastream configuration override looks like in a Mobile SDK [!DNL Android] integration.

```kotlin
// Create experience event from Map
val xdmData = mutableMapOf < String, Any > ()
xdmData["eventType"] = "SampleXDMEvent"
xdmData["sample"] = "data"

val configOverrides = mapOf(
    "com_adobe_experience_platform"
    to mapOf(
        "datasets"
        to mapOf(
            "event"
            to mapOf("datasetId"
                to "SampleEventDatasetIdOverride")
        )
    ),
    "com_adobe_analytics"
    to mapOf(
        "reportSuites"
        to listOf(
            "MyFirstOverrideReportSuite",
            "MySecondOverrideReportSuite",
            "MyThirdOverrideReportSuite"
        )
    ),
    "com_adobe_identity"
    to mapOf(
        "idSyncContainerId"
        to "1234567"
    ),
    "com_adobe_target"
    to mapOf(
        "propertyToken"
        to "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
    )
)

val experienceEvent = ExperienceEvent.Builder()
    .setXdmSchema(xdmData)
    .setDatastreamConfigOverride(configOverrides)
    .build()

Edge.sendEvent(experienceEvent) {
    // Handle the Edge Network response
}
```

>[!ENDTABS]

## Payload example {#payload-example}

The examples above generate an [!DNL Edge Network] payload similar to the one below.

```json
{
  "meta": {
    "configOverrides": {
      "com_adobe_experience_platform": {
        "datasets": {
          "event": {
            "datasetId": "SampleProfileDatasetIdOverride"
          }
        }
      },
      "com_adobe_analytics": {
        "reportSuites": [
        "MyFirstOverrideReportSuite",
        "MySecondOverrideReportSuite",
        "MyThirdOverrideReportSuite"
        ]
      },
      "com_adobe_identity": {
        "idSyncContainerId": "1234567"
      },
      "com_adobe_target": {
        "propertyToken": "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
      }
    },
    "state": {  }
  },
  "events": [  ]
}
```
