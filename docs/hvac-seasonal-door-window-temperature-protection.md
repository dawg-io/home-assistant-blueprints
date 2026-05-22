# HVAC - Seasonal Door/Window Temperature Protection

This blueprint protects HVAC behavior when doors/windows are left open.
It monitors a grouped door/window binary sensor, applies temporary seasonal protection settings after an open delay, and restores your normal daily schedule after everything stays closed for a restore delay.

## How seasonal mode works

The blueprint checks your `season_entity` (usually `sensor.season`) and picks the matching protection HVAC mode:

- **winter** → `winter_hvac_mode`
- **summer** → `summer_hvac_mode`
- **spring** → `spring_hvac_mode`
- **autumn** (Home Assistant uses `autumn`, not `fall`) → `autumn_hvac_mode`

## Protection behavior

When the door/window group is **on** for `open_duration`:
1. Turns on `protection_active_helper`.
2. Sets HVAC mode based on season.
3. Applies protection temperature setpoints:
   - **heat mode:** `winter_open_heat`
   - **cool mode:** `summer_open_cool`
   - **heat_cool mode:** `spring_fall_open_low` and `spring_fall_open_high`
4. Optionally sends a notification and runs optional open actions.

When the door/window group is **off** for `closed_duration` and protection is active:
1. Triggers the selected daily schedule automation (`skip_condition: true`) to restore seasonal schedule targets.
2. Turns off `protection_active_helper`.
3. Optionally sends a notification and runs optional closed actions.

On Home Assistant restart, if protection was active and openings are closed, it waits `closed_duration` and then restores automatically.

## Heat/Cool open-door low/high (plain English)

- **Spring / Fall Heat-Cool Open-Door Low** = lower bound where heating can run.
- **Spring / Fall Heat-Cool Open-Door High** = upper bound where cooling can run.
- The blueprint enforces a safety gap: high must be at least **7°F** above low when `heat_cool` is used.

## Inputs

### Required settings

- **`climate_entities`** (`climate`, multiple): Thermostats to control.
- **`openings_group`** (`binary_sensor`): Group helper that is ON when any tracked opening is open.
- **`protection_active_helper`** (`input_boolean`): Tracks whether protection is currently active.
- **`daily_schedule_automation`** (`automation`): Automation created from the daily schedule blueprint; triggered for restore.
- **`season_entity`** (`sensor`, default `sensor.season`): Season source.

### Timing

- **`open_duration`** (`duration`, default 5 minutes): Open time before protection starts.
- **`closed_duration`** (`duration`, default 5 minutes): Closed time before restore runs.

### Seasonal HVAC modes

- **`winter_hvac_mode`** (`select`, default `heat`)
- **`summer_hvac_mode`** (`select`, default `cool`)
- **`spring_hvac_mode`** (`select`, default `heat_cool`)
- **`autumn_hvac_mode`** (`select`, default `heat_cool`)

### Protection temperatures

- **`winter_open_heat`** (`number`, °F, default 62): Winter heat setpoint during protection.
- **`summer_open_cool`** (`number`, °F, default 80): Summer cool setpoint during protection.
- **`spring_fall_open_low`** (`number`, °F, default 62): Mild-weather lower target in `heat_cool` mode.
- **`spring_fall_open_high`** (`number`, °F, default 80): Mild-weather upper target in `heat_cool` mode.

### Notifications

- **`notify_open`** (`boolean`, default `false`): Send alert when protection starts.
- **`notify_closed`** (`boolean`, default `false`): Send alert when restore completes.
- **`notify_service`** (`text`, default `notify.mobile_app_your_phone`): Notification service.
- **`open_notification_title`** (`text`, default `HVAC Protection Enabled`)
- **`open_notification_message`** (`text`, default provided in blueprint)
- **`closed_notification_title`** (`text`, default `HVAC Restored`)
- **`closed_notification_message`** (`text`, default provided in blueprint)

### Optional actions

- **`open_actions`** (`action`, default `[]`): Extra actions when protection starts.
- **`closed_actions`** (`action`, default `[]`): Extra actions when HVAC is restored.

## Example automation

```yaml
alias: HVAC Seasonal Door/Window Protection (Example)
description: Example only — replace entities with your own.
use_blueprint:
  path: dawg_io/hvac-seasonal-door-window-temperature-protection.yaml
  input:
    climate_entities:
      - climate.downstairs
    openings_group: binary_sensor.door_window_group
    protection_active_helper: input_boolean.hvac_protection_active
    daily_schedule_automation: automation.hvac_daily_thermostat_schedule_example
    season_entity: sensor.season
    open_duration:
      minutes: 5
    closed_duration:
      minutes: 5
    winter_hvac_mode: heat
    summer_hvac_mode: cool
    spring_hvac_mode: heat_cool
    autumn_hvac_mode: heat_cool
    winter_open_heat: 62
    summer_open_cool: 80
    spring_fall_open_low: 62
    spring_fall_open_high: 80
    notify_open: true
    notify_closed: true
    notify_service: notify.mobile_app_phone
    open_notification_title: HVAC Protection Enabled
    open_notification_message: A door/window has been open too long. Protection mode is active.
    closed_notification_title: HVAC Restored
    closed_notification_message: All doors/windows are closed. Daily schedule has been restored.
```
