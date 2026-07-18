# LangChain from Scratch: Getting Started with LCEL and OpenRouter

This project is a beginner-friendly, hands-on introduction to building LLM-powered applications using **LangChain** and the **LangChain Expression Language (LCEL)**. It demonstrates how to initialize a model via **OpenRouter**, run direct queries, structure prompts with templates, parse string outputs, and construct an end-to-end chain.

---

## 🚀 Overview

The primary goal of this project is to illustrate the core concepts of LangChain in a single, self-contained Jupyter Notebook ([Langchain.ipynb](file:///c:/Users/vigne/Desktop/Langchain/Langchain.ipynb)).

Here are the key concepts covered:
1. **Model Initialization**: Connecting to Gemini models through **OpenRouter** using LangChain's OpenAI integration.
2. **Direct Execution**: Querying the model using the `.invoke()` method.
3. **Prompt Engineering**: Using `PromptTemplate` to build reusable, dynamic prompts.
4. **Output Parsing**: Extracting clean text using `StrOutputParser`.
5. **LCEL (LangChain Expression Language)**: Orchestrating the workflow using the pipe operator (`|`) to seamlessly link templates, LLMs, and parsers.

---

## 🛠️ Tech Stack & Dependencies

- **Python** (version 3.10+)
- **LangChain & LangChain Core**: The foundational packages for LLM application orchestration.
- **LangChain OpenAI**: Used to interface with OpenRouter (which exposes an OpenAI-compatible API).
- **python-dotenv**: To securely manage API keys and configuration via a `.env` file.

---

## 📦 Setup & Installation

### 1. Clone the Repository
```bash
git clone https://github.com/igVigneshreddy/Langchain.git
cd Langchain
```

### 2. Set Up a Virtual Environment (Recommended)
```bash
python -m venv venv
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies
Run the following commands inside the notebook or in your terminal:
```bash
pip install langchain langchain-core langchain-openai python-dotenv
```

### 4. Configure Environment Variables
Create a `.env` file in the root directory:
```env
OPENROUTER_API_KEY=your_openrouter_api_key_here
```
> [!IMPORTANT]
> Never commit your `.env` file containing actual keys to Git. The `.gitignore` is already configured to ignore `.env` files.

---

## 💻 Code Explanation & Walkthrough

Here is a step-by-step walkthrough of the implementation in [Langchain.ipynb](file:///c:/Users/vigne/Desktop/Langchain/Langchain.ipynb):

### 1. Load Environment Variables
We secure our API key using `python-dotenv`:
```python
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv("OPENROUTER_API_KEY")
```

### 2. Initialize the LLM via OpenRouter
We use `ChatOpenAI` pointing to the OpenRouter endpoint, invoking the `google/gemini-2.5-flash` model:
```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="google/gemini-2.5-flash",
    temperature=0.7,
    max_tokens=1000,
    api_key=api_key,
    base_url="https://openrouter.ai/api/v1"
)
```

### 3. Direct Model Invocation
We call the model directly to test connection:
```python
prompt = "Suggest me a skill that is in demand?"
response = llm.invoke(prompt)
print("Suggested Skill:\n", response)
```

### 4. Create a Prompt Template & Output Parser
To make prompts dynamic and structure the output:
```python
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser

# Reusable prompt with placeholder {year}
template = "Give me 3 career skills that are in high demand in {year}."
prompt_template = PromptTemplate.from_template(template)

# Output parser to convert model output directly to a clean string
parser = StrOutputParser()
```

### 5. Build and Run the Chain (LCEL)
Using LangChain Expression Language (LCEL), we chain the prompt template, LLM, and parser together using the pipe (`|`) operator:
```python
# Chaining: PromptTemplate -> LLM -> Parser
chain = prompt_template | llm | parser

# Execute the chain
response = chain.invoke({"year": "2026"})
print("\nCareer Skills in 2026:\n", response)
```

---

## 🎯 Example Output

When running the chain with the year `2026`, you'll receive a structured output similar to:

```text
Career Skills in 2026:
Here are 3 career skills that are projected to be in high demand in 2026...

1. AI and Machine Learning Proficiency (including Prompt Engineering)
2. Advanced Data Literacy & Analytics
3. Complex Problem-Solving & Critical Thinking
```

---

## 📝 License
This project is open-source and available under the MIT License.
