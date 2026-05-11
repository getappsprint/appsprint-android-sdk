# AppSprint Android SDK

Lightweight mobile attribution SDK for Android. Tracks installs, events, and campaign attribution with offline support.

## Installation

### Gradle (Maven Central)

Add to your app's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("app.appsprint:sdk:1.0.0")
}
```

Make sure `mavenCentral()` is in your repositories:

```kotlin
repositories {
    mavenCentral()
}
```

### Manual AAR

Download the AAR from [Releases](https://github.com/getappsprint/appsprint-android-sdk/releases) and add to your project's `libs/` directory:

```kotlin
dependencies {
    implementation(files("libs/appsprint-sdk.aar"))
    implementation("androidx.lifecycle:lifecycle-process:2.8.7")
}
```

## Quick Start

```kotlin
import com.appsprint.sdk.AppSprint
import com.appsprint.sdk.AppSprintConfig

// Configure on app launch (in Application.onCreate or Activity)
val config = AppSprintConfig(apiKey = "as_live_your_api_key")
AppSprint.shared(context).configure(config)

// Track events
AppSprint.shared(context).sendEvent(
    AppSprintEventType.PURCHASE,
    name = "premium_upgrade",
    params = mapOf("price" to 9.99)
)

// Get attribution
val attribution = AppSprint.shared(context).getAttribution()
println("Source: ${attribution?.source}")
```

## Configuration

```kotlin
val config = AppSprintConfig(
    apiKey = "as_live_...",                              // Required
    apiUrl = "https://api.appsprint.app",                // Default
    enableAppleAdsAttribution = true,                    // Default (no-op on Android)
    isDebug = false,                                     // Default
    logLevel = 2,                                        // 0=debug, 1=info, 2=warn, 3=error
    customerUserId = null                                // Optional
)
```

## API Reference

```kotlin
// Lifecycle
AppSprint.shared(context).configure(config: AppSprintConfig)
AppSprint.shared(context).destroy()
AppSprint.shared(context).isInitialized(): Boolean

// Events
AppSprint.shared(context).sendEvent(type: AppSprintEventType, name: String?, params: Map?)
AppSprint.shared(context).sendTestEvent(): TestEventResult
AppSprint.shared(context).flush()

// Attribution
AppSprint.shared(context).getAttribution(): AttributionResult?
AppSprint.shared(context).getAppSprintId(): String?

// User
AppSprint.shared(context).setCustomerUserId(userId: String)

// State
AppSprint.shared(context).isSdkDisabled(): Boolean
AppSprint.shared(context).clearData()
```

### Event Types

`LOGIN`, `SIGN_UP`, `REGISTER`, `PURCHASE`, `SUBSCRIBE`, `START_TRIAL`, `ADD_TO_CART`, `ADD_TO_WISHLIST`, `INITIATE_CHECKOUT`, `VIEW_CONTENT`, `VIEW_ITEM`, `SEARCH`, `SHARE`, `TUTORIAL_COMPLETE`, `LEVEL_START`, `LEVEL_COMPLETE`, `CUSTOM`

## Requirements

- Android API 24+ (Android 7.0)
- Kotlin 1.8+ or Java 17+

## License

MIT License. See [LICENSE](LICENSE) for details.
