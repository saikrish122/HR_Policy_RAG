# HR Policy RAG Walkthrough

A beginner-friendly Jupyter Notebook that demonstrates a basic Retrieval-Augmented Generation (RAG) workflow over a text-based HR policy document. It breaks the document into passages, indexes them in ChromaDB, retrieves passages related to a question, and asks a chat model to answer using the retrieved context.

## What the notebook does

1. Loads environment variables from a `.env` file.
2. Reads `data/Sample_HR_policy_Document.txt` as plain text.
3. Splits the document into line-based chunks, skipping short lines and separator lines.
4. Stores the chunks and source labels in a ChromaDB collection named `company_docs`.
5. Retrieves the three most relevant passages for a question.
6. Sends the question and retrieved context to the configured chat model and displays its answer.

The notebook uses the OpenAI Python SDK with Groq's OpenAI-compatible API endpoint (`https://api.groq.com/openai/v1`). The code requests the `openai/gpt-oss-120b` model.

## Project files

```text
project-folder/
├── app.ipynb
|___ README.md
|___.gitignore
├── .env                 # Create this yourself; do not commit it
└── data/
    └── Sample_HR_policy_Document.txt
```

## Requirements

- Python 3.10 or newer
- Jupyter Notebook or JupyterLab
- A Groq API key with access to the model used in the notebook

Install the packages used by the notebook:

```bash
python -m pip install jupyter python-dotenv chromadb pandas openai
```

`pandas` is imported in the notebook but is not used in the displayed RAG steps.

## Setup

1. Clone or download this project and open its folder in VS Code or a terminal.
2. Create and activate a virtual environment (recommended):

   **Windows PowerShell**
   ```powershell
   python -m venv .venv
   .venv\Scripts\Activate.ps1
   ```

   **macOS/Linux**
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

3. Install the requirements from the command above.
4. Create a `.env` file in the project folder and add your Groq key using the variable name read by the notebook:

   ```env
   OPENAI_API_KEY=your_groq_api_key_here
   ```

   Although the variable is named `OPENAI_API_KEY`, the notebook sends requests to Groq. Keep `.env` private and never upload your real key to GitHub.

5. Put the policy text file at `data/Sample_HR_policy_Document.txt`. Keep the same filename or update `TXT_PATH` in the notebook.
6. Open `app.ipynb` in Jupyter or VS Code, select the virtual environment's Python kernel, and run the cells from top to bottom. Make sure the notebook's current working directory is the project folder so it can find `.env` and `data/`.

## Ask a question

The final notebook cell contains an example:

```python
ask_rag("What is the work from home policy?")
```

Change the question to ask about information in the policy. The function also accepts `n_results` to control how many passages are retrieved:

```python
ask_rag("What is the leave policy?", n_results=3)
```

## Important notes

- The sample policy is demonstration content. Replace it only with policy text you are authorized to use.
- The answer is generated from retrieved text, but should still be checked against the source document.
- Calling the model requires network access and may use Groq API quota or incur costs according to your account and model terms.
- ChromaDB is initialized with `chromadb.Client()` in the notebook, which is an in-memory client. Its collection is rebuilt when the notebook cells are rerun; this code does not configure persistent database storage.
- The notebook is a learning demo, not a production HR support system.

## Troubleshooting

- **Policy file not found:** Confirm the file is located at `data/Sample_HR_policy_Document.txt` relative to the notebook's current working directory.
- **API key not loaded:** Confirm `.env` is in the project folder, the variable is spelled `OPENAI_API_KEY`, and restart/rerun the setup cells after editing it.
- **Model/API error:** Check that the API key is a valid Groq key and that the requested model is available to your account.
