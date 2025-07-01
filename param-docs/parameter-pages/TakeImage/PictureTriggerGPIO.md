# Parameter `PictureTriggerGPIO`

Enable GPIO output that activates before taking a picture.

Default Value: `false`

!!! Warning
    This is an **Expert Parameter**! Only change it if you understand what it does!

This feature activates a GPIO pin before taking a picture, which can be used to trigger external devices like strobe lights, relays, or other equipment.

**Requirements:**
- The selected GPIO pin must be configured as output in the GPIO section
- Only GPIO 12 and 13 are recommended for this feature
- See `PictureTriggerGPIONumber` to configure which GPIO pin to use
- See `PictureTriggerDelay` to configure the timing
- See `PictureTriggerDuration` to configure how long the GPIO stays active
- See `PictureTriggerMode` to configure if the GPIO is held for the whole process or just pulsed before the picture

## PictureTriggerMode

- `hold` (default): GPIO is activated before the picture-taking flow and stays on until the picture is taken (and optionally for PictureTriggerDuration after). Use this for lights or devices that need to be on during the picture.
- `pulse`: GPIO is activated for `PictureTriggerDuration` before the picture-taking flow, then deactivated before the rest of the flow continues. Use this for relays, buttons, or triggers that only need a momentary pulse. 