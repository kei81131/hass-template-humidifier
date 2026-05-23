# Humidifier Template

A Home Assistant custom integration that creates a template-based humidifier
entity. It is based on [jcwillox/hass-template-climate](https://github.com/jcwillox/hass-template-climate), but
for the `humidifier` domain.

## Installation With HACS

1. Open HACS in Home Assistant.
2. Open the top right three-dot menu and choose **Custom repositories**.
3. Add this repository URL.
4. Choose **Integration** as the category.
5. Install **Humidifier Template**.
6. Restart Home Assistant.

## Example Configuration

Add this to configuration.yaml

```yaml
humidifier:
  - platform: humidifier_template
    name: Bedroom dehumidifier
    unique_id: bedroom_dehumidifier
    min_humidity: 30
    max_humidity: 70
    target_humidity_step: 1
    modes:
      - "Smart"
      - "Sleep"
      - "Clothes Drying"
    state_template: "{{ states('humidifier.<your_humidifier>') }}"
    current_humidity_template: "{{ states('sensor.<your_humidity_sensor>') }}"
    target_humidity_template: >
      {{ state_attr('humidifier.<your_humidifier>', 'humidity') }}
    mode_template: >
      {{ state_attr('humidifier.<your_humidifier>', 'mode') }}
    action_template: >
      {{ state_attr('humidifier.<your_humidifier>', 'action') }}
    turn_on:
      - action: humidifier.turn_on
        target:
          entity_id: humidifier.<your_humidifier>
    turn_off:
      - action: humidifier.turn_off
        target:
          entity_id: humidifier.<your_humidifier>
    set_humidity:
      - action: humidifier.set_humidity
        target:
          entity_id: humidifier.<your_humidifier>
        data:
          humidity: "{{ humidity }}"
    set_mode:
      - action: humidifier.set_mode
        target:
          entity_id: humidifier.<your_humidifier>
        data:
          mode: "{{ mode }}"
```

## Options

| Option | Description |
| --- | --- |
| `state_template` | Template returning `on` or `off`. |
| `current_humidity_template` | Template for current humidity. |
| `target_humidity_template` | Template for target humidity. |
| `min_humidity` / `max_humidity` | Static humidity range. |
| `min_humidity_template` / `max_humidity_template` | Dynamic humidity range templates. |
| `target_humidity_step` | Humidity step size. |
| `modes` | Available modes shown by Home Assistant. |
| `mode_template` | Template for current mode. |
| `action_template` | Template for `humidifying`, `drying`, `idle`, or `off`. |
| `turn_on` / `turn_off` | Scripts to call when the entity is toggled. |
| `set_humidity` | Script to call when target humidity changes. |
| `set_mode` | Script to call when mode changes. |

## Notes

