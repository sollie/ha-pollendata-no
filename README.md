# Pollen Data Integration for Home Assistant

A Home Assistant custom integration that fetches pollen data from a [pollendata](https://github.com/sollie/pollendata) service and displays it in a beautiful custom card.

## Features

- **Configurable hostname and region** - Connect to your pollendata service
- **Active pollen filtering** - Only shows pollen types with data (level > 0)
- **Optional pollen type filtering** - Monitor specific pollen types
- **Color-coded severity levels** - Visual indication of pollen levels (0-4)
- **Forecast support** - Display text forecasts if available
- **Custom Lovelace card** - Beautiful card with grid layout and color indicators
- **Automatic updates** - Polls data every 30 minutes
- **Proper error handling** - Graceful handling of connection issues

## Installation

### Method 1: Manual Installation

1. Copy the `custom_components/pollendata` folder to your Home Assistant's `custom_components` directory
2. Copy the `www/pollen-card.js` file to your Home Assistant's `www` directory
3. Restart Home Assistant
4. Go to Configuration > Integrations and add the "Pollen Data" integration

### Method 2: HACS (Home Assistant Community Store)

*Note: This integration is not yet available in HACS default repositories*

1. Add this repository as a custom repository in HACS
2. Install the integration through HACS
3. Restart Home Assistant
4. Go to Configuration > Integrations and add the "Pollen Data" integration

## Configuration

### Integration Setup

1. Go to **Configuration** > **Integrations**
2. Click the **+ Add Integration** button
3. Search for "Pollen Data"
4. Enter your pollendata service hostname (e.g., `localhost:8080`)
5. Select your region from the available options
6. Click **Submit**

### Options Configuration

After setting up the integration, you can configure additional options:

1. Go to **Configuration** > **Integrations**
2. Find your Pollen Data integration
3. Click **Configure**
4. Select specific pollen types to monitor (optional)

## Custom Card Setup

### Step 1: Add the Card Resource

1. Go to **Configuration** > **Lovelace Dashboards** > **Resources**
2. Add a new resource:
   - **URL**: `/local/pollen-card.js`
   - **Resource type**: JavaScript module

### Step 2: Add the Card to Your Dashboard

#### Basic Configuration

```yaml
type: custom:pollen-card
title: "Pollen Data"
```

#### Advanced Configuration

```yaml
type: custom:pollen-card
title: "Pollen Levels"
show_forecast: true
show_levels: true
show_thresholds: true
entities:
  - sensor.pollen_birch
  - sensor.pollen_grass
  - sensor.pollen_mugwort
forecast_entity: sensor.pollen_forecast
```

### Card Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `title` | string | "Pollen Data" | Card title |
| `show_forecast` | boolean | true | Show forecast section |
| `show_levels` | boolean | true | Show level names (Low, Moderate, etc.) |
| `show_thresholds` | boolean | true | Show pollen count thresholds |
| `entities` | list | auto-detect | Specific pollen sensor entities |
| `forecast_entity` | string | auto-detect | Forecast sensor entity |

## Sensors

The integration creates the following sensors:

### Pollen Sensors
- `sensor.pollen_birch` - Birch pollen level
- `sensor.pollen_grass` - Grass pollen level
- `sensor.pollen_mugwort` - Mugwort pollen level
- `sensor.pollen_alder` - Alder pollen level
- `sensor.pollen_hazel` - Hazel pollen level
- `sensor.pollen_oak` - Oak pollen level
- `sensor.pollen_pine` - Pine pollen level
- `sensor.pollen_poplar` - Poplar pollen level
- `sensor.pollen_willow` - Willow pollen level

### Forecast Sensor
- `sensor.pollen_forecast` - Text forecast (if available)

## Pollen Levels

The integration uses a 0-4 scale for pollen levels:

| Level | Name | Range (grains/m³) | Color |
|-------|------|-------------------|-------|
| 0 | None | 0 | Green |
| 1 | Low | 1-9 | Yellow |
| 2 | Moderate | 10-99 | Orange |
| 3 | Heavy | 100-999 | Red |
| 4 | Extreme | 1000+ | Purple |

## Sensor Attributes

Each pollen sensor provides these attributes:

- `level_name` - Human-readable level name
- `level_threshold` - Pollen count range
- `color` - Hex color code for the level
- `pollen_type` - Type of pollen
- `region` - Geographic region
- `last_updated` - Last update timestamp

## Pollendata Service

This integration requires a running [pollendata](https://github.com/sollie/pollendata) service that provides:

- `/regions` - Available regions
- `/pollen/{region}` - Pollen data for region
- `/forecast/{region}` - Forecast text for region
- `/combined/{region}` - Combined data and forecast

## Troubleshooting

### Common Issues

1. **Cannot connect to service**
   - Verify the hostname and port are correct
   - Ensure the pollendata service is running
   - Check firewall settings

2. **No sensors created**
   - Verify the region has active pollen data
   - Check Home Assistant logs for errors
   - Ensure the pollendata service returns valid data

3. **Card not displaying**
   - Verify the card resource is properly added
   - Check browser console for JavaScript errors
   - Ensure the card configuration is correct

### Debug Logging

Add to your `configuration.yaml`:

```yaml
logger:
  logs:
    custom_components.pollendata: debug
```

## Development

### Project Structure

```
custom_components/pollendata/
├── __init__.py          # Integration entry point
├── manifest.json        # Integration metadata
├── config_flow.py       # Configuration UI
├── coordinator.py       # Data update coordinator
├── sensor.py           # Sensor platform
├── api.py              # API client
├── const.py            # Constants
└── strings.json        # Translations

www/
└── pollen-card.js      # Custom Lovelace card
```

### Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

MIT License - see LICENSE file for details.

## Related Projects

- [pollendata](https://github.com/sollie/pollendata) - The backend service for fetching pollen data
- [Home Assistant](https://www.home-assistant.io/) - Open source home automation platform