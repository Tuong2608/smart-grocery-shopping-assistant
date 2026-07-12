# Smart Grocery Shopping Assistant

An AI-powered Streamlit application that turns ingredients already in your kitchen
into a structured multi-meal menu, step-by-step recipes, and a consolidated shopping
list for only the missing items.

This compact portfolio project demonstrates production-minded LLM application design:
schema-constrained output, prompt-injection resistance, input validation, secret
management, graceful failures, and a practical user interface.

## Features

- Accepts free-form pantry input such as `3 eggs, some pork, tofu`
- Generates one to five recipes with prep/cook times and serving counts
- Labels recipe ingredients as already available or needing purchase
- Consolidates missing items into a categorized, interactive shopping list
- Supports cuisine preferences and dietary restrictions
- Shows assumptions and relevant food-safety notes
- Exports the complete plan as validated JSON
- Preserves the latest result across Streamlit reruns

## Tech stack

- **Python 3.10+**
- **Streamlit** for the web interface
- **Gemini API** via Google's current `google-genai` Python SDK
- **Pydantic v2** for schema definition and response validation
- **Gemini structured output** for reliable, machine-readable results

> Google has superseded the legacy `google-generativeai` package with
> `google-genai`. This project uses the maintained SDK.

## Project structure

```text
.
├── app.py
├── requirements.txt
└── README.md
```

## Run locally

1. Clone the repository and enter it:

   ```bash
   git clone https://github.com/YOUR_USERNAME/smart-grocery-assistant.git
   cd smart-grocery-assistant
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   ```

   macOS/Linux:

   ```bash
   source .venv/bin/activate
   ```

   Windows PowerShell:

   ```powershell
   .venv\Scripts\Activate.ps1
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Create a Gemini API key in [Google AI Studio](https://aistudio.google.com/app/apikey),
   then expose it using either option below.

   Environment variable (macOS/Linux):

   ```bash
   export GEMINI_API_KEY="your-api-key"
   ```

   Environment variable (Windows PowerShell):

   ```powershell
   $env:GEMINI_API_KEY="your-api-key"
   ```

   Or create `.streamlit/secrets.toml`:

   ```toml
   GEMINI_API_KEY = "your-api-key"
   ```

5. Start the app:

   ```bash
   streamlit run app.py
   ```

Open the local URL shown by Streamlit, normally `http://localhost:8501`.

## Configuration

The default model is `gemini-2.5-flash`. Override it without changing source code:

```bash
export GEMINI_MODEL="gemini-2.5-flash"
```

On Windows PowerShell:

```powershell
$env:GEMINI_MODEL="gemini-2.5-flash"
```

## Prompt-engineering approach

The application combines a tightly scoped system instruction with a separate user
payload. The prompt tells the model to treat pantry text only as data, honor dietary
constraints, track quantities across recipes, minimize waste, consolidate duplicates,
and surface assumptions. Gemini is also given a Pydantic response schema, so the
result is JSON with known fields rather than free-form Markdown. The response is
validated again before it reaches the UI.

## Deployment on Streamlit Community Cloud

1. Push these files to a public GitHub repository.
2. In Streamlit Community Cloud, create an app and select `app.py`.
3. Add `GEMINI_API_KEY = "your-api-key"` under the app's **Secrets** settings.
4. Deploy.

Never commit an API key or `.streamlit/secrets.toml` to Git.

## Limitations and responsible use

AI-generated recipes can contain errors. Users should independently verify allergens,
ingredient labels, safe cooking temperatures, freshness, and dietary suitability.
This application is a planning aid, not medical or professional nutrition advice.

## Ideas for future development

- Pantry image recognition and receipt scanning
- Nutrition estimates and price-aware store integrations
- Persistent user accounts and saved pantry inventory
- Multilingual menus and metric/imperial unit conversion
- Automated tests with mocked model responses

## License

Add the license that fits your intended use (MIT is a common choice for portfolio
projects).
