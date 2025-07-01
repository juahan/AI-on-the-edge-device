# Parameter `PictureTriggerMode`

Controls how the GPIO trigger is activated for picture taking.

- `hold` (default): GPIO is activated before the picture-taking flow and stays on until the picture is taken (and optionally for PictureTriggerDuration after). Use this for lights or devices that need to be on during the picture.
- `pulse`: GPIO is activated for `PictureTriggerDuration` before the picture-taking flow, then deactivated before the rest of the flow continues. Use this for relays, buttons, or triggers that only need a momentary pulse.

**Default:** `hold`

## Use Cases
- **hold**: For a light, keep GPIO high for the whole process (so the light is on during the picture).
- **pulse**: For a relay/button, just send a short pulse before the picture-taking process.

## Related Parameters
- `PictureTriggerGPIO`
- `PictureTriggerDuration`
- `PictureTriggerDelay`
- `PictureTriggerGPIONumber` 