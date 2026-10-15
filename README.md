# MedGyan --- RAG-Based Medical Knowledge Assistant

MedGyan is a Retrieval-Augmented Generation (RAG) assistant project
intended to help users explore medical and health-related information by
grounding responses in a configured knowledge base.

The project is under active development. Its exact capabilities depend
on the data sources, retrieval pipeline, language model, and safeguards
implemented in the current version.

> **Important medical disclaimer:** MedGyan is an informational and
> development project, not a substitute for a qualified healthcare
> professional. It should not be used to diagnose conditions, prescribe
> treatment, or make emergency medical decisions. Verify health
> information with trusted clinical sources and consult a qualified
> professional for personal medical advice.

## Contents

-   [Project goals](#project-goals)
-   [Repository structure](#repository-structure)
-   [How a RAG assistant works](#how-a-rag-assistant-works)
-   [Technology](#technology)
-   [Getting started](#getting-started)
-   [Configuration](#configuration)
-   [Running MedGyan](#running-medgyan)
-   [Evaluation and testing](#evaluation-and-testing)
-   [Responsible use and safety](#responsible-use-and-safety)
-   [Roadmap](#roadmap)
-   [Contributing](#contributing)
-   [License](#license)

## Project goals

MedGyan is being developed around the following goals:

-   **Grounded answers:** use retrieved material from a configured
    knowledge base to inform responses.
-   **Accessible information:** make it easier to ask questions in
    natural language.
-   **Maintainable design:** keep application code, prompts, services,
    tests, and evaluation assets organized.
-   **Evaluation:** provide a place to test retrieval and answer quality
    as the project evolves.
-   **Responsible use:** make limitations clear and avoid presenting
    generated text as a clinical diagnosis.

These are project goals, not claims that every capability is already
implemented.

## Repository structure

The repository currently includes the following top-level files and
directories:

``` text
MedGyan/
├── app.py
├── config.py
├── prompts.py
├── docker-compose.yml
├── requirements.txt
├── README.md
├── data/
├── evaluation/
├── services/
├── tests/
└── ui/
```

-   `app.py` --- application entry point (confirm the current run
    command from the implementation).
-   `config.py` --- application configuration.
-   `prompts.py` --- prompt definitions.
-   `services/` --- service-layer code.
-   `data/` --- data or knowledge-base assets used by the project.
-   `evaluation/` --- evaluation resources and experiments.
-   `tests/` --- automated tests.
-   `ui/` --- user-interface code.
-   `requirements.txt` --- Python dependencies.
-   `docker-compose.yml` --- Docker Compose configuration, if used by
    the current setup.

Update these descriptions if the implementation differs.

## How a RAG assistant works

A typical Retrieval-Augmented Generation workflow has two stages:

1.  **Indexing:** prepare trusted documents, split them into chunks,
    generate searchable representations, and store them in a retrieval
    index.
2.  **Question answering:** accept a question, retrieve relevant
    passages, provide those passages as context to a language model, and
    generate a response grounded in that context.

``` text
Question
   |
   v
Retrieve relevant knowledge
   |
   v
Build context from retrieved passages
   |
   v
Generate a grounded response
   |
   v
Return answer (ideally with source references)
```

This diagram describes the general RAG pattern. The specific embedding
model, vector store, language model, chunking strategy, and citation
behavior should be documented once confirmed in the code.

## Technology

The repository is Python-based. The exact frameworks, model providers,
vector database, and interface libraries should be confirmed from
`requirements.txt` and the implementation before being listed as project
dependencies.

## Getting started

### Prerequisites

-   Python version compatible with the dependencies in
    `requirements.txt`
-   Git
-   Any API credentials required by the configured model or embedding
    provider
-   Docker and Docker Compose only if you choose to use the supplied
    Compose setup

### 1. Clone the repository

``` bash
git clone https://github.com/impyrion/MedGyan.git
cd MedGyan
```

If you already have the project locally, navigate to the project
directory instead.

### 2. Create and activate a virtual environment

On macOS or Linux:

``` bash
python3 -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell:

``` powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

``` bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure environment variables

Review `config.py` and the application code to identify the
configuration values required by your implementation. If the application
uses environment variables, create a local `.env` file or configure them
in your shell as appropriate.

For example, **only if your implementation expects these names**, a
local `.env` file might look like:

``` dotenv
# Example placeholders — replace with the variable names
# and values required by your implementation.
MODEL_API_KEY=your_api_key_here
```

Do not commit API keys, passwords, private datasets, or other secrets.
Keep `.env` out of version control and consider providing a
`.env.example` containing variable names but no real credentials.

### 5. Run the application

The exact launch command depends on how `app.py` and the `ui/` directory
are implemented. Inspect the entry point and project instructions, then
use the matching command. For a conventional Python entry point, it may
be:

``` bash
python app.py
```

Use that command only if `app.py` supports direct execution. If the UI
uses a framework-specific CLI, document and run that command instead.

## Configuration

Before sharing or deploying the project, document the following items
based on the actual implementation:

  -----------------------------------------------------------------------
  Setting                             What to document
  ----------------------------------- -----------------------------------
  Language model                      Provider and model identifier

  Embedding model                     Provider and model identifier

  Retrieval index                     Vector store or search backend

  Knowledge sources                   File formats, sources, and
                                      ingestion steps

  Credentials                         Required environment-variable names
                                      (never their values)

  Interface                           How to launch the user interface

  Persistence                         Where indexes, uploads, and
                                      generated artifacts are stored
  -----------------------------------------------------------------------

## Evaluation and testing

The repository contains `evaluation/` and `tests/` directories. Use them
to document and run the checks that are actually implemented.

For a RAG system, useful evaluation dimensions include:

-   **Retrieval relevance:** whether retrieved passages support the
    question.
-   **Faithfulness:** whether the answer is supported by the retrieved
    context.
-   **Answer relevance:** whether the response addresses the question.
-   **Citation correctness:** whether references point to the sources
    that support the answer, if citations are implemented.
-   **Safety:** whether the assistant handles uncertain, sensitive, or
    urgent medical questions appropriately.
-   **Latency and cost:** response time and model or infrastructure
    usage.

Run the test command configured by the project. If it uses `pytest`, the
command is commonly:

``` bash
pytest
```

This is a suggested command, not a guarantee that the current test suite
is configured for pytest. Add measured results only after running a
reproducible evaluation; do not publish unverified accuracy or
performance claims.

## Responsible use and safety

Because this project concerns medical information:

-   Prefer trusted, current, and appropriately licensed medical sources.
-   Preserve source metadata so retrieved passages can be traced to
    their origin.
-   Make it clear when evidence is missing, conflicting, or
    insufficient.
-   Do not present generated answers as a diagnosis or personalized
    treatment plan.
-   Avoid exposing personal health information in logs, prompts, test
    fixtures, or public datasets.
-   Provide clear guidance to seek professional or emergency care when
    appropriate.
-   Evaluate the system for hallucinations, unsafe recommendations, and
    misleading citations before real-world use.

## Roadmap

Potential future improvements, depending on the current implementation:

-   [ ] Document the exact data-ingestion and indexing workflow.
-   [ ] Record model, embedding, and retrieval configuration.
-   [ ] Add source-grounded citations to responses, if not already
    supported.
-   [ ] Build a reproducible evaluation set and report measured results.
-   [ ] Expand automated tests for retrieval quality and failure cases.
-   [ ] Add medical-safety tests and clear uncertainty handling.
-   [ ] Document deployment, monitoring, privacy, and data-retention
    practices.
-   [ ] Add screenshots or a short demo of the implemented interface.

## Contributing

Contributions and suggestions are welcome. Before opening a pull
request:

1.  Explain the problem or improvement.
2.  Keep changes focused and document any configuration changes.
3.  Add or update tests where appropriate.
4.  Never include secrets or private health information in commits.
5.  Report evaluation results with the method and dataset used.

## License

No license has been specified here because the repository's licensing
status has not been confirmed. Add a `LICENSE` file and update this
section once you have selected an appropriate license and verified that
you have the right to license all included code and data.
