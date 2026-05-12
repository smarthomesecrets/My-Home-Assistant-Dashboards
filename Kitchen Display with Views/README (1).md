# Tab Kitchen Dashboard

Kitchen tablet dashboard with 7 tab views covering the home at a glance, kids' chores, media control, sports scores, HVAC, and live camera feeds.

## Views

| File | View | Description |
|---|---|---|
| `01-view-photos.yaml` | Photos | Full-screen Immich Frame iframe |
| `02-view-home.yaml` | Home | Weather, lights, locks, calendar, front door camera |
| `03-view-kids-chores.yaml` | Kids Chores | Time-of-day task cards via Taskmate |
| `04-view-media.yaml` | Media | Sonos, Apple TV, outdoor speaker controls |
| `05-view-sports.yaml` | Sports | Live game scores via TeamTracker |
| `06-view-hvac.yaml` | HVAC | Thermostat, fireplace, and fan controls |
| `07-view-cameras.yaml` | Cameras | Live UniFi camera feeds with detection-aware borders |

## Required HACS

| Plugin | Used in |
|---|---|
| [calendar-card-pro](https://github.com/FamousWolf/calendar-card-pro) | Home view |
| [taskmate](https://github.com/BlueVerde/taskmate) | Kids Chores view — integration + cards |
| [teamtracker-card](https://github.com/vasqued2/ha-teamtracker) | Sports view — integration + card |
| [card-mod](https://github.com/thomasloven/lovelace-card-mod) | Cameras view |

### Theme

Views use the **Frosted Glass Dark** and **Frosted Glass Dark Lite** themes. Install via HACS ([lovelace-themes](https://github.com/basnijholt/lovelace-themes)) or substitute your own theme name.

---

## Placeholders

All placeholders are in `ALL_CAPS`. Use **Find & Replace** across all files to substitute your values before importing.

| Placeholder | What to replace it with | Where to find it |
|---|---|---|
| `YOUR_IMMICH_FRAME_URL` | Your Immich Frame URL | e.g. `https://your-nas:8083` |
| `person.YOUR_NAME` | Your person entity | Settings → People |
| `person.SPOUSE_NAME` | Your spouse's person entity | Settings → People |
| `calendar.YOUR_SPORTS_CALENDAR` | Your sports calendar entity | Settings → Devices & Services → Calendars |
| `calendar.YOUR_PERSONAL_CALENDAR` | Your personal calendar entity | Settings → Devices & Services → Calendars |
| `calendar.SPOUSE_PERSONAL_CALENDAR` | Your spouse's calendar entity | Settings → Devices & Services → Calendars |
| `sensor.YOUR_POOL_WATER_TEMPERATURE` | Pool water temp sensor | Settings → Entities |
| `sensor.the_tub_current_temperature` | Hot tub temp sensor | Settings → Entities |
| `climate.YOUR_DYSON_CLIMATE` | Dyson climate entity | Settings → Devices → find your Dyson |
| `sensor.YOUR_DYSON_TEMPERATURE` | Dyson temperature sensor | Settings → Devices → find your Dyson |
| `YOUR_CHILD_1_ID` | Child 1 ID from Taskmate | Settings → Taskmate |
| `YOUR_CHILD_2_ID` | Child 2 ID from Taskmate | Settings → Taskmate |
| `YOUR_CHILD_1_REWARD_ID` | Reward ID for child 1 | Settings → Taskmate |
| `YOUR_CHILD_2_REWARD_ID` | Reward ID for child 2 | Settings → Taskmate |
| `media-source://YOUR_BACKGROUND_IMAGE` | Sports tab background image | Settings → Media → Images |

> **Note on pool sensor:** If your pool temperature sensor entity contains your address in its name, rename it in Settings → Entities before use.

---

## Adding or Removing an Entity

### To add an entity to an existing card

Find the `entities:` list in the relevant view file and add a new entry:

```yaml
entities:
  - entity: light.your_existing_light
    tap_action:
      action: toggle
  - entity: light.your_new_light      # add this
    tap_action:
      action: toggle
```

### To remove an entity from an existing card

Delete the corresponding entry from the `entities:` list.

### To add a new card to a view

Append a new card block under the `cards:` list of the relevant `sections:` grid:

```yaml
cards:
  - type: tile
    entity: switch.your_new_switch
    tap_action:
      action: toggle
```

### To remove a card entirely

Delete the full card block — from its `type:` line through to the end of its indented properties — including any trailing `---` separator if present.

---

## Importing into Home Assistant

1. Go to **Settings → Dashboards** and create a new dashboard (or open an existing one in edit mode).
2. Open the three-dot menu → **Edit Dashboard** → **Raw configuration editor**.
3. Paste the contents of each view file under the `views:` key in the correct order.
4. Save and reload.
