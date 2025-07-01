# Parameter `PictureTriggerDuration`

How long to keep the GPIO trigger active.

- In `hold` mode: This is how long the GPIO stays active **after** the picture is taken (the GPIO is held high for the whole process, then for this duration after).
- In `pulse` mode: This is how long the GPIO is pulsed **before** the picture-taking flow starts (the GPIO is activated for this duration, then deactivated before the picture flow continues).

Range: `0.1` to `5.0`

Default Value: `0.1`

!!! Warning
    This is an **Expert Parameter**! Only change it if you understand what it does!

**Usage:**
- Set to `0.1` for a quick pulse (default)
- Set to a higher value if your external device needs more time to complete its operation
- Maximum duration is 5 seconds to prevent the GPIO from staying active too long
- The GPIO will be automatically deactivated after this duration (in pulse mode, before the picture; in hold mode, after the picture) 