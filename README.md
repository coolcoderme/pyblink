# pyblink

Python Blink (Chromium) host library. It launches a local Chrome/Chromium process and talks to it over the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/), so a PyQt6 app can render pages without QtWebEngine.

Public types (`BlinkHost`, `BlinkView`, `BlinkDownload`, …) match the former in-tree `blinkengine` package from [pyqt6-aibrowser](https://github.com/coolcoderme/pyqt6-aibrowser).

## Install

```bash
pip install .
```

For development:

```bash
pip install -e .
```

Requires **Python 3.9+**. PyQt6 is a runtime dependency (`CdpClient`, `BlinkHost`, and `BlinkView` are Qt types).

## Chrome / Chromium

A Chrome or Chromium binary must be available:

- on `PATH` (`google-chrome`, `google-chrome-stable`, `chromium`, or `chromium-browser`), or
- via the `CHROME_PATH` environment variable pointing at the executable.

The library starts Chromium headless with remote debugging and paints frames through CDP screencast. GPU warnings on startup are expected (`--disable-gpu`); they are not failures.

## Usage

```python
from pyblink import BlinkHost, BlinkView, find_chromium

print(find_chromium())  # path to the Chrome/Chromium binary

host = BlinkHost(user_data_dir="./browser_data/blink")
host.start()
view = BlinkView(host)  # QWidget; attach to your Qt layout
view.setUrl("https://example.com")
```

Call `host.stop()` when you are done (and `view.shutdown()` for each view).
