# 📘 GitLab CI/CD Pipeline Key Guide

This document explains all keys and subkeys used in your `.gitlab-ci.yml` file with clear descriptions and helpful emojis. 🧩

---

## 🖼️ `image`
```yaml
image:
  name: <image_name>
  pull_policy: if-not-present
````

* **name**: Docker image to be used in jobs (e.g., `maven:3.8-jdk-17`).
* **pull\_policy**: `if-not-present` avoids re-downloading the image if already available locally.

---

## ⚙️ `variables`

```yaml
variables:
  NEXUSURL: 
  ANSIBLEHOSTS: 
  SETTINGSXML: 
  MVNU: "-U"
  ANSIBLE_FORCE_COLOR: 'true'
  VAULT_SERVER_URL: 
  VAULT_AUTH_ROLE: ${CI_PROJECT_NAME}
  VAULT_AUTH_PATH: jwt2
```

* Global environment variables available to all jobs:

  * **NEXUSURL**: Nexus repo URL 📦
  * **ANSIBLEHOSTS**: Target Ansible hosts 🌐
  * **SETTINGSXML**: Custom Maven settings file 📄
  * **MVNU**: Maven option for updating snapshots 🔄
  * **ANSIBLE\_FORCE\_COLOR**: Enables colored Ansible output 🌈
  * **VAULT\_SERVER\_URL**: Vault service URL 🔐
  * **VAULT\_AUTH\_ROLE**: Auth role to access secrets 🔑
  * **VAULT\_AUTH\_PATH**: Auth method path for Vault 📍

---

## Cache 💾

Defines paths to cache between jobs to speed up builds.

```yaml
cache:
  paths:
    - $CI_PROJECT_DIR/.m2/repository
```

* **paths**: Specifies files or directories to cache (e.g., Maven repository to avoid downloading dependencies every time).

---


## 🔐 `.secrets`
Holds sensitive information like tokens and credentials, often fetched from a secure storage system like Vault.

```yaml
.secrets:
  variables:
    VAULT_SERVER_URL: 
    VAULT_AUTH_ROLE: 
    VAULT_AUTH_PATH:   
  id_tokens:
    VAULT_ID_TOKEN:
      aud:    
  secrets:
    BUILDCHANNEL:
      vault: 
      file: false
      token: 
    ...
```

* **variables**: Same as global but scoped to secret usage.
* **id\_tokens**: Defines how to authenticate with Vault using OIDC 🎫
* **secrets**: Pulls environment secrets from Vault:

  * **vault**: Path in Vault 🧩
  * **file**: Whether to write to file (false = env var) 📄
  * **token**: Vault token for authentication 🔐

---

## ⚙️ `default`

```yaml
default:
  tags:
    - 
```

* **tags**: Tells GitLab Runner which type of runner to use (e.g., `cloudexa`) 🏷️

---

## Include 📂

Includes other YAML files for reusable configurations or functions.

```yaml
include:
  - project: '${CICD_TEMPLATES}'
    file:
      - ''
      - ''
```

* **project**: The repository from which to include files.
* **file**: A list of YAML files to include in the pipeline.

---

## 📶 `stages`

```yaml
stages:
  - init
  - build
  - test
  - push-to-nexus
  - deploy
```
* **stages**: The different phases of the pipeline execution, such as `init`, `build`, `test`, `deploy`.


* Defines the order of execution for jobs ⛓️
---

## Job Examples 🧑‍💻

### `init-job` (Initialization) ⚡

```yaml
init-job:
  stage: init
  rules: 
    - if: 
  before_script:
    - !reference []
  script:
    - echo "variables imported"
  artifacts:
    reports:
      dotenv: 
    expire_in: 1 week
```

* **stage**: Belongs to the `init` stage 🛠️
* **rules**: Conditional logic to decide when job runs ✅
* **before\_script**: Reused script block via `!reference` 📌
* **script**: Commands to execute in the job terminal 🖥️
* **artifacts**: Files passed between jobs 📦

  * **reports.dotenv**: Exported variables
  * **expire\_in**: Lifetime of the artifacts ⏳

---

## 🧱 `.building`

```yaml
.building:
  extends: .secrets
  stage: build
  services:
    - name: docker:dind
      alias: gitlabrunner
  cache:
    paths:
      - $CI_PROJECT_DIR/.m2/repository
  needs:
    - job: init-job
      artifacts: true
  variables:
    DOCKER_HOST: 
    DOCKER_DRIVER: overlay2
    DOCKER_TLS_CERTDIR: ""
  before_script:
    - !reference []
  coverage: '/Total.*?([0-9]{1,3})%/'
  artifacts:
    paths:
      
    reports:
      dotenv: ${CI_PROJECT_DIR}/vars.env
      junit:

    expire_in: 1 week
```

* **extends**: Inherits settings from `.secrets` 🔁
* **services**: Starts Docker-in-Docker (dind) for container operations 🐳
* **needs**: Depends on another job's result 📥
* **coverage**: Regex to extract coverage % 📊

---

## 🧪 `test-job`

```yaml
test-job:
  extends: .building
  rules:
    - if: 
  script:
    
  coverage: 
```

* **rules**: Run only for merge requests 📥
* **script**: Runs tests and prints code coverage 📄✅

---

## 🏗️ `build-job`

```yaml
build-job:
  extends: .building
  rules:
    - if:
  script:
    - mvn clean package ...
    - zip and move artifacts
```

* **script**: Packages the app and prepares release files 📦🔧

---

## ✅ Summary

| 🔑 Key      | 📘 Purpose                                        |
| ----------- | ------------------------------------------------- |
| `image`     | Base container for jobs                           |
| `variables` | Environment config and secrets                    |
| `cache`     | Speeds up build by reusing dependencies           |
| `.secrets`  | Handles Vault-based secrets securely              |
| `default`   | Global settings like runner tags                  |
| `include`   | Reuses shared pipeline code from other repos      |
| `stages`    | Logical order of job execution                    |
| `init-job`  | Initializes and loads config                      |
| `.building` | Shared job config for building steps              |
| `test-job`  | Runs tests and shows coverage                     |
| `build-job` | Packages the app and outputs zipped release files |

---

**Differnce in Artifacts:paths and artifacts:reports**

| Feature              | `artifacts:paths`                | `artifacts:reports`                        |
| -------------------- | -------------------------------- | ------------------------------------------ |
| **Purpose**          | Persist arbitrary files          | Collect and display structured reports     |
| **Usage**            | Archive files for later use      | Enable GitLab to parse and display reports |
| **Upload Condition** | Based on `when` (e.g., `always`) | Always uploaded, regardless of job result  |
| **UI Integration**   | Files available for download     | Reports displayed in GitLab UI components  |
| **Common Use Cases** | Build outputs, logs, binaries    | Test results, code quality, security scans |
