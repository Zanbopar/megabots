# megabots

Build a Python chatbot that answers questions over your documents without writing the retrieval chain from scratch. megabots connects a document index to an OpenAI model and provides an API for document question answering, custom prompts, and conversation memory.

## Features

- Ask questions over a directory of documents or a previously saved index with `bot("qna-over-docs")`.
- Save an index to disk and load it again on a later run.
- Customize answer instructions with prompt templates that accept `context` and `question`.
- Add conversation memory so follow-up questions can refer to earlier messages.
- Connect document retrieval to Milvus, with configurable host and port settings.

## Installation

Use Python 3.10. From the project source directory, create a virtual environment and install the package:

```bash
python3.10 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install .
```

**An OpenAI API key is required for the examples.** Set it in your shell rather than saving it in source code:

```bash
export OPENAI_API_KEY="your-api-key"
```

To download the source archive for release `v0.1.0`, replace `OWNER` with this standalone project's GitHub owner. The tag must exist in that repository:

```bash
curl -fL https://github.com/OWNER/megabots/archive/refs/tags/v0.1.0.tar.gz -o megabots-v0.1.0.tar.gz
```

## Usage

Put the documents you want to query in `./documents`. Save this example as `ask_docs.py`:

```python
import argparse
from megabots import bot

parser = argparse.ArgumentParser(description="Ask a question over your documents")
parser.add_argument("question")
parser.add_argument("--index", default="./documents")
parser.add_argument("--save-index")
args = parser.parse_args()

qnabot = bot("qna-over-docs", index=args.index)
print(qnabot.ask(args.question))

if args.save_index:
    qnabot.save_index(args.save_index)
```

Create an index from your documents and ask a question:

```bash
python ask_docs.py "How do I configure this application?" --index ./documents --save-index ./index.pkl
```

Reuse the saved index on subsequent runs:

```bash
python ask_docs.py "What configuration options are available?" --index ./index.pkl
```

Index creation and answer generation can incur provider charges. Document content may be sent to the configured provider, so check your data-handling requirements before you run the example.

For follow-up questions, enable memory and reuse the same bot instance:

```python
from megabots import bot

qnabot = bot(
    "qna-over-docs",
    index="./index.pkl",
    memory="conversation-buffer",
)

print(qnabot.ask("What is the default configuration?"))
print(qnabot.ask("How do I change it?"))
```

Custom prompts must include `{context}` and `{question}`. When memory is enabled, also include `{history}` so the prompt receives the conversation history.

## License

MIT. This project is based on [momegas/megabots](https://github.com/momegas/megabots), which is also MIT-licensed. Retain the upstream copyright notice and MIT permission notice in copies or substantial portions of the original code.

## Related projects

The following repositories were supplied as other projects from the same author. Their functionality was not documented in the supplied material; check their READMEs for scope and usage.

- [mellyyup789-collab/java-microservice](https://github.com/mellyyup789-collab/java-microservice): Repository named `java-microservice`; implementation details were not supplied.
- [nicolac93/ragtune](https://github.com/nicolac93/ragtune): Repository named `ragtune`; supported workflows were not supplied.
- [sakthivelj10-ai/kstone](https://github.com/sakthivelj10-ai/kstone): Repository named `kstone`; its purpose was not supplied.

