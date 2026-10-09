# Instagram Reel Automation

A Python-based browser automation project for preparing and publishing Instagram Reels using Selenium and Brave Browser.

The project is designed with a modular architecture so that browser management, Instagram authentication, video uploads, caption handling, and publishing can be developed and maintained independently.

> **Project status:** Refactoring from a working standalone script into a modular Python application. Instagram's web interface can change, so selectors and workflows may require maintenance.

## Features

* **Persistent browser session** — reuse a dedicated Brave browser profile.
* **Session detection** — check whether an existing Instagram session is available.
* **Manual security verification** — pause for the user to complete verification when required.
* **Video upload** — select a local video file for the Reel creation workflow.
* **Caption management** — enter captions, including Arabic text and multiline content.
* **Publishing workflow** — navigate Instagram's creation interface and initiate publishing.
* **Diagnostics** — save screenshots and HTML snapshots to help troubleshoot failures.
* **Modular design** — separate application responsibilities into Python modules.

## Technology Stack

* Python
* Selenium WebDriver
* Brave Browser
* Git and GitHub

## Project Structure

```text
instagram-reel-automation/
├── src/
│   └── instagram_reel_automation/
│       ├── __init__.py
│       ├── __main__.py
│       ├── config.py
│       ├── browser.py
│       ├── exceptions.py
│       ├── diagnostics.py
│       └── services/
│           ├── __init__.py
│           ├── instagram_auth.py
│           ├── create_flow.py
│           └── reel_publisher.py
├── tests/
├── .env.example
├── .gitignore
├── config.example.json
├── pyproject.toml
├── requirements.txt
└── README.md
```

### Module Responsibilities

| Module                       | Responsibility                                                           |
| ---------------------------- | ------------------------------------------------------------------------ |
| `config.py`                  | Load and validate local configuration                                    |
| `browser.py`                 | Configure, start, and close Brave WebDriver                              |
| `services/instagram_auth.py` | Check sessions, sign in, and wait for manual verification                |
| `services/create_flow.py`    | Open the creation interface and upload video files                       |
| `services/reel_publisher.py` | Enter captions, navigate publishing steps, and report publication status |
| `diagnostics.py`             | Capture screenshots and HTML for troubleshooting                         |
| `exceptions.py`              | Define application-specific exceptions                                   |
| `__main__.py`                | Coordinate the automation workflow                                       |
| `tests/`                     | Test validation and application logic                                    |

## Requirements

* Python 3.11 or another version supported by your installed Selenium release
* Brave Browser installed on your computer
* A valid Instagram account and access to its login session
* Git for version control

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/instagram-reel-automation.git
cd instagram-reel-automation
```

### 2. Create a virtual environment

**Windows PowerShell**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -e .
```

If the project is not yet configured for editable installation, install its dependencies directly:

```bash
pip install -r requirements.txt
```

### 4. Configure the application

Copy the example configuration:

```powershell
Copy-Item config.example.json config.json
```

Update `config.json` with your local settings. Never commit real passwords, browser session data, or private configuration files.

Example configuration:

```json
{
  "instagram": {
    "username": "YOUR_USERNAME",
    "password": "SET_LOCALLY"
  },
  "browser": {
    "brave_path": "C:\\Path\\To\\brave.exe",
    "profile_dir": "browser-profile"
  },
  "reel": {
    "video_path": "videos\\example.mp4",
    "caption": "Friday\n\nSurah Al-Kahf"
  }
}
```

Use the actual Brave executable path and video path on your computer. The example is a template, not a ready-to-run configuration.

### 5. Run the application

Once the modular entry point is implemented:

```bash
python -m instagram_reel_automation
```

During development, the original standalone script can be used as a reference while functionality is migrated into the new modules.

## Typical Workflow

1. Load and validate configuration.
2. Start Brave using a dedicated browser profile.
3. Check for an existing Instagram session.
4. Sign in if necessary.
5. Pause for any required manual security verification.
6. Open the Reel creation workflow.
7. Select and upload the configured video.
8. Enter the caption.
9. Initiate publishing.
10. Report success, failure, or an unconfirmed result.

## Configuration and Security

* Keep `config.json` and `.env` out of version control.
* Never commit passwords, session cookies, browser profiles, or private tokens.
* Keep the browser profile separate from your everyday Brave profile.
* Avoid committing personal videos, screenshots, or HTML snapshots that contain private account information.
* Use environment variables or a local secrets manager for sensitive values as the project matures.
* If credentials are accidentally published, rotate them immediately.

## Reliability and Error Handling

Instagram's web interface is subject to change. Automation selectors should be treated as implementation details, not permanent identifiers.

The application should distinguish between:

* **Published:** an appropriate confirmation or other reliable evidence indicates success.
* **Failed:** an explicit error or failed workflow is detected.
* **Unconfirmed:** the action was attempted, but publication could not be verified.

A successful Selenium click does not, by itself, prove that a Reel was published.

Before retrying an unconfirmed publication, check the Instagram profile to avoid duplicate posts.

## Troubleshooting

### Brave fails to start

* Verify the configured Brave executable path.
* Check whether another Brave process is using the dedicated profile.
* Check Selenium and browser compatibility.
* Review the exception and saved diagnostics.

### Video file not found

* Verify the configured video path.
* Confirm that the file exists and is accessible.
* Prefer absolute paths during initial development.

### Instagram elements cannot be located

* Check the current browser screen.
* Inspect the latest screenshot and HTML diagnostic files.
* Update the relevant selector only after inspecting the actual page.
* Avoid blindly clicking unrelated controls when a selector fails.

### Publication status is unclear

* Check the Reel on the Instagram profile.
* Inspect any visible errors or pending-upload state.
* Do not automatically retry an unconfirmed publication.

## Development Roadmap

* [ ] Separate browser management from workflow logic.
* [ ] Extract configuration loading and validation.
* [ ] Modularize authentication and session handling.
* [ ] Separate upload and caption operations.
* [ ] Improve publication-status detection.
* [ ] Add structured logging.
* [ ] Add unit tests for configuration and validation.
* [ ] Add workflow-level tests using controlled test doubles.
* [ ] Support a queue of videos and captions.
* [ ] Add scheduling and retry policies.
* [ ] Add a database for job status and publication history.
* [ ] Add reporting and operational monitoring.

## Contributing

1. Create a feature branch.
2. Make one focused change at a time.
3. Test the affected module.
4. Review the Git diff for credentials and private data.
5. Submit a pull request with a clear explanation of the change.

## Responsible Use

Use this project only for accounts you are authorized to access. Complete security challenges manually, respect Instagram's rules and applicable limits, and avoid behavior that could compromise account security.

This project is an independent automation project and is not affiliated with or endorsed by Instagram or Meta.

## License

Choose a license before distributing the project. If you want others to reuse and modify your code, consider the MIT License. Otherwise, leave the license unspecified until you decide how you want the project to be shared.
