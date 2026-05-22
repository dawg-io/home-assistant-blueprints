# HVAC - Daily Seasonal Thermostat Schedule

This blueprint sets thermostat temperature targets at scheduled times during the day.
It applies different values for morning, afternoon, and evening, based on the current season.
<img width="1474" height="970" alt="Screenshot of the HVAC daily seasonal thermostat schedule blueprint" src="https://github.com/user-attachments/assets/f8c94cb9-3500-4c21-98fe-5b4cab0dcd5d" />

## Schedule overview

- **Morning** runs at `morning_time`
- **Afternoon** runs at `afternoon_time`
- **Evening** runs at `evening_time`

It also runs when:
- `season_entity` changes
- Home Assistant starts

The blueprint only runs when `protection_active_helper` is `off`, so seasonal door/window protection can temporarily hold settings.

## Temperature behavior by season

- **Summer:** sets `hvac_mode: cool` with a single cooling target (`temperature`).
- **Winter:** sets `hvac_mode: heat` with a single heating target (`temperature`).
- **Spring/Autumn:** sets `hvac_mode: heat_cool` with low/high range (`target_temp_low`, `target_temp_high`).

Users can customize all schedule times and all seasonal temperature values.

## Inputs

- **`climate_entities`** (`climate`, multiple): Thermostats to control.
- **`protection_active_helper`** (`input_boolean`): If ON, this schedule automation is skipped.
- **`season_entity`** (`sensor`, default `sensor.season`): Season source (`autumn` is used instead of `fall`).

### Schedule times
- **`morning_time`** (`time`, default `06:00:00`)
- **`afternoon_time`** (`time`, default `12:00:00`)
- **`evening_time`** (`time`, default `20:00:00`)

### Summer cooling targets
- **`summer_morning_cool`** (`number`, °F, default 75)
- **`summer_afternoon_cool`** (`number`, °F, default 75)
- **`summer_evening_cool`** (`number`, °F, default 75)

### Winter heating targets
- **`winter_morning_heat`** (`number`, °F, default 65)
- **`winter_afternoon_heat`** (`number`, °F, default 65)
- **`winter_evening_heat`** (`number`, °F, default 65)

### Spring mild-weather heat/cool ranges
- **`spring_morning_low`** (`number`, °F, default 64)
- **`spring_morning_high`** (`number`, °F, default 75)
- **`spring_afternoon_low`** (`number`, °F, default 64)
- **`spring_afternoon_high`** (`number`, °F, default 75)
- **`spring_evening_low`** (`number`, °F, default 64)
- **`spring_evening_high`** (`number`, °F, default 75)

### Autumn/Fall mild-weather heat/cool ranges
- **`autumn_morning_low`** (`number`, °F, default 64)
- **`autumn_morning_high`** (`number`, °F, default 75)
- **`autumn_afternoon_low`** (`number`, °F, default 64)
- **`autumn_afternoon_high`** (`number`, °F, default 75)
- **`autumn_evening_low`** (`number`, °F, default 64)
- **`autumn_evening_high`** (`number`, °F, default 75)

## Example automation

```yaml
alias: HVAC Daily Thermostat Schedule (Example)
description: Example only — replace entities with your own.
use_blueprint:
  path: dawg_io/hvac_daily_thermostat_schedule.yaml
  input:
    climate_entities:
      - climate.downstairs
    protection_active_helper: input_boolean.hvac_protection_active
    season_entity: sensor.season
    morning_time: "06:00:00"
    afternoon_time: "12:00:00"
    evening_time: "20:00:00"
    summer_morning_cool: 74
    summer_afternoon_cool: 76
    summer_evening_cool: 73
    winter_morning_heat: 67
    winter_afternoon_heat: 65
    winter_evening_heat: 68
    spring_morning_low: 63
    spring_morning_high: 75
    spring_afternoon_low: 64
    spring_afternoon_high: 76
    spring_evening_low: 63
    spring_evening_high: 74
    autumn_morning_low: 63
    autumn_morning_high: 75
    autumn_afternoon_low: 64
    autumn_afternoon_high: 76
    autumn_evening_low: 63
    autumn_evening_high: 74
```
