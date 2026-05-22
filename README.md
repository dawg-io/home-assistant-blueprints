# Home Assistant Blueprints by dawg-io

A small collection of beginner-friendly Home Assistant HVAC blueprints focused on temperature scheduling and door/window protection.

## Blueprints

### 1) HVAC - Seasonal Door/Window Temperature Protection
- **What it does:** Watches a door/window group and applies temporary HVAC protection settings if something is left open too long.
- **Best use case:** Preventing wasteful heating/cooling when doors or windows stay open.
- **Detailed docs:** [docs/hvac-seasonal-door-window-temperature-protection.md](docs/hvac-seasonal-door-window-temperature-protection.md)
- **Example YAML:** [examples/hvac-seasonal-door-window-temperature-protection-example.yaml](examples/hvac-seasonal-door-window-temperature-protection-example.yaml)

### 2) HVAC - Daily Seasonal Thermostat Schedule
- **What it does:** Applies season-aware morning/afternoon/evening thermostat targets throughout the day.
- **Best use case:** Keeping consistent comfort ranges by season while allowing door/window protection to temporarily override.
- **Detailed docs:** [docs/hvac-daily-thermostat-schedule.md](docs/hvac-daily-thermostat-schedule.md)
- **Example YAML:** [examples/hvac-daily-thermostat-schedule-example.yaml](examples/hvac-daily-thermostat-schedule-example.yaml)

## How to import

1. In Home Assistant, go to **Settings → Automations & Scenes → Blueprints**.
2. Select **Import Blueprint**.
3. Paste a raw GitHub URL to the blueprint YAML:
   - `https://raw.githubusercontent.com/dawg-io/home-assistant-blueprints/main/blueprints/automation/dawg_io/hvac-seasonal-door-window-temperature-protection.yaml`
   - `https://raw.githubusercontent.com/dawg-io/home-assistant-blueprints/main/blueprints/automation/dawg_io/hvac_daily_thermostat_schedule.yaml`
4. Create automations from the imported blueprints.

> Replace all example entity IDs with your own Home Assistant entities.
