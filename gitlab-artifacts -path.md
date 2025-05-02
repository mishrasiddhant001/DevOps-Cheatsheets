
# GitLab CI/CD Artifact Path Patterns

This document outlines valid ways to define file and directory patterns for `artifacts: paths:` in GitLab CI/CD pipelines.

## ✅ Valid Patterns

### 1. Single File
```yaml
artifacts:
  paths:
    - path/to/file.txt
```

### 2. All Files in a Directory
```yaml
artifacts:
  paths:
    - build/
    - dist/
```

### 3. All Files at Root Level
```yaml
artifacts:
  paths:
    - "*"
```

### 4. All Files Recursively
```yaml
artifacts:
  paths:
    - "**/*"
```

### 5. Specific File Types Recursively
```yaml
artifacts:
  paths:
    - "**/*.html"
    - "**/*.js"
    - "**/*.css"
```

### 6. Files in a Specific Folder
```yaml
artifacts:
  paths:
    - public/**/*.html     # All .html in public and subfolders
    - logs/*.log           # All .log files in logs folder (not recursive)
```

### 7. Files Matching Prefix or Suffix
```yaml
artifacts:
  paths:
    - "**/report-*.pdf"    # Any PDF file starting with "report-"
```

---

| Pattern           | What it matches                                                   | Notes                                                                                       |
|-------------------|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| `qa-release/*`    | files & folders directly under `qa-release/` (one level)           | Does not recurse into sub‑folders                                                            |
| `qa-release/**/*` | all files under `qa-release/` at any depth                         | • `**` (globstar) lets you recurse into sub‑dirs<br>• the trailing `/*` means “then one more path‑segment” (so it won’t match the root itself) |
| `qa-release/**`   | all files & folders under `qa-release/` recursively (including empty dirs and the `qa-release/` folder itself) | • `**` by itself matches zero or more path segments, including “nothing”                      |


---

## ✅ Recommended Example for Frontend Project

```yaml
artifacts:
  paths:
    - public/
    - "**/*.html"
    - "**/*.js"
    - "**/*.css"
```

This ensures that all relevant frontend build outputs are included as artifacts.

---

## Notes

- `*` matches anything at one directory level
- `**/*` matches anything recursively
- Always test patterns if unsure — incorrect patterns can result in missing artifacts
