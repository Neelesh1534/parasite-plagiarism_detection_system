# 🤖 PARASITE: Master Architecture & AI File Prompts Collection

This document contains **ready-to-use production prompts** for every component and documentation file in the **PARASITE Compiler Plagiarism Detection System**. You can copy-paste these prompts into any LLM or AI coding assistant to recreate, expand, or explain any file in the repository.

---

## 1. Master System Generation Prompt (The Complete Project)

```text
Act as a Senior Principal Compiler Engineer and Technical Architect.
Build a full-stack Web Application named "PARASITE" (Compiler-Based Structural Code Plagiarism Detection System) using React 18, TypeScript, Vite, Tailwind CSS, Recharts, Lucide Icons, and jsPDF.

Key Architecture Requirements:
1. Core Compiler Pipeline:
   - Lexical Analysis & Scope Token Normalization (maps variable names to VAR, numbers to NUM).
   - Symbol Table & Scope Depth Resolver.
   - AST Generation & Bottom-Up Subtree Hashing (invariant to identifier renaming).
   - Control Flow Analysis (CFG: extracts IF, LOOP, SWITCH, CALL decision graphs).
   - Data Flow Analysis (Def-Use variable dependency tracking).
   - Sliding N-Gram Winnowing (2, 3, 4-grams) and LCS (Longest Common Subsequence) Line Matching.
   - Weighted Scoring Model: Combined = 35% AST + 25% Token + 20% CFG + 10% DataFlow + 10% LCS.
2. Performance & Concurrency:
   - Non-blocking Web Worker (plagiarism.worker.ts) offloading O(N^2) pairwise matrix checks.
3. Dashboard UI & Analytics:
   - Landing Page, Multi-File Drag-and-Drop Upload Page, Animated Processing Pipeline, Results Page (Similarity Matrix, Pairwise Diff), and Interactive Data Visualization Page (Radar Charts, Network Graphs, PDF Exporter).
```

---

## 2. File-by-File Technical Prompts

### ⚙️ Core Analysis Engine (`src/lib/plagiarism.ts`)
```text
Write a production TypeScript module named `plagiarism.ts` for static program analysis:
- Define Interfaces: Token, ComparisonResult, MultiComparisonResult, ASTNode, ASTComparisonResult, PlagiarismType, EnhancedComparisonResult.
- Preprocessing: Strip single-line (//) and multi-line (/* */) comments, preprocessor directives (#include, import), normalize whitespace, and map literals to 'NUM' or 'STR'.
- Lexer (`tokenizeCode`): Tokenize multi-language code (C, C++, Java, Python, PHP, SQL) into type, value, normalized form, and line numbers.
- N-Gram Matching (`compareTokens`, `compareTokensWeighted`): Sliding 2, 3, and 4-grams with weighted Jaccard similarity.
- AST Normalizer & Matcher (`compareAST`): Parse scope blocks, recursively compute bottom-up child-sorted structural hashes, index subtrees in a HashMap, and classify plagiarism (Verbatim, Renamed, Rearranged, Structural).
- Control Flow & Data Flow Evaluators: Extract structural flow sequences and variable Def-Use scope depths.
- Pipeline Orchestrators (`analyzeCodeEnhanced`, `analyzeMultipleEnhanced`): Compute weighted similarity matrix and pairwise metrics.
```

---

### 🛡️ Extended Boilerplate Whitelisting & Pipeline Extensions (`src/lib/parasite-extensions.ts`)
```text
Write a TypeScript extension module `parasite-extensions.ts` for PARASITE:
- Input Validation (`validateInput`): Validate file extensions (.c, .cpp, .java, .py, .js, .php, .sql), file size (<1MB), and UTF-8 encoding.
- Whitelist Filter (`filterWhitelistedCode`): Strip boilerplate header directives (#include <stdio.h>, import java.util.*, package), author comments, licensing, and assignment headers before analysis to prevent false positives.
- Network Graph Generator (`generateNetworkGraph`): Convert multi-file similarity matrix into node-edge graph structures for visualization.
```

---

### ⚡ Parallel Web Worker (`src/workers/plagiarism.worker.ts`)
```text
Write a dedicated Web Worker script `plagiarism.worker.ts` in TypeScript:
- Handle message events (`self.onmessage`) for commands: 'analyze', 'analyzeMultiple', 'validateFiles', 'generateGraph'.
- Run CPU-intensive tokenization, AST hashing, CFG extraction, and matrix computation asynchronously without blocking the main browser thread.
- Send real-time progress events (`self.postMessage`) with percentage completion (0% to 100%) and current pair processing status.
```

---

### 📄 PDF Report Generator (`src/lib/pdfReport.ts`)
```text
Write a PDF export module `pdfReport.ts` using `jsPDF`:
- Function `exportPlagiarismReport(result, fileNames)`: Generate a multi-page PDF document.
- Page Design: Blue header banners, executive summary card, overall similarity percentage badge, AST/Token/CFG metric breakdown table, matched code line snippets, and auto-numbered footer (`Page X of Y`).
```

---

### 🎨 Visual Analytics Dashboard Page (`src/pages/VisualizationPage.tsx`)
```text
Build a React TypeScript page `VisualizationPage.tsx` using Tailwind CSS and Recharts:
- Interactive Multi-View Dashboard:
  - Similarity Matrix Heatmap: Interactive grid of file pairs colored by risk level (Green = Low, Yellow = Medium, Red = High).
  - Network Topology Graph: Canvas/SVG representation connecting plagiarized file nodes.
  - Multi-Metric Radar Chart: Polygon chart displaying AST, Token, CFG, Data Flow, and LCS scores side-by-side.
  - Code Diff Viewer: Parallel side-by-side code match view with line-number highlighting.
- Action Toolbar: Download PDF report button, filter threshold sliders, and tab navigation.
```

---

### 🚀 Processing Pipeline Screen (`src/pages/ProcessingPage.tsx`)
```text
Create a React TypeScript component `ProcessingPage.tsx`:
- Animated Pipeline Progress UI:
  - Step progress bar displaying 10 analysis stages (Validation -> Preprocessing -> Lexing -> Symbol Table -> AST Hashing -> CFG -> Data Flow -> N-Grams -> Scoring -> Rendering).
  - Pulsing terminal log viewer outputting active processing logs in real-time.
  - Seamless navigation to Results/Visualization page upon 100% Web Worker completion.
```

---

### 📥 Upload Interface Page (`src/pages/UploadPage.tsx`)
```text
Create a React component `UploadPage.tsx` for multi-file code submission:
- Drag-and-Drop Dropzone: Supports single-file and multi-file drag-and-drop (.c, .cpp, .java, .py, .js, .php, .sql).
- Preset Sample Code Selector: Load pre-built benchmark test files (Factorial, Fibonacci, Bubble Sort) for instant demonstration.
- Language selector dropdown and active file list with file size badges and delete buttons.
```

---

### 📖 Master Technical Presentation & Interview Guide (`docs/PARASITE_PRESENTATION_AND_INTERVIEW_GUIDE.md`)
```text
Write an exhaustive markdown documentation artifact `PARASITE_PRESENTATION_AND_INTERVIEW_GUIDE.md`:
- Slide Deck Outline: 8-slide PowerPoint presentation outline detailing Title, Problem Statement, DFD Diagrams, Compiler Concepts, 10-Stage Pipeline, Scoring Math, Benchmarks, and Future Roadmap.
- Level 1 & Level 2 Data Flow Diagrams (DFD) in ASCII & Mermaid.
- Comprehensive Technical Defense Q&A: Detailed answers for 15+ complex professor/interview questions covering AST hashing, CFG, LCS $O(m \times n)$ complexity, N-gram Winnowing, variable renaming zero-impact proof, and Locality-Sensitive Hashing (LSH) scalability.
```

---

### 📘 Executive Readme (`README.md`)
```text
Write a professional capstone project README `README.md` for PARASITE:
- Badges: Build Status, License (MIT), TypeScript, React 18.
- Comprehensive Overview, ASCII Front-End Compiler Architecture Diagram, Lexical/AST/CFG breakdown.
- Mathematical Scoring Formula rendered in LaTeX:
  Combined Score = 0.35 * AST + 0.25 * Token + 0.20 * CFG + 0.10 * DataFlow + 0.10 * LCS.
- Directory Structure, Installation & Development Execution Guide, and Author Capstone Attestation.
```
