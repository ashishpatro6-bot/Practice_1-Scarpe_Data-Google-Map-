# Google Maps scraping (Selenium)

This project contains a small Selenium script that opens Google Maps place URLs and prints:

- **Title**
- **Rating**
- **Address**
- **Review text** *(best-effort; Google Maps HTML changes often)*

The main script to run is:

- `Scrape_data(GM)/combination.py`

## Requirements

- Windows + PowerShell
- Python 3.10+ (recommended)
- Google Chrome installed

## Setup (create `.venv` and install dependencies)

From the project folder:

```powershell
cd "C:\Users\ashis\OneDrive\Desktop\Python"
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -U pip
pip install -U selenium
```

If PowerShell blocks activation, run once:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## Run

Recommended (always uses the venv interpreter):

```powershell
cd "C:\Users\ashis\OneDrive\Desktop\Python"
.\.venv\Scripts\python.exe -u "Scrape_data(GM)\combination.py"
```

Alternatively, after activating `.venv`:

```powershell
python -u "Scrape_data(GM)\combination.py"
```

## What the script does (high level)

For each Google Maps URL in `urls`:

- Loads the page in Chrome (headless by default).
- Waits for the left details panel to load.
- Extracts:
  - **title**: from the browser tab title (`browser.title`)
  - **rating**: from an element whose `aria-label` contains `"stars"`
  - **address**: from `button[data-item-id='address']`
  - **reviews**: attempts to open the reviews panel and then collect visible review text nodes

The extracted values are printed to the console.

## Notes / troubleshooting

- **Reviews may show as `(not found)`**: Google Maps frequently changes markup and may render reviews differently (especially in headless). If that happens, try running with a visible browser by removing this line in `Scrape_data(GM)/combination.py`:
  - `options.add_argument("--headless=new")`
- If you see `ModuleNotFoundError: No module named 'selenium'`, you’re running outside the venv. Use:
  - `.\.venv\Scripts\python.exe ...`

