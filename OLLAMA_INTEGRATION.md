# Local Deep Researcher - Ollama Integration

**Purpose:** Deep research workflows using local Ollama models with RAG integration.

**Last Updated:** 2026-04-02

---

## 🎯 Overview

This repo provides deep research capabilities using **local Ollama models** with integration into the **private-agent-library RAG** system.

---

## 🔧 Setup

### Prerequisites

1. **Ollama installed:**
   ```bash
   curl -fsSL https://ollama.com/install.sh | sh
   ```

2. **Pull required models:**
   ```bash
   ollama pull qwen2.5:7b
   ollama pull qwen2.5:14b
   ollama pull nomic-embed-text
   ```

3. **Install dependencies:**
   ```bash
   cd local-deep-researcher
   uv sync
   ```

---

## 🚀 Quickstart

### Basic Research Query

```bash
cd local-deep-researcher

python3 src/ollama_deep_researcher/research.py \
  --query "What are best practices for skill design?"
```

### With RAG Integration

```bash
python3 src/ollama_deep_researcher/research.py \
  --query "How to improve skill quality?" \
  --rag-path /home/sdigits/Documents/Git/private-agent-library/skill-rlm \
  --top-k 5
```

### Multi-Step Research

```bash
python3 src/ollama_deep_researcher/research.py \
  --query "Compare model quantization techniques" \
  --steps 3 \
  --model qwen2.5:14b
```

---

## 📊 Output Formats

### Default (Markdown Report)

```markdown
# Research Report: [Query]

## Executive Summary
[Brief summary]

## Key Findings
1. [Finding 1]
2. [Finding 2]

## Sources
- [Source 1]
- [Source 2]

## Recommendations
[Actionable recommendations]
```

### JSON Export

```bash
python3 src/ollama_deep_researcher/research.py \
  --query "..." \
  --output-format json \
  --output-file research_output.json
```

---

## 🔗 RAG Integration

### Supported RAG Sources

| Source | Path | Type |
|--------|------|------|
| Private Agent Library | `private-agent-library/skill-rlm` | Skills + Papers |
| Papers | `private-agent-library/papers` | Research Papers |
| Docs | `private-agent-library/docs` | Documentation |

### RAG Query Enhancement

```python
from ollama_deep_researcher.rag import RAGEnhancer

enhancer = RAGEnhancer(
    rag_path="/home/sdigits/Documents/Git/private-agent-library/skill-rlm",
    top_k=5
)

# Enhance query with RAG context
enhanced_query = enhancer.enhance("How to improve skill quality?")
print(enhanced_query)
```

---

## 🎛️ Configuration

### Model Selection

| Model | VRAM | Use Case |
|-------|------|----------|
| qwen2.5:3b | 2GB | Quick queries, simple research |
| qwen2.5:7b | 4GB | General research, good balance |
| qwen2.5:14b | 8GB | Complex research, deep analysis |

### Research Depth

| Depth | Steps | Time | Use Case |
|-------|-------|------|----------|
| Shallow | 1-2 | <1 min | Quick answers |
| Medium | 3-5 | 2-5 min | Standard research |
| Deep | 6-10 | 5-15 min | Comprehensive analysis |

---

## 📋 Example Workflows

### Workflow 1: Skill Quality Research

```bash
# Research skill quality best practices
python3 src/ollama_deep_researcher/research.py \
  --query "What makes a skill production-ready?" \
  --rag-path /home/sdigits/Documents/Git/private-agent-library \
  --model qwen2.5:7b \
  --steps 3 \
  --output-format markdown \
  --output-file skill_quality_report.md
```

### Workflow 2: Security Best Practices

```bash
# Research security scanning techniques
python3 src/ollama_deep_researcher/research.py \
  --query "Automated security scanning best practices" \
  --steps 5 \
  --model qwen2.5:14b \
  --output-format json \
  --output-file security_research.json
```

### Workflow 3: Model Optimization

```bash
# Research model optimization techniques
python3 src/ollama_deep_researcher/research.py \
  --query "KV cache optimization techniques for local LLMs" \
  --rag-path /home/sdigits/Documents/Git/private-agent-library/papers \
  --model qwen2.5:14b \
  --steps 5 \
  --output-format markdown
```

---

## 🔗 Integration with Agent-Assistant (Mika)

### Run Research via Mika

```bash
cd agent-assistant

python3 skills/mika-runner/scripts/run.py \
  --name "deep-research" \
  --out-root /tmp/mika_research \
  -- \
  python3 ../local-deep-researcher/src/ollama_deep_researcher/research.py \
    --query "Skill design patterns" \
    --output-format markdown
```

### Research + Audit Workflow

```bash
# 1. Run research
python3 src/ollama_deep_researcher/research.py \
  --query "Disk cleanup best practices" \
  --output-file cleanup_research.md

# 2. Run Mika disk audit
python3 skills/mika-disk-audit/scripts/audit_data.py \
  --root /home/sdigits/Documents/Git

# 3. Apply research findings
# (Manual review and action)
```

---

## 📊 Performance Benchmarks

| Query Type | Model | Steps | Time | Tokens |
|------------|-------|-------|------|--------|
| Simple Q&A | qwen2.5:3b | 1 | 30s | 500 |
| Standard Research | qwen2.5:7b | 3 | 2 min | 2K |
| Deep Analysis | qwen2.5:14b | 5 | 5 min | 5K |
| Comprehensive | qwen2.5:14b | 10 | 15 min | 10K |

---

## 🛠️ Troubleshooting

### Ollama Not Running

```bash
# Start Ollama
ollama serve

# Or as background service
brew services start ollama  # macOS
systemctl start ollama      # Linux
```

### Model Not Found

```bash
# Pull missing model
ollama pull qwen2.5:7b
```

### RAG Path Not Found

```bash
# Verify RAG path exists
ls -la /home/sdigits/Documents/Git/private-agent-library/skill-rlm

# Update path if needed
python3 src/ollama_deep_researcher/research.py \
  --query "..." \
  --rag-path /correct/path/to/rag
```

---

## 📝 Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-02 | Initial Ollama integration guide |
