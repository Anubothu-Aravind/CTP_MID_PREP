# Computational Thinking & Programming - Midterm Preparation (CTP_MID_PREP)

This repository contains organized midterm examination preparation materials, solution modules, benchmarking notebooks, and architectural standards for the **M.Tech Algorithmic Programming / Computational Thinking and Programming (CTP)** curriculum.

---

## Repository Structure

```text
CTP_MID_PREP/
├── README.md                                   # Global Repository Documentation
├── RULES.md                                    # Coding & Documentation Architectural Standards
├── .gitignore                                  # Git ignore rules for Python, Jupyter & OS metadata
├── CO1/                                        # Course Outcome 1: Algorithmic Foundations
│   ├── C01.pdf                                 # CO1 Midterm Question Bank
│   ├── readme.md                               # CO1 Overview & Problem Index
│   ├── a_ComputationalThinking/               # Decomposition, Abstraction & Pattern Recognition
│   ├── b_AlgorithmDesignAndComplexity/        # Search Paradigms, Hashing & Complexity Analysis
│   │   └── b2_SocialMediaFollowers/
│   │       ├── b2_SocialMediaFollowers.ipynb  # Interactive Benchmarking Notebook
│   │       └── readme.md                      # Detailed Exam Documentation & Manual Traces
│   ├── c_DivideAndConquer/                    # Divide & Conquer Paradigms
│   ├── d_DynamicProgramming/                  # Dynamic Programming & Optimization
│   ├── e_GreedyAlgorithms/                    # Greedy Choice Strategies
│   └── f_Backtracking/                        # Constraint Satisfaction & Traversal
├── CO2/                                        # Course Outcome 2: Advanced Data Structures
│   └── CO2.pdf                                 # CO2 Midterm Question Bank
└── CO3/                                        # Course Outcome 3: Graph Algorithms & Optimization
    └── CO3.pdf                                 # CO3 Midterm Question Bank
```

---

## Solved Modules Index

### Course Outcome 1 (CO1)
| Section | Topic | Problem Statement | Paradigm | Key Complexity | Artifacts |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Section B** | **b.2 Social Media Followers** | Compare Linear Search and Separate Chaining Hashing for username validation ($n = 500,000$). | Searching & Hashing | Linear: $O(n)$<br>Hash: $O(1)$ expected | [Notebook](./CO1/b_AlgorithmDesignAndComplexity/b2_SocialMediaFollowers/b2_SocialMediaFollowers.ipynb) \| [Readme](./CO1/b_AlgorithmDesignAndComplexity/b2_SocialMediaFollowers/readme.md) |

---

## Coding & Architectural Standards

All implementations and documentation in this repository adhere strictly to [RULES.md](./RULES.md):
- **Clean Notebooks**: Zero inline comments or docstrings in `.ipynb` code cells; self-explanatory identifiers.
- **Two-Cell Scalability Benchmarks**:
  - *Cell 1 (Setup)*: Data structure construction ($O(1)$ insertion, load factor $\alpha \approx 0.75$).
  - *Cell 2 (Timing)*: Isolate query lookup execution time using `time.perf_counter()`.
- **Standardized Terminal Output**: Clean ASCII table borders (`ALGORITHM`, `RESULT`/`TIME`, `OPERATIONS`) without emojis.

---

## Setup & Usage

### Prerequisites
- Python 3.10+
- Jupyter Notebook / JupyterLab

### Running Notebooks
1. Clone the repository:
   ```bash
   git clone https://github.com/Anubothu-Aravind/CTP_MID_PREP.git
   cd CTP_MID_PREP
   ```
2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
3. Navigate to any solution module (e.g., `CO1/b_AlgorithmDesignAndComplexity/b2_SocialMediaFollowers/b2_SocialMediaFollowers.ipynb`).
