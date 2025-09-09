# Bodylink SDK Documentation

## 1\. Overview

The Bodylink SDK is a plug-and-play Unity solution that provides reliable full-body user detection and gesture inputs directly from a device's camera. Designed for game developers, the SDK handles core setup tasks like camera permissions, calibration, and human presence checks, giving you a clean and intuitive API to work with. The goal is to provide a seamless way to integrate camera-based full-body inputs into your Unity project without extensive boilerplate code.

### What it Delivers

*   **Bodylink Prefab**: A single drag-and-drop prefab that acts as the primary entry point for integrating the SDK into your Unity scene.
*   **Singleton Access**: A globally accessible `BodylinkSDK.Bodylink` singleton that provides a central access point for runtime configuration and data.
*   **Configurable Settings**: A set of exposed configuration settings, accessible via the Unity Inspector and through code, that allow you to fine-tune the SDK's behavior.
*   **Lifecycle Methods**: A collection of runtime methods for manually controlling the SDK's initialization, calibration, and shutdown.
*   **Runtime Events**: A series of subscribable events that allow you to trigger custom game logic based on the SDK's status.
*   **Initial Setup Scene**: A pre-built scene that demonstrates an end-to-end user flow for human detection and a basic UI for calibration.

## 2\. Features & System Compatibility

### Key Features

*   **Plug-and-play Unity prefab**: A single drag-and-drop prefab (`Bodylink`) for easy integration.
*   **Automatic or manual calibration options**: Supports Head, Upper Body, Feet, and Full Body calibration.
*   **Gesture detection support**: Recognizes a variety of gestures, including Nod, Shake, Look Left/Right, Throw, and Swipe.
*   **Real-time runtime parameter access**: All parameters are easily accessible via the `Bodylink` singleton.
*   **Visibility-based recalibration system**: Automatically triggers recalibration if the user's body visibility falls below a certain threshold.
*   **Easy integration**: Designed for seamless integration with existing UI and custom workflows.

### Supported Platforms

*   **Unity**: Tested and recommended for Unity >= 2022.3 LTS and above.
*   **Bodylink and Android builds**: Supports a wide range of desktop and mobile builds.

### Requirements

*   **Unity**: Version 2022.3 LTS or newer.
*   **Bodylink SDK**

## 3\. Getting Started
### 3.1 Package installation

Import Bodylink SDK Package (Under development)

### 3.2 Installation

To begin, simply drag the `Bodylink` prefab into your Unity scene.

### 3.3 Singleton Access

The SDK is built around a powerful singleton pattern, making its core functionality easily accessible from any script in your project.

```csharp
Bodylink.Instance.Calibrate();
```

## 4\. Configuration

The following settings can be adjusted in the Inspector or programmatically.

| Setting Name | Description | Default |
| --- | --- | --- |
| `initializeOnStart` | If enabled, the SDK will automatically initialize. | `true` |
| `calibrateOnStart` | If enabled, the SDK will automatically trigger a calibration. | `true` |
| `visibilityThreshold` | Defines how sensitive tracking is to off-screen body points. | `0.5 sec` |
| `visibilityWaitingTime` | The delay before an auto-recalibration is triggered. | `5.0 sec` |
| `calibrationType` | Specifies the type of calibration to perform. | `Full Body` |

## 5\. Runtime Methods

The SDK provides a set of runtime methods for programmatic control.

*   **Initialize()**: Manually initializes the SDK. The corresponding C# method is `public void Initialize(Action callback = null)`.
*   **Calibrate()**: Programmatically triggers a calibration sequence. The corresponding C# method is `public void Calibrate(Action callback = null)`.
*   **Dispose()**: Shuts down the SDK's pipeline and releases all resources. The corresponding C# method is `public void Dispose()`.
*   **SelectSource(SourceType source)**: Allows you to select the input source for body tracking. The corresponding C# method is `public void SelectSource(int index)`.

## 6\. Runtime Events

The SDK exposes several events you can subscribe to.

| Event Name | Description |
| --- | --- |
| `OnInitialized` | Fired when the SDK has completed its initialization process. |
| `OnCalibrated` | Fired after a calibration sequence has successfully completed. |
| `OnDisposed` | Fired when the SDK has been cleanly shut down. |
| `OnPlayerOutOfScreen` | Fired when the tracked player leaves the camera view. |

### Example Event Subscription

```csharp
// Example: Subscribing to an event
// This method sets up a listener for the OnInitialized event and logs a message.
Bodylink.Instance.OnInitialized += HandleInitializationComplete;

private void HandleInitializationComplete()
{
    Debug.Log("Bodylink SDK has been initialized!");
}
```
## 7\. Core Components

The SDK's core functionality is built on the following key scripts.

### BodylinkCameraFinder.cs

This script automatically finds the active camera in the scene and assigns it to the UI Canvas. This is essential for ensuring that the UI overlay, including the camera feed, is correctly rendered on top of the 3D scene.

| Function Name | Description | Parameters |
| --- | --- | --- |
| `Awake()` | Finds the main camera and assigns it to the Canvas component at startup. Logs an error if a main camera or a Canvas component is not found. | None. |
| `Update()` | Continuously checks and updates the Canvas's world camera if the main camera changes or if it was not found on Awake. | None. |

### BodylinkPoseLandmarkListAnnotion.cs

This component is responsible for connecting to the Mediapipe tracking pipeline. It waits for the \`PointListAnnotation\` component, which contains the raw tracking data, and makes this data available to other parts of the SDK via a static event.

| Function Name | Description | Parameters |
| --- | --- | --- |
| `Start()` | A coroutine that finds the \`PointListAnnotation\` component and invokes a static Action to notify the SDK that tracking data is available. | None. |
| `OnPointListAnnotation` | A static \`Action\` event that other scripts, like \`Bodylink\`, can subscribe to in order to receive the raw \`PointListAnnotation\` data. | `pointListAnnotation (PointListAnnotation)`: The data containing the list of body landmarks. |

### BodylinkAvatar.cs

This script manages the visual representation of the user's body landmarks during calibration and also controls the mini-camera view. It listens to the Mediapipe output to update the on-screen body points in real-time.

| Function Name | Description | Parameters |
| --- | --- | --- |
| `Start()` | Initializes the avatar and subscribes to the pose landmark detection event from the \`PoseLandmarkerRunnerInstance\`. | None. |
| `OnPoseLandmarkDetectionOutput()` | An event handler that processes the real-time pose landmark data and updates the on-screen visuals accordingly. | `result (PoseLandmarkerResult)`: The result object containing the detected body landmarks. |
| `Callibrate()` | Manages the calibration process, including the progress bar and displaying the static calibration points. | `calibrationType (BodylinkCalibrationType)`: The type of calibration to perform.  <br>`callback (Action)`: A callback to be executed upon completion.  <br>`time (float)`: The duration of the calibration process. |
| `ShowCamerainFullScreen()` | Toggles the visibility of the main camera feed in full-screen mode. | `status (bool)`: \`true\` to show, \`false\` to hide. |

## 8\. The Core Bodylink Script (Bodylink.cs)

This is the central singleton script for the Bodylink SDK, providing a single point of access to manage the entire lifecycle, from initialization to disposal. It handles the core logic for controlling the SDK's state and public API.

| Function Name | Description | Parameters |
| --- | --- | --- |
| `Initialize()` | Manually initializes the Bodylink SDK. This sets up the Mediapipe pipeline and prepares the avatar. | `callback (Action)`: An optional callback to be executed once initialization is complete. |
| `Calibrate()` | Programmatically triggers a calibration sequence. This is essential for setting the user's body as the reference point for gestures and movements. | `callback (Action)`: An optional callback to be executed once calibration is complete. |
| `Dispose()` | Shuts down the SDK, disposes of all resources, and stops the Mediapipe pipeline. This should be called when the SDK is no longer needed. | None. |
| `SelectSource()` | Allows you to switch the input camera source for body tracking. | `index (int)`: The index of the camera source to be used. |

## 9\. Project Architecture

The project architecture is built around the **BodylinkSDK** namespace. The core functionality is managed by a singleton, making it easily accessible.

### Internal Components (For reference)

*   **BodylinkScene**: The initial scene used for testing and validation.
*   **BodylinkTesting**: A script containing a demo UI for internal validation and testing.

