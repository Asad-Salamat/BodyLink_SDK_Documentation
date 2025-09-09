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

## 7\. Project Architecture

The project architecture is built around the **BodylinkSDK** namespace. The core functionality is managed by a singleton, making it easily accessible.

### Internal Components (For reference)

*   **BodylinkScene**: The initial scene used for testing and validation.
*   **BodylinkTesting**: A script containing a demo UI for internal validation and testing.

## 8\. Licensing and Credits

This SDK utilizes the **Mediapipe Unity Plugin** for body landmark detection, which is maintained by [homuler](#). We are grateful for his work in making this functionality available to the Unity community.

### MIT License

    Copyright (c) 2025 homuler
    
    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.
