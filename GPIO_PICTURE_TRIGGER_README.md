# GPIO Picture Trigger Feature

This feature adds the ability to trigger a GPIO output when a picture is taken, which can be used to control external devices like strobe lights, relays, or other equipment.

## Overview

The GPIO Picture Trigger feature allows you to:
- Enable/disable GPIO triggering when pictures are taken
- Configure which GPIO pin to use (GPIO 12 or 13 recommended)
- Set a delay before taking the picture after GPIO activation
- Configure how long the GPIO stays active

## Timing: How PictureTriggerDelay and WaitBeforeTakingPicture Work Together

There are two relevant timing parameters:

- **PictureTriggerDelay**: How long before the picture is taken the GPIO is activated. This is the lead time for the GPIO trigger before the actual capture process begins.
- **WaitBeforeTakingPicture**: The original parameter that controls how long the system waits (in seconds) before actually taking the picture after the picture-taking process is initiated. This is typically used for camera or lighting stabilization.

### Sequence

1. **GPIO is activated**
2. Wait for `PictureTriggerDelay` seconds
3. Begin camera's pre-capture process (which includes `WaitBeforeTakingPicture`)
4. Wait for `WaitBeforeTakingPicture` seconds
5. Take the picture
6. GPIO is deactivated after the configured duration (`PictureTriggerDuration`)

**Total time from GPIO activation to picture capture:**

    PictureTriggerDelay + WaitBeforeTakingPicture

### Example

- `PictureTriggerDelay = 0.5`
- `WaitBeforeTakingPicture = 2`

**Timeline:**

```
|<--- PictureTriggerDelay --->|<--- WaitBeforeTakingPicture --->| (Picture taken)
GPIO ON  ---------------------+-------------------------------+-------------------
                             |                               |
                             (Camera pre-capture starts)      (Picture)
```

### Recommendations

- **For most use cases:**
  - Set `PictureTriggerDelay` to a short value (e.g., 0.1–0.5s) and use `WaitBeforeTakingPicture` for the main pre-capture wait.
- **If you want the GPIO to be active for the entire pre-capture period:**
  - Set `PictureTriggerDelay` to 0, so the GPIO is activated immediately before the camera's own wait.
- **If you want the GPIO to activate well before the camera starts its own pre-capture wait:**
  - Set `PictureTriggerDelay` to a higher value.

### Customization

If you want a different behavior (for example, the GPIO should be active only during the last part of the pre-capture wait), the code can be adjusted. Contact the maintainers or open an issue for advanced timing requirements.

---

## Configuration

### TakeImage Section Parameters

Add these parameters to the `[TakeImage]` section in your `config.ini`:

```ini
[TakeImage]
; ... existing parameters ...
PictureTriggerGPIO = false          ; Enable/disable the picture trigger GPIO
PictureTriggerDelay = 0.5           ; Delay in seconds before picture taking (0.0 to 10.0)
PictureTriggerGPIONumber = 12       ; GPIO pin number to use (12 or 13 recommended)
PictureTriggerDuration = 0.1        ; How long to keep GPIO active (0.1 to 5.0 seconds)
```

### GPIO Configuration

You must also configure the selected GPIO pin in the GPIO section:

```ini
[GPIO]
IO12 = output disabled 10 false false PictureTrigger
```

or

```ini
[GPIO]
IO13 = output disabled 10 false false PictureTrigger
```

## Parameters

### PictureTriggerGPIO
- **Type**: Boolean
- **Default**: `false`
- **Description**: Enable or disable the GPIO picture trigger feature

### PictureTriggerDelay
- **Type**: Float (seconds)
- **Range**: 0.0 to 10.0
- **Default**: `0.5`
- **Description**: Delay in seconds before taking the picture after GPIO activation

### PictureTriggerGPIONumber
- **Type**: Integer
- **Options**: `12`, `13`
- **Default**: `12`
- **Description**: GPIO pin number to use for triggering

### PictureTriggerDuration
- **Type**: Float (seconds)
- **Range**: 0.1 to 5.0
- **Default**: `0.1`
- **Description**: How long to keep the GPIO active after taking the picture

### WaitBeforeTakingPicture
- **Type**: Integer (seconds)
- **Default**: `2`
- **Description**: How long the camera waits before actually taking the picture (camera pre-capture wait)

## Usage

1. **Enable the feature**: Set `PictureTriggerGPIO = true`
2. **Configure GPIO pin**: Set `PictureTriggerGPIONumber` to 12 or 13
3. **Configure GPIO section**: Add the corresponding GPIO configuration
4. **Set timing**: Adjust `PictureTriggerDelay` and `PictureTriggerDuration` as needed
5. **Save configuration**: The changes will take effect after the next reboot

## Hardware Considerations

- **GPIO 12 and 13** are the safest pins to use as they don't conflict with camera, SD card, or other system functions
- Ensure the GPIO pin is properly connected to your external device
- The GPIO will be set HIGH when triggered and LOW after the duration expires
- Maximum delay is 10 seconds to prevent excessive waiting times
- Maximum duration is 5 seconds to prevent the GPIO from staying active too long

## Example Use Cases

1. **Strobe Light Control**: Trigger a strobe light before taking a picture for better illumination
2. **Relay Control**: Activate a relay to control external equipment
3. **LED Strip Control**: Trigger additional lighting for better image quality
4. **External Sensor Trigger**: Activate external sensors or devices

## Safety Notes

- The feature is disabled by default to prevent accidental activation
- Only GPIO 12 and 13 are allowed to prevent conflicts with system functions
- Input validation ensures parameters stay within safe ranges
- Error handling prevents system crashes if GPIO operations fail

## Troubleshooting

1. **GPIO not activating**: Check that the GPIO pin is configured as output in the GPIO section
2. **Wrong GPIO pin**: Ensure you're using GPIO 12 or 13
3. **Timing issues**: Adjust the delay and duration parameters
4. **Configuration not saved**: Make sure to save the configuration and reboot the device

## Implementation Details

The feature integrates with the existing `ClassFlowTakeImage` class and uses the existing GPIO handler system. The GPIO is triggered before taking a picture and deactivated after the specified duration. 