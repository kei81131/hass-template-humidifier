# Humidifier Template

A Home Assistant custom integration that creates a template-based humidifier
entity. It is based on the same pattern as template climate integrations, but
for the `humidifier` domain.

## Installation With HACS

1. Open HACS in Home Assistant.
2. Go to **Integrations**.
3. Open the three-dot menu and choose **Custom repositories**.
4. Add this repository URL.
5. Choose **Integration** as the category.
6. Install **Humidifier Template**.
7. Restart Home Assistant.

## Manual Installation

Copy this folder:

```text
custom_components/humidifier_template
```

to:

```text
<config>/custom_components/humidifier_template
```

Then restart Home Assistant.

## Example Configuration

Add this under the standard Home Assistant `humidifier:` key:

```yaml
humidifier:
  - platform: humidifier_template
    name: bedroom_humidifier
    unique_id: bedroom_humidifier
    min_humidity: 30
    max_humidity: 80
    target_humidity_step: 1
    modes:
      - "Auto"
      - "Silent"
      - "Medium"
      - "High"
    state_template: "{{ states('humidifier.your_humidifier') }}"
    current_humidity_template: "{{ states('sensor.your_humidity_sensor') }}"
    target_humidity_template: >
      {{ state_attr('humidifier.your_humidifier', 'humidity') }}
    mode_template: >
      {% set mode = state_attr('humidifier.your_humidifier', 'mode') %}
      {{ mode if mode is not none else 'Auto' }}
    action_template: >
      {% set action = state_attr('humidifier.your_humidifier', 'action') %}
      {% if action in ['humidifying', 'drying', 'idle', 'off'] %}
        {{ action }}
      {% elif is_state('humidifier.your_humidifier', 'on') %}
        humidifying
      {% else %}
        off
      {% endif %}
    turn_on:
      - action: humidifier.turn_on
        target:
          entity_id: humidifier.your_humidifier
    turn_off:
      - action: humidifier.turn_off
        target:
          entity_id: humidifier.your_humidifier
    set_humidity:
      - action: humidifier.set_humidity
        target:
          entity_id: humidifier.your_humidifier
        data:
          humidity: "{{ humidity }}"
    set_mode:
      - action: humidifier.set_mode
        target:
          entity_id: humidifier.your_humidifier
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

Before publishing, replace `YOUR_GITHUB_USERNAME` in
`custom_components/humidifier_template/manifest.json` with your real GitHub
username, and update the repository name in the `documentation` and
`issue_tracker` URLs if you choose a different repo name.
