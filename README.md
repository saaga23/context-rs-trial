# context-rs: The Intelligent Context Agent

**context-rs** is a compiler-aware CLI tool that automatically generates *perfect context* for Large Language Models (LLMs).

Unlike traditional RAG tools that rely on probabilistic vector search and often miss critical files, **context-rs** uses **Abstract Syntax Tree (AST) parsing** to deterministically resolve your code’s full dependency graph — exactly how the compiler sees it.

---

## The Problem

You upload `main.rs` to an LLM to fix a bug.

The fix fails — not because the model is weak, but because it does not know about:
- `scanner.rs`
- `utils.rs`
- deeply nested modules pulled in through `mod` or `use`

Missing context leads to hallucinations, incorrect assumptions, and broken suggestions.

---

## The Solution

**context-rs** detects what you are actively working on using Git, recursively walks the compiler dependency tree, and packages **every required file** into a **token-optimized XML payload** designed for senior-level LLM reasoning.

The model receives *exactly* the code it needs — no more, no less.

---

## Key Features

### 1. Deep Dependency Resolution (AST-Based)

Most tools stop at the file level.  
**context-rs** understands Rust code structure using AST parsing.

- **Recursive Scanning**  
  If `main.rs` imports `scanner.rs`, and `scanner.rs` imports `utils.rs`, the entire chain is discovered automatically.

- **Deterministic Accuracy**  
  No guessing. No embeddings. No missed modules.

- **Hallucination Reduction**  
  The AI sees the real definitions it depends on, dramatically reducing incorrect suggestions.

---

### 2. Optimization Dashboard

Running the tool generates an interactive HTML report (`report.html`) that visualizes your context:

- **Interactive Dependency Graph**  
  See how files connect (visualized with Mermaid.js).

- **Token & Cost Audit**  
  Exact token count and estimated GPT-4o cost *before* you paste.

- **Logic Trace**  
  A transparent explanation of why each file was included.

---

### 3. Safety-First XML Payload

The generated XML payload is structured for high-quality LLM reasoning and includes a system protocol that instructs the AI to:

- Treat **User-Modified** files as the primary target
- Treat **Auto-Resolved** dependencies as read-only context
- Suggest changes to dependencies **only when strictly necessary**

This prevents accidental refactors and unsafe recommendations.

---

## Installation

```bash
git clone https://github.com/yourusername/context-rs
cd context-rs
cargo build --release
```

---

## Usage

```bash
cargo run -- --smart
```

---

## How It Works

### Phase 1: Context Detection (Git)
Identifies the exact files the developer is modifying.

### Phase 2: Recursive AST Walking
Uses the `syn` crate to parse Rust code and resolve all dependencies recursively.

### Phase 3: Token Optimization
Packs code into CDATA-protected XML while excluding unnecessary files.

---

## Why Use context-rs?

| Feature | Standard RAG | context-rs |
|------|-------------|------------|
| Accuracy | Probabilistic | Deterministic |
| Dependency Depth | Partial | Complete |
| Cost Visibility | Unknown | Pre-audited |
| Output | Raw text | Structured XML |

---

Built with care in Rust.
