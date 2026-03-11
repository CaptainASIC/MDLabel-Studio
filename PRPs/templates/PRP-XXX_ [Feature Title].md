# PRP-XXX: [Feature Title]

**Objective:** This Product Requirements Prompt (PRP) provides a complete, step-by-step instruction set for an AI agent (Warp) to implement the `[Feature Title]` feature. Execute all steps in the specified order.

**Status:** Ready for execution
**Author:** Manus
**Date:** YYYY-MM-DD
**Depends on:** [PRP-XXX (Dependency), ...]

---

## 1. Objective & Scope

**Goal:** Implement the `[Feature Title]` feature as described below.

**Summary of changes:**

| Area | Files | Action |
|------|-------|--------|
| Configuration | `config.py`, `.env.example` | [Brief description of changes] |
| Dependencies | `pyproject.toml` | [Brief description of changes] |
| New Feature | `[new_file].py` | **CREATE** — [Brief description] |
| Modified Feature | `[modified_file].py` | **MODIFY** — [Brief description] |
| Tests | `test_[feature].py` | **CREATE** — [Brief description] |

**Total new files:** X
**Total modified files:** Y

---

## 2. Implementation Instructions

### Execution Order

**Execute these steps sequentially:**

1.  **Step 2.1** — Create `new_file_1.py`
2.  **Step 2.2** — Modify `existing_file_1.py`
3.  **Step 2.3** — Create `new_file_2.py`
4.  **Step 3.1** — Create `test_new_feature.py`
5.  **Step 4.1** — Execute tests

---

### 2.1 New File: `path/to/new_file_1.py`

**Action:** CREATE

**Instructions:** Create the file `path/to/new_file_1.py` with the following content. Do not add or remove any lines.

```python
"""Docstring for the new file."""

# ... full file content ...
```

---

### 2.2 Modify: `path/to/existing_file_1.py`

**Action:** MODIFY

**Instructions:** Apply the following diff to the file `path/to/existing_file_1.py`.

```diff
- old_line_of_code()
+ new_line_of_code()
```

---

## 3. Test Instructions

### 3.1 New File: `path/to/test_new_feature.py`

**Action:** CREATE

**Instructions:** Create the test file `path/to/test_new_feature.py` with the following content.

```python
"""Tests for the new feature."""

import pytest

# ... full test file content ...
```

---

## 4. Validation Instructions

### 4.1 Execute Tests

**Action:** EXECUTE

**Instructions:** Run the following command from the `backend/` directory.

```bash
cd backend && uv run pytest tests/path/to/test_new_feature.py -v
```

**Expected Outcome:** All tests must pass. If any test fails, stop execution and report the failure.

### 4.2 Acceptance Criteria

**Action:** VERIFY

**Instructions:** Confirm that all of the following criteria are met. All criteria are verified by the test suite.

| # | Criterion | Verification |
|---|-----------|-------------|
| 1 | [Description of criterion] | `test_name` |
| 2 | ... | ... |

---

## 5. Configuration (Optional)

### 5.1 Modify: `.env.example`

**Action:** MODIFY

**Instructions:** Apply the following diff to `.env.example`.

```diff
 # New Feature
+NEW_FEATURE_ENABLED=false
```

### 5.2 Modify: `config.py`

**Action:** MODIFY

**Instructions:** Apply the following diff to `backend/app/core/config.py`.

```diff
 class AiriSettings(BaseSettings):
+    new_feature_enabled: bool = Field(default=False)
```

---

## 6. Execution Summary

| Step | Action | File | Lines Changed |
|------|--------|------|--------------|
| 1 | CREATE | `path/to/new_file_1.py` | ~XXX new |
| 2 | MODIFY | `path/to/existing_file_1.py` | +X, -Y |
| 3 | CREATE | `path/to/test_new_feature.py` | ~XXX new |

**Total new code:** ~XXX lines
**Total modifications:** ~Y lines
**Total tests:** Z

---

*End of PRP-XXX*
