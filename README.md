# Weather (translated)

Omarchy's weather widget with its strings read from a translation
catalog instead of written into the QML. Drop-in replacement: install it and it
takes the built-in widget's place in the bar, keeping its position and settings.

```bash
omarchy plugin add https://github.com/sbelcl/omarchy-weather-l10n.git --enable
```

Removing it puts the built-in back. Translated: the city search, the FEELS / WIND / HUMID labels and the loading state.

It also fixes the forecast day names. Upstream formats them with `Qt.formatDate()`, which renders through the C locale whatever `LANG` says, so they read English on every system; this build takes them from the locale (`ČETRTEK`, not `THURSDAY`). The upstream fix is [omacom/omarchy#10955](https://github.com/omacom/omarchy/pull/10955).

## Where the words come from

```
~/.config/omarchy/locales/<language>.json    e.g. sl.json, ru.json
```

A plain map of English string to translation, watched, so editing it changes
the widget without a restart. No catalog at all reads as English, which is the
source of every key.
[omarchy-language](https://github.com/sbelcl/omarchy-language) ships and
installs the catalogs; this plugin does not require it.

**Stays English:** City names and weather conditions, which come from the weather service; temperatures and wind speeds, which are numbers.

## Why this is a fork

Every string is a QML literal, and there is no hook for one plugin to reach
another's. `manifest.json` declares `omarchy.clonedFrom: "omarchy.weather"`, which
the shell uses to route the built-in's IPC here, hand this its slot in the bar,
and restore the built-in when this is removed.

If [omacom/omarchy#7284](https://github.com/omacom/omarchy/issues/7284) lands a
translation layer upstream, delete this plugin rather than maintain it.

## Keeping it current

A copy of Omarchy **4.0.4**, so upstream fixes do not reach it on their own.
`upstream.diff` records every line this build changes. To re-sync after a
release:

```bash
cp /usr/share/omarchy/shell/plugins/panels/weather/{BarWidget.qml,Panel.qml,Model.js} .
patch -p0 < upstream.diff
omarchy plugin validate .
```

## License

MIT, as upstream. Derived from [Omarchy](https://github.com/omacom/omarchy).
