# O&M Robotics — Onshape CAD Management Automation

A set of Python scripts that automate the organization and management of CAD files within [Onshape](https://www.onshape.com), built for the O&M Robotics project suite.

## Overview

This project manages a library of 373+ Onshape documents across five engineering sub-projects. The scripts handle bulk document organization, folder structure creation, and STEP file uploading — all without consuming Onshape's limited API quota by authenticating through an existing browser session.

## Projects

| Folder | Description |
|---|---|
| **Base Vehicle** | Main robot chassis and drivetrain assemblies |
| **Cleaning Robot Tower** | Vertical tower mechanism with brush and roller systems |
| **Brush Cage & Roller** | Cage assembly and roller sub-components |
| **ELB** | Separate sub-project assembly |
| **Standard Hardware** | Shared fasteners, brackets, and off-the-shelf components |

Each project folder contains `Assemblies` and `Parts` sub-folders in Onshape.

## Scripts

### `fix_empty_assemblies.py`
Re-uploads STEP files for assembly documents that exist in Onshape but contain no CAD data. Uses browser session authentication (no API key required).

**Uploads:**
- `BV-ASM-222-100.stp` → Base Vehicle / Assemblies
- `Tower Assm.stp` → Cleaning Robot Tower / Assemblies

### How it works

Rather than using Onshape's REST API with an access key (which has a 2,500 request/year quota), these scripts authenticate by reading session cookies directly from the local browser — the same session used when you're logged in to Onshape in your browser. This allows unlimited API calls without touching the quota.

Key techniques:
- **Browser session auth** via `browser_cookie3` — reads Chrome/Brave/Edge cookies automatically
- **XSRF-TOKEN forwarding** — extracts and forwards the anti-CSRF token required by Onshape's API
- **Multipart STEP upload** — streams large binary files to Onshape's blob element endpoint
- **Graceful fallback** — if auto cookie-read fails (e.g. permission issues), the script prompts for manual cookie paste from DevTools

## Requirements

- Python 3.8+
- A browser (Brave, Chrome, or Edge) logged into [cad.onshape.com](https://cad.onshape.com)

Dependencies are installed automatically on first run:
```
requests
browser-cookie3
```

## Usage

```bash
python fix_empty_assemblies.py
```

Run from VS Code terminal or any Python environment. If the script can't read browser cookies automatically, it will walk you through copying them manually from DevTools.

## File Structure

```
O&M Files/
├── fix_empty_assemblies.py        # Re-upload empty assembly STEP files
├── run_fix_assemblies.bat         # Windows launcher (optional)
├── BASE VEHICLE DESIGN PROJECT/
│   └── BV-ASM-222-100 STEP Files/
│       └── BV-ASM-222-100.stp
└── CLEANING ROBOT TOWER PROJECT/
    └── Tower Assm STEP Files/
        └── Tower Assm.stp
```

## License

MIT — see [LICENSE](LICENSE)
