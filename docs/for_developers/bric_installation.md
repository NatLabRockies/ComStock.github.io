---
layout: default
title: BuildStock Rate Impact Calculator Installation
parent: For Developers
---

# Install the BuildStock Rate Impact Calculator
{: .no_toc }

The BuildStock Rate Impact Calculator (BRIC) is a Streamlit web application for calculating how building upgrades affect electricity bills using [ResStock](https://resstock.nlr.gov/) and [ComStock](https://comstock.nlr.gov/) simulation data. Once the app is installed, see [Calculate Custom Rates with the BuildStock Rate Impact Calculator]({{  site.baseurl  }}{% link docs/resources/how_to_guides/bric_rate_impact_calculator.md %}) for how to use it.

There are two ways to install the app:

- **[Option 1: Download the app](#option-1-download-the-app-recommended)** (recommended). Download one file and double-click it. No terminal, no other software, and no administrator rights are needed.
- **[Option 2: Install with pixi](#option-2-install-with-pixi)**. Use this if you want to change the code, or if Option 1 doesn't work on your computer.

<details open markdown="block">
<summary>Contents</summary>
{::options toc_levels="2" /}
1. TOC
{:toc}
</details>

## Option 1: Download the app (recommended)

### Download

1.	Download the zip file for your computer. The current version is v0.9.0; earlier versions are on the [releases page](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/releases).

    | Computer | File | Size |
    |---|---|---|
    | Windows | [BRIC-v0.9.0-win-64.zip](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/releases/download/v0.9.0/BRIC-v0.9.0-win-64.zip) | 207 MB |
    | Mac with Apple silicon (M1 or newer) | [BRIC-v0.9.0-osx-arm64.zip](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/releases/download/v0.9.0/BRIC-v0.9.0-osx-arm64.zip) | 198 MB |
    | Mac with an Intel processor | [BRIC-v0.9.0-osx-64.zip](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/releases/download/v0.9.0/BRIC-v0.9.0-osx-64.zip) | 211 MB |
    | Linux | [BRIC-v0.9.0-linux-64.zip](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/releases/download/v0.9.0/BRIC-v0.9.0-linux-64.zip) | 274 MB |

    On a Mac, choose **Apple menu → About This Mac**. "Chip: Apple M…" means Apple silicon; "Processor: Intel…" means Intel.

2.	**Windows only:** before extracting, right-click the zip file, choose **Properties**, check **Unblock** (if shown), and click **OK**. This prevents security warnings later.
3.	Extract the zip. On Windows, right-click it and choose **Extract All...**; on a Mac, double-click it. Extract it anywhere (for example, Downloads). You can delete the folder once the app has set itself up.

### Start the app

Open the extracted folder and double-click:

- **Windows:** `Start BRIC.cmd`
- **macOS:** `Start BRIC.command`
- **Linux:** `start-bric.sh` (or run `./start-bric.sh` in a terminal)

A window with some text opens, followed by the app in your web browser.

The **first time** you start a new version, the app sets itself up before opening. This takes a minute or so and needs about 1.5 GB of free disk space. Later starts take a few seconds.

To stop the app, click **Quit app** at the bottom of the app's sidebar, or close the text window. The app also stops on its own about 2 minutes after you close its browser tab.

#### Security warnings

The app isn't signed with a Microsoft or Apple certificate yet, so the first start may show a warning:

- **Windows, "Windows protected your PC":** click **More info**, then **Run anyway**. For an "Open File – Security Warning," click **Run**.
- **macOS, "cannot be opened because it is from an unidentified developer":** right-click (or Control-click) `Start BRIC.command`, choose **Open**, then click **Open** again. You only need to do this once.

### Updating and uninstalling

- **Update:** download and extract the new version's zip, and start it the same way. It sets itself up once and removes the previous version.
- **Uninstall:** delete the app's folder:
  - Windows: `%LOCALAPPDATA%\BRIC` (paste this into the File Explorer address bar)
  - macOS: `~/Library/Application Support/BRIC`
  - Linux: `~/.local/share/BRIC`

Updating or uninstalling never touches your downloaded data, projects, or saved OpenEI API key. These live in the **Data Directory** shown in the app's **Setup** panel (by default, `Documents/buildstock_results`).

### If the app stops unexpectedly

If the app stops on its own (the browser shows a connection error), the text window stays open and shows the end of the app's log. Please send that log file to the BRIC developers by [opening an issue](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/issues). Logs of the last 10 runs are kept in:

- Windows: `%LOCALAPPDATA%\BRIC\logs`
- macOS: `~/Library/Logs/BRIC`
- Linux: `~/.local/state/BRIC`

Next, set up your [OpenEI API key](#openei-api-key-optional).

## Option 2: Install with pixi

Use this option if you want to change the code, or if Option 1 doesn't work on your computer. Installing pixi may need administrator rights.

1.	Install [pixi](https://pixi.prefix.dev/latest/installation/), then open a new terminal so the `pixi` command is found.
2.	Get the code, either by cloning it or by downloading the ZIP from the [repository](https://github.com/NatLabRockies/buildstock-rate-impact-calculator) (**Code → Download ZIP**) and unzipping it:

    ```bash
    git clone https://github.com/NatLabRockies/buildstock-rate-impact-calculator.git
    cd buildstock-rate-impact-calculator
    ```

3.	From the project folder, install the environment. The first run takes a few minutes.

    ```bash
    pixi install
    ```

4.	Start the app, then open the address it prints (usually `http://localhost:8501`) in your browser:

    ```bash
    pixi run app
    ```

To stop the app, click **Quit app** at the bottom of the app's sidebar or press `Ctrl+C` in the terminal. The app also stops on its own about 2 minutes after you close its last browser tab.

## OpenEI API key (optional)

Some features, such as utility rate lookups, require a free API key from OpenEI.

1.	Go to <https://openei.org/services/api/signup/>.
2.	Sign up and get an API key.
3.	Open the app in your browser.
4.	In the app's sidebar, find the **OpenEI API Key** field under **Setup**.
5.	Paste your API key into that field.

## Using VS Code (optional)

You do **not** need VS Code to run the app. If you want to use it anyway:

1.	Download and install VS Code from <https://code.visualstudio.com/>.
2.	Open VS Code.
3.	Click **File → Open Folder...** and choose the project folder.
4.	Click **Terminal → New Terminal**.
5.	Run the same commands in the VS Code terminal:

    ```bash
    pixi install
    pixi run app
    ```

## Development commands

If you are contributing to the project, run these commands from the project folder:

| Task | Command |
|---|---|
| Run the app | `pixi run app` |
| Run tests | `pixi run test` |
| Lint the code | `pixi run lint` |
| Fetch utility data | `pixi run fetch-utilities` |
| Build the stand-alone downloads | `pixi run pack` (see [standalone/README.md](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/blob/main/standalone/README.md)) |

The app's license is in the [repository](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/blob/main/LICENSE.md).
