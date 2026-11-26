<h1 align="center">
  <img src="assets/figma-icon.svg" alt="Figma" width="38" valign="middle" />
  Figma Desktop for Linux
</h1>

Runs the official Figma Desktop client on Linux. The build takes the Windows installer, patches it for Linux and packages it as an AppImage, `.deb` or `.rpm`.

This is a fork of [IliyaBrook/figma-linux](https://github.com/IliyaBrook/figma-linux), which is no longer maintained.

## Features

- The real Electron desktop client, not a browser wrapper
- System tray icon that follows light and dark mode
- `figma://` links and `.fig` file opening
- MCP server at `http://127.0.0.1:3845/mcp`
- Local fonts through a built-in font helper
- X11 and Wayland
- "Allow Duplicate Tabs" tray option to open the same file in several tabs

## Install

Download a package from the [Releases](https://github.com/AgenceStratis/figma-linux/releases) page.

```bash
# AppImage
chmod +x figma-desktop-*.AppImage
./figma-desktop-*.AppImage

# Debian / Ubuntu
sudo apt install ./figma-desktop_*.deb

# Fedora / RHEL
sudo dnf install ./figma-desktop-*.rpm
```

On first launch, the AppImage adds itself to the application menu and registers the `figma://` handler.

If FUSE is not available, run it with `APPIMAGE_EXTRACT_AND_RUN=1`.

## Build from source

The script installs missing tools (7z, ImageMagick, wget, Node.js 22.12+) with `apt` or `dnf`.

```bash
git clone https://github.com/AgenceStratis/figma-linux.git
cd figma-linux
./build.sh --build appimage   # or deb, rpm
```

| Option                        | Effect                                  |
| ----------------------------- | --------------------------------------- |
| `--build appimage\|deb\|rpm`  | Package format (default: matches distro) |
| `--exe /path/to/FigmaSetup.exe` | Use a local installer instead of downloading |
| `--clean no`                  | Keep the `build/` directory             |

`make build`, `make build-deb`, `make build-rpm`, `make run`, `make run-debug` and `make clean` wrap the same commands. `make url` prints the current Figma version.

## Configuration

Set these environment variables before launching Figma.

| Variable                             | Effect                                                  |
| ------------------------------------ | ------------------------------------------------------- |
| `FIGMA_USE_WAYLAND=1`                | Use native Wayland instead of XWayland                  |
| `FIGMA_USE_X11=1`                    | Prefer XWayland when both are available                 |
| `FIGMA_RENDERER=software`            | Software rendering, for broken GPU drivers              |
| `FIGMA_RENDERER=hardware`            | Electron graphics defaults                              |
| `FIGMA_DEBUG=1`                      | Verbose logs on stdout, DevTools for the tray window    |
| `FIGMA_FONT_AGENT_URL`               | Use another font helper endpoint                        |
| `FIGMA_FONT_AGENT_DISABLED=1`        | Skip external font helpers                              |
| `FIGMA_BUILTIN_FONT_AGENT_DISABLED=1` | Disable the built-in font helper                        |

By default, the launcher switches to software rendering when an X11 display has no GLX.

## Debugging

Logs go to `~/.cache/figma-desktop-linux/launcher.log`.

`Ctrl+Alt+I` opens DevTools for the current tab. `Shift+Ctrl+Alt+I` opens them for the window shell. Press `Alt` to show the menu bar.

## Known limitations

- No auto-update. Rebuild or download a new release to update.
- No eyedropper, since it needs native screen capture.
- x86_64 only.
- Font previews (`/figma/font-preview`) are not implemented.

## License

This project provides build tooling only. Figma Desktop is proprietary software owned by Figma, Inc. Using it means you accept [Figma's Terms of Service](https://www.figma.com/tos/).
