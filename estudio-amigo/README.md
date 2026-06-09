# Estudio Amigo — AI academic mentor for Sciences, Mathematics, Psychology, Environment, and CBSE Commerce (Classes 11-12).

A simple Flask web client for interacting with a Microsoft Foundry / Azure-hosted agent that provides computing history information.

## Quick start (for contributors & testers)

Prerequisites:
- Python 3.8+
- Git
- An Azure identity able to call the agent endpoint (DefaultAzureCredential works with Azure CLI login or env vars)

Clone and run:

```powershell
# Clone the repository
git clone https://github.com/YOUR_USERNAME/estudio-amigo-ai-agent.git
cd estudio-amigo-ai-agent/estudio-amigo

# Create and activate virtual environment (Windows PowerShell)
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Create .env file (see example below) and set AGENT_ENDPOINT and any Azure auth variables
# Run the app
python app.py
```

Open http://localhost:5000 in your browser.

Example `.env` entries (`.env` should NOT be committed):

```
AGENT_ENDPOINT=https://<your-foundry-agent-endpoint>/responses
# Optional (if you're using service principal creds locally):
# AZURE_CLIENT_ID=...
# AZURE_TENANT_ID=...
# AZURE_CLIENT_SECRET=...
```

Notes
- The app reads `AGENT_ENDPOINT` from environment variables. `agent_client.py` uses `DefaultAzureCredential` to obtain tokens — you can sign in locally with `az login` or set the service principal env vars.
- The folder was renamed to `estudio-amigo` from `computer-history-client`. If you require the original folder path for compatibility, copy `estudio-amigo/agent_client.py` back to `computer-history-client/agent_client.py`.
- Do not commit `.env` or the virtual environment — `.gitignore` is configured to ignore them.

## Deploy to Render

Render is a good fit for this Flask app. The key requirements are:
- A Python Web Service
- `pip install -r estudio-amigo/requirements.txt`
- `python estudio-amigo/app.py` or `gunicorn --chdir estudio-amigo app:app`
- environment variables set in Render for `AGENT_ENDPOINT` and any Azure auth

### Deploy steps

1. Create a new Web Service on Render.
2. Connect your GitHub repo and select the `main` branch.
3. Set the Build Command:

```bash
pip install -r estudio-amigo/requirements.txt
```

4. Set the Start Command:

```bash
gunicorn --chdir estudio-amigo app:app
```

5. Add environment variables in Render:
- `AGENT_ENDPOINT` (your Foundry/Azure endpoint)
- Optional Azure identity variables if needed: `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_CLIENT_SECRET`

6. Deploy.

After deployment, Render will assign a public URL to your service.

Contributing
- Make changes on a feature branch and open a pull request.

License
- MIT
