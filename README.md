# RuleSum — Gradio UI (IRAC KG vs Zero-shot)

This repo contains a **Google Colab notebook** that reproduces the RuleSum pipeline:
**IRAC Knowledge Graph → IRAC-Quota Top-K → Kaping-IRAC serialization → GPT-4o summary**, plus a **Zero-shot** baseline. The UI also renders **IRAC** and **Plain** KGs and reports **SBERT similarity** and **readability (FKGL)**.

## Features

* Paste a legal document and compare **Best (structured)** vs **Zero-shot** summaries
* Visualize **IRAC** and **Plain** knowledge graphs
* Inspect **Top-K (IRAC-Quota)** triples and the **Kaping-IRAC** block used for prompting
* Quick metrics: **SBERT(similarity to source)** and **FKGL(readability)**

---

## Run in Google Colab (recommended)

1. **Open the notebook from GitHub**

* Go to Google Colab → **File → Open Notebook → GitHub** tab
* Paste this repo URL and open **`RuleSum_Gradio_Colab.ipynb`**
  (or download the `.ipynb` and use **Upload** tab)

2. **Set runtime**

* **Runtime → Change runtime type → Python 3** (GPU not required)

3. **Run the cells in order**

* The **Setup** cell installs all dependencies (Gradio, LangChain, OpenAI, etc.) and system **graphviz**
* The **API Key** cell prompts you to set:

  ```python
  import os
  os.environ["OPENAI_API_KEY"] = "sk-...your key..."
  ```
* The **Demo data** cell downloads **5 MultiLexSum cases** for quick testing
* The **Gradio App** cell launches the UI; click the link that appears under the cell

> **Tip:** If Colab asks to restart after installs, accept and re-run the cells.

---

## Notebook cells (reference)

**Install & system packages**

```python
!pip -q install gradio>=4.38.1 networkx>=3.2 pydot>=2.0.0 graphviz>=0.20.3 \
               httpx>=0.27.0 openai>=1.43.0 langchain>=0.2.15 \
               langchain-openai>=0.1.24 langchain-experimental>=0.0.64 \
               sentence-transformers>=3.0.1 textstat>=0.7.4 huggingface_hub>=0.24.6
!apt-get -qq update && apt-get -qq install -y graphviz > /dev/null
```

**Set your OpenAI key**

```python
import os
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY") or input("Paste OPENAI_API_KEY: ").strip()
os.environ["OPENAI_API_KEY"] = OPENAI_API_KEY
print("API key set:", bool(os.environ.get("OPENAI_API_KEY")))
```

Then just run the remaining cells (KG extraction, Top-K selection, Kaping-IRAC, metrics, and **Gradio UI**).

---

## Troubleshooting

* **Dependency resolver warnings** in Colab are normal; if imports fail, re-run the Setup cell and then **Runtime → Restart runtime**.
* If **Graphviz** SVGs don’t render, the notebook falls back to a PNG plot.
* If the **Gradio link** doesn’t appear, re-run the final cell after the runtime restart.

---

## Privacy

The notebook keeps data in the current Colab session only. Do not commit your **OPENAI_API_KEY** to the repo or share notebooks with keys embedded.
