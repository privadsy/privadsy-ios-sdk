# Privadsy iOS SDK

Show Privadsy ads in your iOS app.

## Requirements

- iOS 13.0+
- Xcode 15+

## Installation

The SDK is distributed as a binary via Swift Package Manager.

### Xcode

1. **File → Add Package Dependencies…**
2. Enter `https://github.com/privadsy/privadsy-ios-sdk`
3. Choose **Up to Next Major Version** and add the `PrivadsySDK` library to your app target.

### Package.swift

```swift
dependencies: [
    .package(url: "https://github.com/privadsy/privadsy-ios-sdk", from: "1.0.0")
]
```

and add `"PrivadsySDK"` to your target's dependencies.

## Configuration

1. Copy [`PrivadsySDK-Info.plist`](PrivadsySDK-Info.plist) into your project and add it to your app target, so it ends up in the app bundle.
2. Set `PRIVADSY_SDK_TOKEN` to the token you received from Privadsy.

| Key | Type | Description |
| --- | --- | --- |
| `PRIVADSY_SDK_HOST` | String | Privadsy API host |
| `PRIVADSY_SDK_TOKEN` | String | Your app's API token |
| `PRIVADSY_SDK_CACHE_DAYS` | Number | Days to keep cached ad images |
| `PRIVADSY_SDK_CUSTOM_IDS` | Array | Custom IDs sent with ad requests |

If the file is missing or invalid, the SDK logs a configuration error and shows no ads.

## Usage

### Showing an AdView

```swift
import PrivadsySDK
```

Create a `PrivadsyAdView` in code with a `PrivadsyAdViewConfig`:

| Parameter | Description |
| --- | --- |
| `identifier` | Identifier of this AdView, used for image caching. Must be unique within your app. |
| `maxHistoryItems` | Maximum number of ads visible/scrollable in the AdView. Filled with each `loadItem` call. |
| `adCountLoadOnInit` | Number of ads loaded when the view is created (min 1). |
| `adCountLoadNext` | Number of ads loaded with every `loadItem` call (min 1). |
| `countToShow` | Maximum number of ads to scroll through after a `loadItem` call. `0` = scroll infinitely, `1` = show one new ad without scrolling, `2+` = scroll to x new ads, each shown for `delayBetweenAds`. |
| `delayBetweenAds` | Time in ms before switching to the next ad. Minimum 1600 to compensate for navigation delays. |
| `adChangeAnimated` | Whether the ad change is animated by scrolling. |

- After creation, the AdView loads `adCountLoadOnInit` ads into its cache. You can create it at app launch to preload ads for later.
- The AdView loads `adCountLoadNext` new ads on every `viewDidAppear` of its view controller.
- To react to network or server errors, set a `PrivadsyAdViewDelegate` via `setDelegate` on the AdView. The callback `changeAdVisibility` delivers the AdView identifier and whether the AdView should be visible.

Using `PrivadsyAdView` via Storyboard is not supported.

### Labels

To use labels, implement `PrivadsyCallbackDelegate` and pass it to the SDK:

```swift
PrivadsyConnection.sharedInstance().setPrivadsySettings(delegate: yourDelegate)
```

Return the labels from `getPrivadsyLabels()`.

## Privacy

The SDK ships a privacy manifest (`PrivacyInfo.xcprivacy`), which Xcode includes in your app's privacy report.

## Versions

See [Releases](https://github.com/privadsy/privadsy-ios-sdk/releases).
