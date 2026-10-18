# ScopeGuard

A single static page that checks a client message against the scope you agreed on, without sending either of them anywhere.

Freelancers sign a scope, then the scope quietly grows: "while you're in there, can you also...". ScopeGuard turns that feeling into numbers you can act on. It is built for the case where the text is confidential - an NDA, a client's internal roadmap, a legal review - and pasting it into a cloud tool is not an option.

## What it does

1. Paste the agreed scope (brief, proposal, signed SOW).
2. Paste the client message you just received.
3. Each message sentence is embedded locally and compared with every scope sentence. The closest match is shown per line, and lines below a threshold you control are flagged.
4. A reply draft is generated from the flagged lines - a plain template with your lines dropped in, so you can send a change order instead of absorbing the work.

Everything runs in the tab. There is no backend, no account, no telemetry, and after the model is cached you can turn off the network and it still works.

## How it works

- transformers.js (Apache-2.0) runs the model in the browser through onnxruntime-web.
- The model is Xenova/all-MiniLM-L6-v2, an open-weight sentence embedding model (Apache-2.0), loaded quantized (dtype q8, about 23 MB) from the public Hugging Face hub on first run only.
- Similarity is cosine similarity between mean-pooled, L2-normalised sentence embeddings.
- Sentence splitting is a small regex pass; the threshold is yours to tune (default 0.45).

## Run it

Open index.html in a modern browser, or serve the folder with `python -m http.server 8000`, then visit http://127.0.0.1:8000.

Hosted copy: https://zoizero86-beep.github.io/scopeguard/

## Limits, stated plainly

- A small embedding model measures topical relatedness, not intent. It will occasionally flag a sentence that is genuinely in scope, and it will not notice a scope change that reuses your own words.
- The reply draft is a template. No model writes it, and you should edit it before sending.
- English-language scope text is what it was tuned on; sentences shorter than four words are ignored.

## License

MIT.
