# Awesome Context Engineering [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated, **auto-updating**, **practitioner-first** list for context engineering: getting the right tokens into an LLM's window, and fighting the failure modes when you do not.

Prompt engineering was about wording one message. Context engineering is the systems problem underneath agents: what goes into the window, in what order, compressed how much, and thrown away when. More context is not better; the *right* context is. This list is organized around that, and around the concrete failure modes, with the open-source tool that mitigates each.

**Browse and filter: [context-engineering.agentpostmortem.com](https://context-engineering.agentpostmortem.com)**

## Start here: the mental model

**The four moves** (LangChain's framing): **write** context (scratchpads, memory), **select** what to pull in (retrieval, tools), **compress** it (summaries, compaction), and **isolate** it (sub-agents, separate windows).

**The failure modes, and where to look below:**
- **Context rot.** Quality degrades as the window fills, not gracefully. *See: long-context benchmarks, compaction.*
- **Context bloat.** You pay for tokens you did not need. *See: token counting, profiling, compression.*
- **Lost in the middle.** The model ignores content buried mid-window. *See: retrieval, compression.*

**The one rule:** a bigger context window is a budget, not a solution. Spend it on the smallest set of tokens that makes the model right.

<!-- LIST:START -->
**39 entries**, auto-refreshed weekly. Star counts updated **2026-08-01**. Browse the filterable version at **[context-engineering.agentpostmortem.com](https://context-engineering.agentpostmortem.com)**.

### Foundations and definitions

- [Effective context engineering (Anthropic)](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents): Anthropic's formal definition: curating and maintaining the optimal set of tokens during LLM inference.
- [Context Engineering for Agents (LangChain)](https://www.langchain.com/blog/context-engineering-for-agents): LangChain's canonical framing of the four moves: write, select, compress, isolate.
- [Context Engineering for Agents (Lance Martin)](https://rlancemartin.github.io/2025/06/23/context_engineering/): Widely-cited practitioner essay on managing an agent's context across its trajectory.
- [Context Engineering: A Practical Guide (Sourcegraph)](https://sourcegraph.com/blog/context-engineering): Practical guide to context engineering for coding agents from the Sourcegraph/Amp team.
- [Context Engineering 101 (Victor Dibia)](https://newsletter.victordibia.com/p/context-engineering-101-how-agents): How agents implement context-engineering strategies, worked through Claude Code.

### Context management and compaction

- [context-mode](https://github.com/mksglu/context-mode) `* 19.5k`: MCP server that sandboxes tool output (up to 98% token reduction) and persists session memory in SQLite with BM25 retrieval on compaction.
- [hermes-lcm](https://github.com/stephenschoettler/hermes-lcm) `* 930`: DAG-based context engine that compacts old context into depth-aware summary nodes without dropping messages.
- [context-llemur](https://github.com/jerpint/context-llemur) `* 93`: Structural, human-plus-LLM-managed context tool for collaboration, no embeddings.
- [Context Compaction teardown (badlogic)](https://gist.github.com/badlogic/cd2ef65b0697c4dbe2d13fbecb0a0a5f): Comparative teardown of how Claude Code, Codex CLI, OpenCode, and Amp handle context compaction.

### Retrieval and RAG

- [LlamaIndex](https://github.com/run-llama/llama_index) `* 51.3k`: Leading data framework for ingesting, indexing, and retrieving context to augment LLM output.
- [Chroma](https://github.com/chroma-core/chroma) `* 28.9k`: Open-source vector database for building retrieval and context layers for LLM apps.

### Prompt and context compression

- [LLMLingua (Microsoft)](https://github.com/microsoft/LLMLingua) `* 6.5k`: Coarse-to-fine prompt compression using a small LM to drop low-information tokens; LLMLingua-2 and LongLLMLingua included.
- [LLMLingua paper](https://arxiv.org/abs/2310.05736): Original paper on compressing prompts for accelerated inference while preserving quality.

### Memory systems

- [Mem0](https://github.com/mem0ai/mem0) `* 62.2k`: Lightweight open-source memory layer adding persistent memory to any LLM app over vector and relational storage.
- [Cognee](https://github.com/topoteretes/cognee) `* 29.6k`: Open-source memory framework building knowledge graphs from data for agent long-term memory.
- [Graphiti (Zep)](https://github.com/getzep/graphiti) `* 29.4k`: Temporal knowledge-graph memory that tracks how facts change over time with provenance.
- [Letta (formerly MemGPT)](https://github.com/letta-ai/letta) `* 24k`: Stateful-agent platform with main/recall/archival memory tiers the model pages in and out via tool calls.

### Token counting and cost

- [tiktoken (OpenAI)](https://github.com/openai/tiktoken) `* 18.9k`: OpenAI's official BPE tokenizer for exact local token counting to stay inside context windows and forecast cost.
- [tiktoken-rs](https://github.com/zurawiki/tiktoken-rs) `* 405`: Rust tokenizer library for GPT/tiktoken token accounting.
- [tiktoken-cli](https://github.com/samber/tiktoken-cli) `* 10`: CLI to count tokens across files and directories using tiktoken.
- [ctxtrim](https://github.com/royalpinto007/Ctxtrim) `* 0`: Trims AI-context bloat: finds the files ballooning your Claude Code, Cursor, and Codex token cost. npx ctxtrim.
- [tokencut](https://github.com/royalpinto007/tokencut) `* 0`: Measure and cut the token cost of an LLM or agent message payload before you send it: truncate bloated tool results, drop duplicate context, trim to a budget. npx tokencut.

### Context-window profiling

- [ccusage](https://github.com/ryoppippi/ccusage) `* 17.6k`: CLI that parses Claude Code's local JSONL logs to report token usage and cost by day, session, and project.
- [claude-code-usage-analyzer](https://github.com/aarora79/claude-code-usage-analyzer) `* 4`: Analyzes Claude Code usage data to profile token and context consumption.
- [ctxlens](https://github.com/royalpinto007/Ctxlens) `* 0`: Context-window profiler for agents: shows what is eating your tokens and where to cut.

### Frameworks

- [DSPy (Stanford NLP)](https://github.com/stanfordnlp/dspy) `* 36.5k`: Framework for programming (not prompting) LLMs, with typed signatures, modules, and automated prompt/context optimization.
- [FastMCP](https://github.com/jlowin/fastmcp) `* 27k`: Ergonomic Python framework for building MCP servers that feed context to agents.
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) `* 23.8k`: Official MCP SDK for exposing tools and resources that agents select into context.

### Multi-agent and isolation

- [MetaGPT](https://github.com/FoundationAgents/MetaGPT) `* 69.6k`: Multi-agent framework that isolates context across role-specialized agents.
- [CAMEL](https://github.com/camel-ai/camel) `* 17.5k`: Framework for multi-agent systems with isolated per-agent contexts.

### Evals for context quality

- [DeepEval](https://github.com/confident-ai/deepeval) `* 17.3k`: Pytest-style LLM eval framework with debuggable RAG and context metrics for CI/CD.
- [Ragas](https://github.com/explodinggradients/ragas) `* 15.1k`: Reference-free RAG evaluation with metrics for context relevance and precision, plus synthetic test-set generation.

### Long-context and context rot

- [RULER (NVIDIA)](https://github.com/NVIDIA/RULER) `* 1.6k`: Synthetic benchmark measuring the real usable context size across retrieval, multi-hop, aggregation, and QA.
- [Context Rot toolkit (Chroma)](https://github.com/chroma-core/context-rot) `* 291`: Toolkit reproducing Chroma's finding that LLM reliability degrades non-uniformly as input tokens grow, across 18 models.
- [NoLiMa (Adobe Research)](https://github.com/adobe-research/NoLiMa) `* 202`: Long-context benchmark beyond literal matching; needle and question share minimal lexical overlap, exposing steep degradation with length.
- [Context Rot report (Chroma)](https://research.trychroma.com/context-rot): Technical report showing performance drops with longer inputs, worse when needle and question are semantically similar.

### Papers

- [NoLiMa paper](https://arxiv.org/abs/2502.05167): ICML paper: even GPT-4o falls from 99.3% to 69.7% as context length grows on association-based retrieval.
- [LLMLingua-2 paper](https://arxiv.org/abs/2403.12968): Task-agnostic prompt compression via token classification distilled from GPT-4, 3-6x faster.
- [The Complexity Trap](https://arxiv.org/html/2508.21433v1): Finds simple observation masking is as efficient as LLM summarization for agent context management.

<!-- LIST:END -->

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Edit `data/tools.json`, run `node scripts/generate.mjs`, open a PR.

## License

[CC0 1.0](LICENSE) (public domain).
