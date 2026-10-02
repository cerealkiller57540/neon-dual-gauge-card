<div align="center">

# ⚡ Neon Dual Gauge Card

**Two concentric LED gauges for Home Assistant, with a WebGL plasma core that reacts to the value.**

[![HACS Custom][hacs-badge]][hacs-url]
[![Release][release-badge]][release-url]
[![Validate][validate-badge]][validate-url]
[![License: MIT][license-badge]][license-url]

[![Open your Home Assistant instance and open this repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=cerealkiller57540&repository=neon-dual-gauge-card&category=plugin)

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-dual-gauge-card/main/images/main.gif" alt="Neon Dual Gauge Card: battery power on the inner ring, state of charge on the outer ring, plasma arcs in between" width="440">

</div>

Two related values in one round gauge: an inner LED ring and an outer one, each with its own range, colours, markers and zones. Built for a home battery (power in watts on the inside, charge level on the outside), it works just as well for temperature and humidity, solar production and consumption, or any pair of sensors.

The WebGL variant fills the space between the core and the inner ring with plasma arcs. Their number, speed and pulses follow the inner value, they flow towards the core or the ring depending on its sign, and a halo around the outer ring breathes with the second value.

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-dual-gauge-card/main/images/variants.png" alt="The CSS card (left) and the WebGL card (right) with the same data, without any theme" width="700">

*Left: `neon-dual-gauge-card` (CSS). Right: `neon-dual-gauge-card-webgl`. Same data, no theme.*

## ✨ Features

- **Two cards in one install**
  - `neon-dual-gauge-card-webgl`: plasma, LEDs, shadows and halos drawn in a single WebGL canvas (recommended).
  - `neon-dual-gauge-card`: the same gauges built from DOM elements and CSS.
- **Bidirectional mode** for values that go both ways (battery charging / discharging, grid import / export): the ring fills from zero in either direction.
- **Severity colours, markers and zones** per gauge.
- **Comet head, ignition sweep, glass centre, tick marks**, each one an on/off option.
- **Four halo styles** around the outer ring (WebGL): resonance, reservoir, nebula, or the plain glow of the CSS card.
- **Visual editor** with grouped sections; every plasma and halo setting is a slider.
- Inherits your theme colours by default. Pauses when off-screen, releases its WebGL context when removed (Android WebViews cap a page at 8 contexts), and falls back to the CSS LED rendering if WebGL is not available.

## 📦 Installation

### HACS (recommended)

1. Click the **Open in HACS** button above, or add this repository as a custom repository in HACS (category **Dashboard**): `https://github.com/cerealkiller57540/neon-dual-gauge-card`.
2. Download **Neon Dual Gauge Card**.
3. Reload your browser.

HACS registers one resource, `neon-dual-gauge-card.js`. It loads the WebGL variant on its own, so **do not** add `neon-dual-gauge-card-webgl.js` as a second resource.

### Manual

1. Copy both files from [`dist/`](dist) to `config/www/neon-dual-gauge-card/`.
2. Add a dashboard resource: URL `/local/neon-dual-gauge-card/neon-dual-gauge-card.js`, type **JavaScript module**.

## 🚀 Usage

The first gauge is the inner ring, the second the outer ring.

```yaml
type: custom:neon-dual-gauge-card-webgl
gauge_size: 240
inner_gauge_size: 150
inner_gauge_radius: 105
gauges:
  - entity: sensor.battery_power
    min: -2000
    max: 2000
    unit: W
    decimals: 0
    bidirectional: true
    leds_count: 200
    severity:
      - { value: -1000, color: "#7C3AED" }
      - { value: 0, color: "#A78BFA" }
      - { value: 1000, color: "#00F0FF" }
    markers:
      - { value: 0, label: 0W, color: "#A78BFA" }
  - entity: sensor.battery_level
    min: 0
    max: 100
    unit: "%"
    decimals: 0
    markers:
      - { value: 20, label: Low, color: "#7C3AED" }
      - { value: 80, label: Full, color: "#00F0FF" }
```

## ⚙️ Options

**Card**

| Option | Default | Description |
|---|---|---|
| `name` | — | Card title |
| `title_position` | `bottom` | `top`, `bottom`, `inside-top`, `inside-bottom`, `none` |
| `title_font_size` / `_family` / `_weight` / `_color` | theme | Title style |
| `gauge_size` | `200` | Outer diameter (px) |
| `inner_gauge_size` / `inner_gauge_radius` | 65 % of size / `65` | Inner disc diameter, radius of the inner LED ring |
| `primary_gauge` | `inner` | Which value is shown large in the centre |
| `hide_card` | `false` | Transparent card, no frame |
| `hide_shadows` | `false` | Turn every shadow off |
| `card_theme` | `default` | `default`, `light`, `dark`, `custom` |
| `custom_background`, `custom_gauge_background`, `custom_center_background`, `custom_text_color`, `custom_secondary_text_color` | theme | Colour overrides |
| `enable_comet_head` / `enable_ignition` | `true` | Bright head on the active LED / sweep on load |
| `enable_glass_center` / `enable_tick_marks` | `true` | Glass ring around the centre / engraved ticks |
| `neon_value_glow` / `value_glow_dynamic` | `true` | Glowing value text, coloured like the active LED |
| `enable_custom_effects`, `enable_top_glow`, `enable_pulse_animation`, `enable_glitch_hover` | `true` | Extra effects |
| `power_save_mode` / `debounce_updates` | auto | Pause when hidden / group fast updates. On by default on phones and tablets (debounce: iPad only) |

**Per gauge** (`gauges:`, exactly two)

| Option | Default | Description |
|---|---|---|
| `entity` | **required** | Sensor |
| `min` / `max` | `0` / `100` | Range |
| `unit` / `decimals` | entity unit / `1` | Display |
| `bidirectional` | `false` | Fill from zero (or the midpoint) in both directions |
| `leds_count` / `led_size` | `100` / `6` | Number and diameter (px) of LEDs |
| `hide_inactive_leds` | `false` | Show only the lit LEDs |
| `smooth_transitions` / `animation_duration` | `true` / `800` | Animate value changes (ms) |
| `severity` | — | List of `{value, color}`: colour by value |
| `markers` / `markers_radius` | — / LED radius | List of `{value, color, label}` around the ring |
| `zones` | — | List of `{from, to, color, opacity}` arcs |
| `value_font_size` / `_weight` / `_color` / `_family`, `unit_font_…` | theme | Value and unit style |
| `center_shadow`, `outer_shadow` (+ `_blur`, `_spread`) | `false` | Shadows |

**WebGL card only**

| Option | Default | Description |
|---|---|---|
| `halo_soc` | `resonance` | Halo around the outer ring: `resonance`, `reservoir`, `nebuleuse` (nebula), `v2` (plain CSS glow) |
| `halo_power` | `ondes` | Waves pushed out by the inner value, or `v2` |
| `halo_unavailable` | `freeze` | When a sensor is unavailable: `freeze` the plasma or `fade` it out |
| `plasma_flow_charge` | `core` | Where the arcs flow when the inner value is positive: `core` or `ring` |
| `plasma_fil_max` | `7` | Maximum number of arcs |
| `plasma_speed` / `plasma_pulse` | `1.00` / `0.70` | Arc speed and pulses |
| `plasma_glow` / `plasma_haze` | `1.25` / `1.00` | Coloured glow and background haze |

About thirty more `plasma_*` and `halo_*` settings (roughness, frequency, reach, turbulence…) are easiest to tune from the visual editor, where each is a slider with its range.

## ❓ FAQ

**Which value goes where?** The first entry of `gauges:` is the inner ring and drives the plasma; the second is the outer ring and drives the halo.

**Some cards go blank on my Android phone.** Android WebViews keep at most 8 WebGL contexts per page and drop the oldest one. This card uses one context and gives it back when it leaves the page. If you run many WebGL cards on one view, use `neon-dual-gauge-card` on some of them.

**The editor labels are in French.** Translation is on the way. Every option can also be set in YAML.

**Which theme is in the screenshots?** Neo Tokyo, the author's own dark theme (not published). The card works with any theme.

## 🙏 Credits

This card is a fork of [**dual_gauge**](https://github.com/guiohm79/dual_gauge) by [Guiohm79](https://github.com/guiohm79) (MIT). The two LED rings, the bidirectional mode, severity colours and markers come from there; the WebGL plasma, the halos and the neon styling were added on top. Go give the original a ⭐.

## 🌃 More neon cards

This card is part of a family. See the full collection at [**Home-Assistant-Neon-Cards**](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards).

---

## 🐾 Support this project

If you enjoy these cards, please consider donating to **Quatre Pattes**, an animal rescue organization.

[![Sauver des animaux](https://img.shields.io/badge/🐾%20Sauver%20des%20animaux-Faire%20un%20don-ff69b4?style=for-the-badge)](https://don.quatre-pattes.org/s/?_jtsuid=70083177244599792679303)

> 💛 No need to support me — just help the animals. Thank you!

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-card`
3. Commit and push
4. Open a Pull Request

---

## 📄 License

[MIT License][license-url]

[hacs-badge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[hacs-url]: https://hacs.xyz
[release-badge]: https://img.shields.io/github/v/release/cerealkiller57540/neon-dual-gauge-card?style=for-the-badge
[release-url]: https://github.com/cerealkiller57540/neon-dual-gauge-card/releases
[validate-badge]: https://img.shields.io/github/actions/workflow/status/cerealkiller57540/neon-dual-gauge-card/validate.yml?branch=main&label=HACS&style=for-the-badge
[validate-url]: https://github.com/cerealkiller57540/neon-dual-gauge-card/actions/workflows/validate.yml
[license-badge]: https://img.shields.io/github/license/cerealkiller57540/neon-dual-gauge-card?style=for-the-badge
[license-url]: LICENSE
