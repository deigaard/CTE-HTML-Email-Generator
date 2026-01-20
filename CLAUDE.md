# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Running the Application
```bash
# From project root
cd src
streamlit run app.py
```

### Installing Dependencies
```bash
# Install Python dependencies
pip install -r src/requirements.txt

# For system packages needed for Chrome/Selenium (Linux)
# Install packages from packages.txt if running on Linux
```

### Python Environment
The project uses a virtual environment located at `src/venv/`. Activate it before development:
```bash
cd src
source venv/bin/activate  # On macOS/Linux
# or
venv\Scripts\activate  # On Windows
```

## Architecture Overview

This is a Streamlit-based web application for generating TAMU event newsletters with three main processing steps:

1. **Event Scraping** - Scrapes events from TAMU calendars using Selenium
2. **AI Categorization** - Uses OpenAI/OpenWebUI APIs to categorize scraped events
3. **Newsletter Generation** - Creates formatted HTML newsletters

### Core Structure

- **Entry Point**: `src/app.py` - Main Streamlit application
- **Services Layer**: `src/services/` - Core business logic
  - `scraper.py` - Event scraping from TAMU calendars
  - `categorizer.py` - AI-powered event categorization
  - `newsletter.py` - HTML newsletter generation
- **UI Components**: `src/components/` - Streamlit UI components for each step
- **Utilities**: `src/utils/` - Shared utilities for logging, state management, and process execution

### Key Design Patterns

- **Session State Management**: Uses `StateManager` class to handle Streamlit session state across steps
- **Callback-based UI**: Each step has a callback function that handles the business logic
- **Service Layer Architecture**: Business logic is separated into service classes
- **Security-First API Handling**: API keys are passed directly and cleared from memory immediately after use

### Data Flow

1. User inputs date range → `EventScraper.scrape_events()` → Events stored in session state
2. User provides API key → `EventCategorizer.categorize_events()` → Categorized events stored in session state
3. User generates newsletter → `NewsletterGenerator.generate_newsletter()` → HTML content stored in session state

### Dependencies

- **Streamlit** - Web UI framework
- **Selenium + WebDriver Manager** - Web scraping
- **OpenAI/LangChain** - AI categorization
- **Pandas** - Data manipulation
- **Google Sheets API** - Optional data export

### Security Considerations

- API keys are never stored in session state or environment variables
- Keys are cleared from memory immediately after use
- Subprocess execution uses secure environment handling

### File Structure Notes

- The main app file is `src/app.py`, not at project root
- Virtual environment is included in `src/venv/`
- System packages required for Chrome/Selenium are listed in `packages.txt`
- Python dependencies are in `src/requirements.txt`