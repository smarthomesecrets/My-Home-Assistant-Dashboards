# Daily Messages Dashboard

AI-generated morning briefing dashboard. Each household member sees their own personalised view via conditional cards tied to HA user IDs. Shared cards (weather forecast, energy summary) are visible to everyone.

## Views

| File | View | Description |
|---|---|---|
| `01-view-messages.yaml` | Messages | Full briefing: weather, energy, per-user calendar and AI digest |

## Required HACS

| Plugin | Used for |
|---|---|
| [bubble-card](https://github.com/Clooos/Bubble-Card) | Energy and weather pop-up panels |
| [html-template-card](https://github.com/PiotrMachowski/lovelace-html-template-card) | Hourly and 8-day forecast rendering |
| [nimbus-weather-card](https://github.com/NimbusSolutions/nimbus-weather-card) | Weather overview tile |
| [button-card](https://github.com/custom-cards/button-card) | Energy details navigation button |

## Custom Sensor Requirement

This dashboard depends on two **custom template sensors** that you must build and populate:

- `sensor.YOUR_DAILY_DIGEST` — your personal digest
- `sensor.SPOUSE_DAILY_DIGEST` — your spouse's digest

These sensors are populated by an automation using an AI conversation agent (e.g. Claude). Each sensor exposes named attributes that the dashboard Jinja2 templates read from. The attribute names in the YAML must match your sensor's attribute names exactly.

**Expected attributes on each digest sensor:**

| Attribute | Contents |
|---|---|
| `generated_at` | Timestamp string shown in the briefing header |
| `YOUR_WORK_EVENTS` | Pipe-delimited work calendar events |
| `YOUR_PERSONAL_EVENTS` | Pipe-delimited personal calendar events |
| `YOUR_SPORTS_EVENTS` | Pipe-delimited sports calendar events |
| `SPOUSE_EVENTS` | Pipe-delimited spouse calendar events |
| `family_events` | Pipe-delimited shared family calendar events |
| `YOUR_SPARK_ATTR` | Daily quote, joke, or fact for your briefing |
| `YOUR_SPOUSE_SPARK_ATTR` | Daily spark + optional extra content for spouse briefing |

## Built-in Helpers Required

Create these under **Settings → Helpers → Add Helper → Number**:

| Helper | Purpose |
|---|---|
| `input_number.energy_meter_midnight_baseline` | kWh reading at midnight (reset daily by automation) |
| `input_number.tou_rate_off_peak` | Off-peak TOU rate ($/kWh) |
| `input_number.tou_rate_mid_peak` | Mid-peak TOU rate ($/kWh) |
| `input_number.tou_rate_on_peak` | On-peak TOU rate ($/kWh) |
| `input_number.yesterday_home_energy_kwh` | Yesterday's total kWh (set by automation at midnight) |
| `input_number.yesterday_home_energy_cost` | Yesterday's estimated cost (set by automation at midnight) |
| `input_number.last_tesla_charge_kwh` | kWh added in last charge session |
| `input_number.last_tesla_charge_cost` | Estimated cost of last charge session |

---

## Placeholders

All placeholders are in `ALL_CAPS`. Use **Find & Replace** across the file to substitute your values.

| Placeholder | What to replace it with | Where to find it |
|---|---|---|
| `YOUR_USER_ID` | Your HA user ID | Settings → People → [your user] → User ID (shown in URL or via developer tools) |
| `YOUR_SPOUSE_USER_ID` | Your spouse's HA user ID | Settings → People → [spouse] → User ID |
| `sensor.YOUR_DAILY_DIGEST` | Your digest sensor entity | Settings → Entities (after building the sensor) |
| `sensor.SPOUSE_DAILY_DIGEST` | Spouse digest sensor entity | Settings → Entities |
| `sensor.YOUR_HOME_ENERGY_KWH` | Cumulative kWh sensor from your energy meter | Settings → Entities |
| `sensor.YOUR_TESLA_BATTERY_LEVEL` | Tesla battery level sensor | Settings → Devices → Tesla |
| `sensor.YOUR_TESLA_WALL_CONNECTOR_STATUS` | Tesla wall connector status sensor | Settings → Devices → Tesla |
| `YOUR_WORK_EVENTS` | Attribute name on your digest sensor | Must match your sensor implementation |
| `YOUR_PERSONAL_EVENTS` | Attribute name on your digest sensor | Must match your sensor implementation |
| `YOUR_SPORTS_EVENTS` | Attribute name on your digest sensor | Must match your sensor implementation |
| `SPOUSE_EVENTS` | Attribute name on your spouse's digest sensor | Must match your sensor implementation |
| `YOUR_SPARK_ATTR` | Attribute name for your daily spark | Must match your sensor implementation |
| `YOUR_SPOUSE_SPARK_ATTR` | Attribute name for spouse's spark/extra content | Must match your sensor implementation |
| `YOUR_NAME` / `SPOUSE_NAME` | Display names in markdown headings | Replace with preferred display names |

> **Finding your user ID:** Go to Settings → People → click your user → the UUID appears in the browser URL bar, e.g. `/config/person/edit/XXXXXXXX`.

---

## Adding or Removing a User

### To add a third user

Duplicate an existing `conditional` card block and update the `users:` list with the new user's ID:

```yaml
type: conditional
conditions:
  - condition: user
    users:
      - YOUR_NEW_USER_ID
card:
  type: markdown
  content: "## 📬 Third Person's Briefing..."
```

### To remove a user's personalised section

Delete the entire `conditional` card block for that user — from `type: conditional` through to the end of its nested `card:` block.

### To remove the Tesla section

Find the energy markdown card and delete the lines from `---` through to the end of the Tesla block (`*Charged at off-peak rate...*`). Remove the corresponding helpers if no longer needed.

---

## Importing into Home Assistant

1. Go to **Settings → Dashboards** and open your Messages dashboard in edit mode.
2. Open the three-dot menu → **Raw configuration editor**.
3. Paste the contents of `01-view-messages.yaml` under the `views:` key.
4. Save and reload.
