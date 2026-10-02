# 🧬 PARASITE
> **Compiler-Based Structural Code Plagiarism Detection System**

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)]()
[![License](https://img.shields.io/badge/license-MIT-blue.svg)]()
[![TypeScript](https://img.shields.io/badge/language-TypeScript-3178C6.svg)]()
[![React](https://img.shields.io/badge/framework-React_18-61DAFB.svg)]()

---

## 🔍 Overview

**PARASITE** is a high-performance, compiler-inspired static analysis system engineered to detect **structural plagiarism in source code**. 

Unlike standard text-matching tools (like `diff` or raw string comparison), PARASITE parses source code into **intermediate representations (IR)**, extracting structural features, lexical token patterns, **Abstract Syntax Tree (AST)** hierarchies, **Control Flow Graphs (CFG)**, and **Data Flow dependencies**.

It reliably detects similarity even when code has undergone common obfuscation techniques:
* 🔹 **Identifier Renaming**: Replacing variable, function, and parameter names.
* 🔹 **Code Formatting**: Indentation shifts, line splits, whitespace variation, and comment removal.
* 🔹 **Reordering**: Rearranging independent statements, block definitions, or loop constructs.
* 🔹 **Type Aliasing**: Using typedefs, type synonyms, or wrapper variables.

---

## 🧠 Compiler Design Concepts Implemented

PARASITE mirrors the front-end and middle-end of a production compiler pipeline:

```
+------------------+     +--------------------+     +---------------------+
|   Source Code    | --> |  Lexical Analyzer  | --> | Normalized Tokens   |
+------------------+     +--------------------+     +---------------------+
                                                               |
                                                               v
+------------------+     +--------------------+     +---------------------+
| Similarity Score | <-- | Static Evaluator   | <-- | AST & CFG Generator |
+------------------+     +--------------------+     +---------------------+
```

1. **Lexical Analysis (Tokenization & Normalization)**
   - Strips comments, preprocessor directives (`#include`, `import`), and layout formatting.
   - Converts identifiers into scope-aware tokens (`VAR`), numeric literals into `NUM`, and standardizes operators (`OP_ASSIGN`, `OP_EQ`).

2. **Symbol Table & Scope Resolution**
   - Maps variable declarations to scope depth, eliminating identifier name bias.

3. **Abstract Syntax Tree (AST) Hashing**
   - Builds hierarchical syntax tree representations.
   - Computes bottom-up structural hashes invariant to variable renaming.

4. **Control Flow Graph (CFG) Analysis**
   - Extracts program flow structures (`IF`, `LOOP`, `SWITCH`, `RETURN`, `CALL`).
   - Analyzes cyclomatic complexity, branch density, and nesting depth.

5. **Data Flow Analysis**
   - Tracks variable definition-use (Def-Use) chains and access depth.

6. **Fingerprinting & N-Gram Winnowing**
   - Computes sliding token N-grams (2-gram, 3-gram, 4-gram) with weighted Jaccard similarity.

---

## ⚙️ Weighted Scoring Model

PARASITE combines multiple structural metrics into a unified similarity score:

$$\text{Combined Score} = 0.35 \times S_{\text{AST}} + 0.25 \times S_{\text{Token}} + 0.20 \times S_{\text{CFG}} + 0.10 \times S_{\text{DataFlow}} + 0.10 \times S_{\text{LCS}}$$

Where:
- $S_{\text{AST}}$: AST structural subtree match rate.
- $S_{\text{Token}}$: Weighted N-gram token sequence similarity.
- $S_{\text{CFG}}$: Control flow structural similarity.
- $S_{\text{DataFlow}}$: Definition-use scope depth correlation.
- $S_{\text{LCS}}$: Longest Common Subsequence line-level match.

---

## ⚡ Tech Stack & Performance Architecture

* **UI & Dashboard**: React 18, TypeScript, Tailwind CSS, Recharts (Radar/Matrix charts), Lucide Icons.
* **Analysis Engine**: Modular TypeScript compiler pipeline (`plagiarism.ts`, `parasite-extensions.ts`).
* **Multi-threading**: Dedicated **Web Workers** (`plagiarism.worker.ts`) offloading $O(N^2)$ pairwise matrix computations from the main UI thread.
* **Export Engine**: Integrated PDF generation via `jsPDF`.
* **Build System**: Vite.

---

## 📁 Project Directory Structure

```
parasite-compiler/
 ├── src/
 │    ├── components/         # Modular React UI components (Navbar, NavLink, etc.)
 │    ├── pages/              # Application views (Upload, Processing, Results, Visualization)
 │    ├── lib/
 │    │    ├── plagiarism.ts           # Core Lexer, Tokenizer, AST & Scoring engine
 │    │    ├── parasite-extensions.ts  # Boilerplate Whitelisting & CFG/DataFlow modules
 │    │    └── pdfReport.ts            # PDF report exporter
 │    ├── workers/
 │    │    └── plagiarism.worker.ts    # Web Worker parallel processing pipeline
 │    └── main.tsx            # Application Entry point
 ├── index.html               # Main HTML entry
 ├── package.json             # Dependencies & build scripts
 └── vite.config.ts           # Vite configuration
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18.0.0 or higher)
- npm (v9.0.0 or higher)

### Installation & Execution

```bash
# 1. Install project dependencies
npm install

# 2. Start local development server
npm run dev

# 3. Build for production compilation
npm run build
```

---

## 🎯 Future Technical Roadmap

- [ ] **Tree-Sitter Parsing Integration**: Native Wasm bindings for full CST multi-language parsing.
- [ ] **Program Dependence Graph (PDG)**: Graph isomorphism via Weisfeiler-Lehman kernels for statement-swap invariance.
- [ ] **Rabin-Karp Rolling Hashes**: Linear-time $O(N)$ MOSS-style winnowing.
- [ ] **LLVM IR Analysis**: Bytecode compilation pass before comparison to bypass surface syntax completely.

---

## 👤 Author & Architecture

**Designed & Developed as a Compiler Engineering Capstone Project.**
Demonstrating practical application of lexical analysis, syntax trees, static analysis, and multi-threaded Web Workers.
