# MID_PREP - Coding & Documentation Standards

This document establishes the mandatory standards for code implementation, notebook architecture, documentation structure, and repository management across all course outcome modules under `MID_PREP`.

---

## 1. Code Cleanliness Standards

* **Zero Comments & Docstrings**: Code cells inside `.ipynb` notebooks must be completely free of inline comments (`#`), block comments, and docstrings (`"""..."""` or `'''...'''`).
* **Self-Explanatory Identifiers**: Function names, variable names, and class attributes must be clean and self-descriptive.
* **Pure Execution Focus**: The notebook cells are designed for concise algorithmic demonstration during midterm examination evaluation.

---

## 2. Notebook Architecture for Benchmarking

When implementing benchmark experiments comparing algorithmic search or sorting paradigms:

* **Cell 1: Data Structure Pre-processing & Setup (Run Once)**:
  * Dataset creation and table/structure construction must be performed in a dedicated setup cell.
  * Load factors for hash tables must be calibrated ($\alpha = \frac{n}{m} \approx 0.75$) with $O(1)$ constant-time insertion to ensure setup finishes within milliseconds.
* **Cell 2: Query Execution & Performance Timing (Run Multiple Times)**:
  * Isolate query search execution time from dataset construction time.
  * Use `time.perf_counter()` to measure strictly the algorithm lookup duration.
  * Allows re-running different target queries instantaneously without re-triggering dataset construction loops.

---

## 3. Output Formatting & Aesthetic Rules

* **Text-Based Status Labels**: No emojis in documentation or terminal output. Use explicit text markers (e.g., `FOUND`, `NOT FOUND`, `[Chosen]`, `[Not Chosen]`).
* **Standardized Table Banners**:
  * Output print statements must follow a standardized table layout using ASCII borders (`=` and `-`).
  * Explicit column headers: `ALGORITHM`, `RESULT` / `TIME`, and `OPERATIONS`.
* **Operations Tracking**: Explicitly count and display runtime operations (e.g., `comparisons` for Linear Search, `chain probes` for Hash Search).

---

## 4. Documentation Structure (`readme.md`)

Each module directory must contain an exam-ready `readme.md` following this structure:

1. **Problem Statement**: Practical real-world scenario (e.g., Social Media Username Verification).
2. **Algorithm Identification & Comparison Table**:
   * Definitions of compared approaches.
   * "Where It Will Be Helpful" criteria.
   * Types / Variants table highlighting chosen standard variants.
3. **Core Concepts & Definitions**:
   * Algorithmic principles, expected time/space complexities.
4. **Design Choices & Rationale**:
   * Detailed trade-off analysis explaining why specific variants were chosen for manual exam tracing vs code implementation.
5. **Step-by-Step Manual Trace**:
   * Step-by-step trace walkthroughs (e.g., ASCII summation modulo capacity) for exam preparation.
6. **Procedural Logic**:
   * Clear algorithmic step lists.
7. **Python Code Implementation**:
   * Clean implementation synchronized with notebook logic.
8. **Input and Output**:
   * Inputs and exact execution outputs matching live notebook runs.

---

## 5. Repository & Version Control Rules

* Maintain a dedicated `.gitignore` in `MID_PREP` ignoring `.ipynb_checkpoints/`, `__pycache__/`, virtual environments (`.venv/`), and OS metadata (`.DS_Store`).
* Ensure all notebook `.ipynb` files are saved with pre-computed cell outputs corresponding to the documented test cases.
