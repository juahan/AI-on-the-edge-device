# Parameter `PictureTriggerDelay`

Delay in seconds before taking a picture when GPIO trigger is activated.

Range: `0.0` to `10.0`

Default Value: `0.5`

!!! Warning
    This is an **Expert Parameter**! Only change it if you understand what it does!

This parameter controls how long to wait after activating the GPIO trigger before taking the picture. This allows time for external devices (like strobe lights) to stabilize.

**Usage:**
- Set to `0.0` to trigger the GPIO and take the picture immediately
- Set to a higher value (e.g., `0.5`) to give external devices time to respond
- Maximum delay is 10 seconds to prevent excessive waiting times 