# Hubitat Connector WLAN Integration Driver

## Overview

This is an unofficial, community-developed Hubitat Elevation driver for controlling motorized window coverings that use the "Connector" brand Wi-Fi bridge (DD7006 compatible). It allows for direct integration of your blinds and shades into your Hubitat environment, enabling you to control them via the Hubitat UI, dashboards, and automation rules.

This driver was developed through a collaborative debugging process and incorporates a robust token refresh and command queuing system to ensure reliable communication with the bridge and devices, correcting for several undocumented quirks in the device's API.

## Features

-   **Automatic Discovery:** Automatically discovers and creates child devices for each blind/shade connected to the bridge.
-   **Reliable Communication:** Implements a command queue and proactive token refresh mechanism to handle the API's unique authentication and prevent `AccessToken` errors.
-   **Full Control:** Supports Open/Up, Close/Down, Stop, and setting a specific percentage position.
-   **Status Reporting:** Child devices report their last known position (`level` and `windowShade` capabilities).
-   **Configurable Logging:** Includes a toggle to switch between detailed debugging logs and clean, secure production logs.
-   **Stable State:** Prevents devices from being marked as "unknown" just from being idle, preserving their last known position in the UI.

## Prerequisites

1.  A Hubitat Elevation hub (any model).
2.  A compatible "Connector" Wi-Fi Bridge (e.g., DD7006) connected to your local network.
3.  The official "Connector" mobile app installed on your phone.
4.  Your blinds/shades already paired with the Wi-Fi bridge via the mobile app.

---

## Installation Instructions

Follow these steps carefully to install the driver and configure your devices.

### Step 1: Get Your Bridge IP Address and Key

You will need two pieces of information from your setup before installing the driver.

1.  **Bridge IP Address:**
    -   Find the IP address of your Connector Wi-Fi bridge. You can usually find this in your router's DHCP client list or by using a network scanning app.
    -   It is **highly recommended** to assign a **static IP address** (or DHCP reservation) to your bridge in your router's settings. This will prevent the IP from changing in the future, which would break the integration.

2.  **Bridge Key:**
    -   Open the "Connector" app on your mobile device.
    -   Navigate to the **"About"** page within the app.
    -   Quickly tap on the "About" page title or version number **5 times**.
    -   A "Reminder" popup will appear showing your unique **KEY**. It will be a 16-character string that includes hyphens (e.g., `79851734-00bc-44`).
    -   **Copy this key exactly as it appears**, including hyphens and respecting the case (it is likely all lowercase).

### Step 2: Install the Parent Driver Code

1.  In your web browser, navigate to your Hubitat hub's IP address.
2.  Click on **Drivers Code** in the left-hand menu.
3.  Click the **+ New Driver** button in the top-right corner.
4.  An editor window will appear. Copy the entire Groovy driver code from the `Connector-WLAN-Parent-Driver.groovy` file and paste it into this window, replacing any text that is already there.
5.  Click the **Save** button in the top-right corner. The driver is now installed on your hub.

### Step 3: Add the Virtual Device

1.  Click on **Devices** in the left-hand menu.
2.  Click the **+ Add Virtual Device** button in the top-right corner.
3.  In the `Device Name` field, give your bridge a logical name (e.g., "Blinds Bridge").
4.  In the `Type` dropdown menu, scroll all the way down to the "User" section at the bottom and select **Connector WLAN Integration (Full)**.
5.  Click the **Save Device** button.

### Step 4: Configure the Device

You will now be on the device page for the bridge you just created.

1.  In the "Preferences" section, enter the following:
    -   **Bridge IP:** The static IP address of your Connector bridge that you found in Step 1.
    -   **Bridge Key:** The 16-character key (with hyphens) that you got from the app in Step 1.
2.  Click the **Save Preferences** button.

### Step 5: Initialize and Discover Devices

This is the final and most important step.

1.  On the same device page, find the command buttons at the top.
2.  Click the **Initialize** button.
3.  Wait about 10-15 seconds. The driver will communicate with the bridge, acquire a token, and automatically create child devices for every blind it finds.
4.  Refresh the page. You should now see all of your blinds listed as child devices at the bottom of the page.

## Usage

Your installation is now complete!

-   You can control each blind individually from its own device page.
-   You can add these child devices to your Hubitat Dashboards using the "Shade" or "Window Shade" template.
-   You can use these devices in your automations with Rule Machine, Motion Lighting, etc., to open/close blinds based on time of day, motion, or other events.

## Troubleshooting

-   **Child devices are not created:**
    -   Verify the Bridge IP address is correct and that the Hubitat hub can reach it on your network.
    -   Check the logs for any errors after clicking `Initialize`.
-   **Commands are not working:**
    -   Verify the Bridge Key is entered *exactly* as it appears in the app (16 characters, with hyphens, lowercase).
    -   Go to the parent device page, turn on `Enable Debug Logging`, and save preferences. Try a command and check the logs for `AccessToken error`. This indicates a mismatch between your key and the token from the bridge.
-   **Device status shows "unknown":**
    -   This should no longer happen with the latest version of the driver. If it does, ensure you have the correct code installed. Click `Refresh` on the parent device page to get the latest status.
