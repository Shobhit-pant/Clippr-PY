# Clippr PY

<img src="assets/clippr.png" width="96" alt="Clippr PY icon">

**Clippr PY** is the Python + PyQt6 edition of Clippr: a cross-platform
clipboard manager with text and image history, saved tabs, search, preview,
OCR, screen snips, strip-images, eight themes, and a system tray.

It runs on **Microsoft Windows** and **Apple macOS** from the same codebase,
and also works on Linux with a desktop environment.

- GitHub: <https://github.com/Shobhit-pant/Clippr-PY>
- Original Windows PowerShell edition: <https://github.com/Shobhit-pant/Clippr>

---

## What it does

- Captures copied **text** and **images** automatically.
- Remembers history in `history.json`; image files live beside it.
- **Strip images (text only)** removes the redundant bitmap from Excel/Office
  clipboard contents, so `Ctrl+V`/`Cmd+V` pastes text instead of a snapshot.
- **New Snip** captures a drag-selected area of the primary screen.
- **Preview** shows the full text or image.
- **OCR + COPY** recognizes text and copies it to the clipboard.
- **Search** across text, image captions, and saved OCR text.
- **Views and tabs**: All items, Text, Images, plus your own tabs.
- **Eight themes**: Swiss Paper, Midnight Brass, Neo-Brutalist, Calm Slate,
  Forest Ink, Rose Quartz, Sunset Ember, Mono Terminal.
- **Bring-your-own OCR**: Windows OCR, Apple Vision, Tesseract, OCR.space,
  Azure AI Vision, Google Cloud Vision, and PaddleOCR/self-hosted servers.
- System tray, pause/resume capture, start at login, single instance, and a
  themed "still running" toast.

---

## Install on Microsoft Windows

### 1. Install Python

Install **Python 3.10 or newer** (3.12/3.13 are good choices):

- Download from <https://www.python.org/downloads/windows/>
- During setup, enable **Add python.exe to PATH**.
- Or use winget:

```powershell
winget install Python.Python.3.12
```

Check it:

```powershell
py --version
```

If `py` is not available, use `python` instead.

### 2. Get the project

```powershell
cd "D:\path\to\Clippr-PY"
```

If you cloned from GitHub instead:

```powershell
git clone https://github.com/Shobhit-pant/Clippr-PY.git
cd Clippr-PY
```

### 3. Create a virtual environment and install dependencies

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
py -m pip install --upgrade pip
py -m pip install -r requirements.txt
```

If PowerShell blocks activation, allow it for this terminal only:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

### 4. Run Clippr PY

```powershell
py main.py
```

Or double-click `run_windows.bat`.

### 5. First use

1. Copy some text or an image.
2. Open Clippr PY and confirm it appears in history.
3. Select an entry to enable **Copy**, **Preview**, **OCR**, and **Delete**.
4. Try the red **NEW SNIP** button at the top, then drag a rectangle.
5. Open **SETTINGS** to choose a theme, adjust strip-images, or configure OCR.

### Windows OCR

Windows OCR uses the built-in `Windows.Media.Ocr` engine. No API key is needed.
If it reports that no OCR language is installed:

1. Open **Settings > Time & Language > Language & region**.
2. Select English (or your language) > **Language options**.
3. Install **Optical character recognition**.
4. Restart Clippr PY, or choose **Tesseract**/a cloud provider in
   **Settings > OCR**.

### Build a Windows .exe

```powershell
.\build_windows.ps1
```

The executable is written to `dist\ClipprPy\ClipprPy.exe`. Build on Windows;
PyInstaller cannot cross-compile a Windows executable from macOS.

Windows SmartScreen may warn about an unsigned executable. Use **More info >
Run anyway** for a build you made yourself, or sign the executable for
distribution.

---

## Install on Apple macOS

### 1. Install Python

Use **Python 3.10 or newer**. The old Python that comes with Xcode Command Line
Tools may be too old.

- Download the macOS installer from <https://www.python.org/downloads/macos/>.
- Or with Homebrew:

```bash
brew install python
```

Check it:

```bash
python3 --version
```

### 2. Get the project

```bash
cd ~/Clippr-PY
```

Or clone it:

```bash
git clone https://github.com/Shobhit-pant/Clippr-PY.git
cd Clippr-PY
```

### 3. Create a virtual environment and install dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements-macos.txt
```

`requirements-macos.txt` includes **PyObjC Vision** and **Quartz** for Apple's
built-in OCR.

### 4. Run Clippr PY

```bash
python main.py
```

Or make the launcher executable once and double-click it:

```bash
chmod +x run_macos.command
./run_macos.command
```

If Gatekeeper blocks a downloaded script, right-click it and choose **Open**.

### 5. Allow screen recording

**New Snip** captures the primary display. On macOS:

1. Open **System Settings > Privacy & Security > Screen Recording**.
2. Enable **Terminal**, **iTerm**, **Python**, or **Clippr** (whichever launches
   the app).
3. Quit and reopen Clippr PY if macOS does not apply the permission immediately.

Without this permission a snip may be empty or the system may silently block
the capture.

### macOS OCR

Apple Vision OCR is used through PyObjC. It is installed by
`requirements-macos.txt`; no API key is required.

If Vision is unavailable:

- Install Tesseract (`brew install tesseract`) and `pip install pytesseract`,
  then choose **Tesseract** in **Settings > OCR**, or
- configure **OCR.space**, **Azure AI Vision**, **Google Cloud Vision**, or a
  self-hosted **PaddleOCR** server.

Selecting **Windows OCR** on macOS intentionally fails with a clear message —
that engine exists only on Windows. **Auto** selects Apple Vision on macOS.

### Build a macOS .app

Install the Xcode Command Line Tools if `sips` or `iconutil` are missing:

```bash
xcode-select --install
```

Then:

```bash
chmod +x build_macos.sh
./build_macos.sh
open dist/Clippr.app
```

The script creates an `.icns` icon, bundles the assets and OCR helper, and
produces `dist/Clippr.app` using PyInstaller.

For distribution to other Macs you need an Apple Developer account, code
signing, and notarization (`codesign` and `notarytool`). An unsigned local build
can be opened with right-click **Open**, but it is not suitable for public
distribution.

---

## Linux (optional)

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python main.py
```

For OCR install Tesseract and `pytesseract`:

```bash
sudo apt install tesseract-ocr
python -m pip install pytesseract
```

A system tray implementation is required for hide-to-tray behavior.

---

## Using Clippr PY

### Capturing clips

- Copy text or an image in any application. It is added to history
  automatically unless capture is paused.
- The **CAPTURING** button at top-right pauses/resumes clipboard capture.
- Tick **Strip images (text only)** to keep only text from mixed
  text+image clipboard contents (the Excel duplicate-image fix).
- New clips go to the currently selected custom tab; if a built-in view is
  selected, they go to the last custom tab.

### New Snip

- Click the red-bordered **NEW SNIP** button next to the Clippr title.
- Drag a rectangle; release to capture.
- Press `Esc` to cancel.
- The snip is saved to history and copied to the clipboard.
- Shortcut while Clippr PY is focused: `Ctrl+Shift+S` on Windows or
  `Cmd+Shift+S` on macOS.
- The tray menu also has **New Snip…**.

Region capture currently covers the **primary display**.

### Preview and OCR

Double-click an entry or select it and click **PREVIEW**.

- **COPY** copies the text or image.
- **OCR + COPY** runs OCR on an image and copies the recognized text.
- If OCR uses a fallback provider, the status line tells you.
- Recognized text is saved on the image item, so search can find it.

### Tabs

- **All items** shows everything.
- **Text** and **Images** filter by type.
- Create your own tabs with **+ NEW TAB**.
- **RENAME TAB** and **DELETE TAB** apply to custom tabs only.
- Deleting a custom tab also deletes its items.

### Themes

Open **SETTINGS > GENERAL** and choose a theme from the dropdown. Themes apply
instantly and are remembered:

Swiss Paper, Midnight Brass, Neo-Brutalist, Calm Slate, Forest Ink, Rose
Quartz, Sunset Ember, and Mono Terminal.

### Tray behavior

Closing the main window hides Clippr PY to the tray and shows a small themed
toast. Double-click the tray icon (Windows) or use its menu (macOS/Windows) to
reopen. Use the tray menu to pause capture, create a snip, open settings, or
quit.

### Start at login

Enable **Start Clippr when I log in** in **Settings > General**:

- Windows: writes a current-user Run entry.
- macOS: writes a LaunchAgent in `~/Library/LaunchAgents/`.
- Linux: writes an autostart `.desktop` entry.

The app starts hidden in the tray.

---

## OCR configuration

Open **Settings > OCR**.

### Built-in engines

| Engine | Platform | Notes |
|---|---|---|
| **Auto** | All | Apple Vision on macOS; Windows OCR on Windows; Tesseract elsewhere. Native failure falls back to Tesseract. |
| **Windows OCR** | Windows only | Built into Windows; no key. |
| **macOS Vision** | macOS only | Apple Vision via PyObjC; no key. |
| **Tesseract** | All | Requires the Tesseract binary and `pytesseract`. |

If you do not have Windows/macOS native OCR, choose **Tesseract** or a cloud
provider. Explicit Windows/macOS choices fail clearly on the other platform.

### Bring your own providers

| Provider | What you need |
|---|---|
| **OCR.space** | API key; language (`eng`, `deu`, `hin`, etc.) |
| **Azure AI Vision** | Computer Vision endpoint + subscription key |
| **Google Cloud Vision** | API key with Cloud Vision enabled |
| **PaddleOCR / custom server** | Endpoint; optional API key/header; request mode |

Paddle supports:

- **PaddleHub JSON** (`images[]`)
- **Multipart form** (file upload)
- **Gravitee / image_url** (`input[]`)
- OAuth2 client credentials (token URL, client ID, secret, optional scope)

**Auto** uses a configured cloud provider first (OCR.space, Azure, Google,
Paddle), then built-in OCR. If a selected cloud provider fails, the app falls
back to built-in OCR and reports the reason. **AUTHENTICATE** runs the selected
provider on a small test image before you save.

### Auto-name images

Enable **Auto-name new images with OCR** if you want new images named from the
first OCR line. This may call the selected cloud provider for every new image,
so leave it off if you want to control API usage.

### Key storage

OCR keys are stored locally in `settings.json` in the app data folder. They are
plain text on disk, not uploaded anywhere except to the provider you select.
Never commit `settings.json` or share the app data folder.

---

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl/Cmd + F` | Focus search |
| `Ctrl/Cmd + Shift + S` | New Snip |
| `Delete` (history list focused) | Delete selected item |
| `Enter` / double-click | Preview selected item |

macOS uses `Cmd` where Windows uses `Ctrl`.

---

## Data and privacy

| Platform | Data folder |
|---|---|
| Windows | `%APPDATA%\ClipprPy` |
| macOS | `~/Library/Application Support/ClipprPy` |
| Linux | `~/.local/share/ClipprPy` |

Contents:

- `history.json` — clipboard entries, tabs, and OCR text.
- `settings.json` — theme, limits, OCR provider settings, and keys.
- `images/` — captured image files.

History is local. Clippr PY does not upload clipboard contents unless you
configure a cloud OCR provider and run OCR. The standalone app uses a separate
folder from the original PowerShell Clippr, so the two histories do not mix.

To reset the app, quit it, rename or remove that data folder, and restart. This
deletes local history and settings.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `python`/`py` is not recognized | Reinstall Python with PATH enabled, or use the full Python launcher path. |
| `ModuleNotFoundError: PyQt6` | Activate the virtual environment and run `pip install -r requirements.txt` (or `-r requirements-macos.txt`). |
| PowerShell will not activate `.venv` | Run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`, then activate again. |
| New Snip is empty on macOS | Enable Screen Recording permission for Terminal/Python/Clippr and restart the app. |
| OCR says no engine/key | Choose a configured provider in **Settings > OCR**, install Tesseract, or install the Windows OCR language pack. |
| `Authentication failed` | Check the key, endpoint, header, and network access; try **Built-in** first. |
| A second launch does nothing | Clippr PY is single-instance; the existing window is brought to the front. |
| Theme/keys do not persist | Check write access to the data folder. |
| App was built on the wrong OS | Build Windows `.exe` on Windows and macOS `.app` on macOS; PyInstaller cannot cross-compile these. |

---

## Tests

Run from the project root:

```bash
python tests/test_store.py
python tests/test_themes.py
python tests/test_ui_logic.py
python tests/test_ocr_config.py
python tests/test_settings_ocr.py
python tests/test_clipboard.py       # saves/restores the real clipboard
python tests/test_ocr_preview.py     # OCR + clipboard restore
python tests/smoke_gui.py            # renders all eight themes
python tests/smoke_dialogs.py        # preview + settings dialogs
python tests/smoke_app.py            # tray, toast, persistence
```

Pause or quit other clipboard managers before the clipboard tests. The tests
use temporary data folders and restore the clipboard in a `finally` block.

---

## Known limitations

- New Snip captures the primary display only; multi-monitor region selection is
  not implemented yet.
- There is no system-wide hotkey while another app is focused; the shortcut
  works when the Clippr PY window is focused.
- The original PowerShell edition's `clippr.json` history is not imported.
- macOS builds need a Mac for final testing, signing, and notarization.
- Auto-name with a cloud provider can consume API quota; it is optional.

---

## License

MIT. See [`LICENSE`](LICENSE).
