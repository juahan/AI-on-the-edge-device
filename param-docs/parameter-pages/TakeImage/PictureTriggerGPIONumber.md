# Parameter `PictureTriggerGPIONumber`

GPIO pin number to use for picture trigger.

Options: `12`, `13`

Default Value: `12`

!!! Warning
    This is an **Expert Parameter**! Only change it if you understand what it does!

**Important:** Only GPIO 12 and 13 are recommended for this feature as they are not used by other system functions.

**GPIO Configuration:**
You must also configure the selected GPIO pin in the GPIO section of the configuration file:

```ini
[GPIO]
IO12 = output disabled 10 false false PictureTrigger
```

or

```ini
[GPIO]
IO13 = output disabled 10 false false PictureTrigger
```

**Hardware Considerations:**
- GPIO 12 and 13 are the safest pins to use
- Other GPIO pins may conflict with camera, SD card, or other system functions
- Ensure the GPIO pin is properly connected to your external device 