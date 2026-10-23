# heliograph lite

One file, `heliograph.pyz`, that turns a markdown scene file into a 1080p MP4: the checker, the renderer,
158 icons, 21 templates, 5 themes and the guides. No web app, server or model tooling.

## Set up once

You need Python 3.11 to 3.14; ffmpeg comes with the packages. In a virtual environment:

```
python -m venv .venv                         (python3 on Linux and macOS)
.venv\Scripts\activate                       Windows
. .venv/bin/activate                         Linux and macOS
python -m pip install -r requirements.txt    the packages it needs, ffmpeg included, each checked by its hash
python heliograph.pyz doctor                 ends with RESULT: OK
```

Activate the environment again in each new terminal. PowerShell runs `activate` only if scripts are allowed
(`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, once); without that, type `.venv\Scripts\python`
wherever this page says `python`.

ffmpeg is the copy inside the imageio-ffmpeg package, so every machine encodes with the same version. To use
another, set `HELIOGRAPH_FFMPEG` to its path. Windows on ARM has no bundled copy: install ffmpeg there
(`winget install --id Gyan.FFmpeg -e`) and heliograph finds it on PATH; it also needs Python 3.12 or later,
as PyYAML publishes no 3.11 build for it. On Linux, skia-python also needs
libEGL and libGL, which desktop installs have (`sudo apt install libegl1 libgl1` otherwise).

## Make a video

```
python heliograph.pyz example.md
```

That checks `example.md` and writes `example.mp4` beside it. A scene with errors renders nothing, and each
error names its line and the fix. `python heliograph.pyz` on its own lists every command.

## Write a scene

```
python heliograph.pyz new --template network-overview -o my-scene.md    start from a template
python heliograph.pyz check my-scene.md       errors and layout warnings, each with its line and a fix
python heliograph.pyz review my-scene.md      the same with the fixes spelled out, plus writing suggestions
python heliograph.pyz preview my-scene.md     one frame per beat, in my-scene.preview.png
python heliograph.pyz frame my-scene.md -t 12 the frame at 12 seconds, full size
python heliograph.pyz my-scene.md             the MP4 (--draft: a quick 720p one; --gif: a looping GIF)
```

A scene is front matter that places icons on a backdrop and links them, then one line per beat:
`- START-END | actions | caption`. The guides are built in:

```
python heliograph.pyz guide brief             the format and the whole library on one page
python heliograph.pyz guide layout            backdrops, positions, and the fix for each layout warning
python heliograph.pyz guide writing           planning beats and writing captions
python heliograph.pyz layout stage            a picture of every position on the stage (also orbits, earth, panels)
python heliograph.pyz library list icons      every icon and what it shows (also links, themes, templates)
```

## With a language model

`SKILL.md` teaches a model to write scenes: the steps, the rules and the one-page reference. Paste it into
a chat, or install it as a skill: a folder named `heliograph-lite` holding `SKILL.md`, in your agent's
skills folder (`.opencode/skills` in an OpenCode project, for example). Run `check` on what the model writes
and give it the output: each message says how to fix the problem.

## Your own icons

Put them in a `library` folder where you run it, or name another with `--library DIR`.
`python heliograph.pyz guide library` explains the format and `library new icon NAME` starts one.

## Good to know

- It unpacks its package once into a cache folder, because it reads its fonts and library as ordinary
  files: `%LOCALAPPDATA%\heliograph\lite` on Windows, `~/.cache/heliograph/lite` on Linux and
  `~/Library/Caches/heliograph/lite` on macOS, or the folder in `HELIOGRAPH_LITE_CACHE`. On every run it
  checks those files against the hashes in `heliograph.pyz` and unpacks them again if any differ or
  anything has been added. Deleting the cache is always safe.
- It makes no network connections.
- `SHA256SUMS` lists the hash of every file here: after a download or a transfer, `sha256sum -c SHA256SUMS`
  (or `Get-FileHash heliograph.pyz` in PowerShell) shows that nothing changed on the way.
- A scene renders identically here and in the full heliograph, which adds the web app, the language-model
  tools and the MCP server.
