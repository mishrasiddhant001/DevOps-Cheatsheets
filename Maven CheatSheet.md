## 🛠️ Maven Commands Guide

This document provides a friendly yet comprehensive overview of essential and advanced Maven commands, tailored for DevOps engineers.

 
---
### 🔹 Basic Maven Commands

#### 🧹 `mvn clean`

Removes the `target/` directory, wiping out compiled classes and artifacts. Ensures a fresh start for the build.

👉 **Use it when:** You want to avoid conflicts caused by outdated files.

 

#### 🛠️ `mvn compile`

Compiles Java source code located in `src/main/java`.

👉 **Use it when:** You need to build your main application code (no tests).

 

#### 🧪 `mvn test`

Compiles and runs test cases from `src/test/java`.

👉 **Use it when:** You're verifying that your code changes haven't broken anything.

 

#### 📦 `mvn package`

Compiles, tests, and packages your project into a `.jar` or `.war` file.

👉 **Use it when:** You’re preparing a distributable application.

 

#### 📥 `mvn install`

Does everything `package` does and then installs the artifact into your local Maven repository (`~/.m2/repository`).

👉 **Use it when:** Other local projects need to depend on this build.

 

#### 🌍 `mvn deploy`

Builds the project and uploads the final artifact to a remote repository.

👉 **Use it when:** You're ready to share your build with the team or deploy it.

 

#### 🔎 `mvn validate`

Checks that your project is correctly set up before building.

👉 **Use it when:** You want to catch configuration issues early, especially in CI pipelines.

 

#### 🌲 `mvn dependency:tree`

Shows a visual tree of your project’s dependencies.

👉 **Use it when:** Debugging dependency issues or checking for conflicts.

 

#### 🔍 `mvn dependency:analyze`

Highlights unused and undeclared dependencies.

👉 **Use it when:** Cleaning up or optimizing your `pom.xml`.

 

#### 🧱 `mvn archetype:generate`

Scaffolds a new Maven project using predefined templates.

👉 **Use it when:** You want to quickly start a new project with best practices.

 

#### 📊 `mvn site:site`

Generates a full project website with metrics, reports, and documentation.

👉 **Use it when:** You need internal documentation or project insights.

 

#### 🧪 `mvn compiler:testCompile`

Compiles test classes only (no execution).

👉 **Use it when:** You want to ensure test classes are compiled before running tests.

 

#### ✅ `mvn verify`

Runs all verification steps like integration tests.

👉 **Use it when:** You want a final check before packaging or deploying.

 ---

### 💻 Maven CLI Options

#### ❓ `mvn -help`

Shows available Maven CLI options.

#### 📂 `mvn -f path/to/pom.xml package`

Builds a Maven project using a specific `pom.xml` file.

👉 **Use it when:** Working with non-standard or multi-module structures.

 

#### 🚫 `mvn -o package`

Runs Maven in offline mode.

👉 **Use it when:** You’re without internet or want faster builds.

 

#### 🤫 `mvn -q package`

Quiet mode—outputs only errors.

👉 **Use it when:** You need clean logs (e.g., in CI).

 

#### 🐞 `mvn -X package`

Enables debug mode.

👉 **Use it when:** You're troubleshooting a complex issue.

 

#### 📌 `mvn -v`

Displays Maven version and system details.

👉 **Use it when:** You’re confirming tool versions.

 

#### 🕒 `mvn -DskipTests package`

Skips running tests during the build.

👉 **Use it when:** You need faster builds in dev.

 

#### ⚡ `mvn -T 4 clean install`

Runs builds using 4 threads.

👉 **Use it when:** Speeding up builds in large/multi-module projects.

 ---

### 🚀 DevOps & Advanced Maven Commands

#### 🧭 `mvn versions:display-dependency-updates`

Shows newer versions of project dependencies.

👉 **Use it when:** Keeping dependencies up-to-date.

 

#### 🧩 `mvn versions:display-plugin-updates`

Lists outdated Maven build plugins.

👉 **Use it when:** Ensuring stability and security.

 

#### 🔄 `mvn versions:update-properties`

Auto-updates version properties.

👉 **Use it when:** Managing dependencies with properties or during releases.

 

#### 🏁 `mvn release:prepare`

Prepares a release: updates versions, commits, tags.

👉 **Use it when:** Automating your release process.

 

#### 🚢 `mvn release:perform`

Performs the release by building and deploying the prepared version.

👉 **Use it when:** Completing a release lifecycle.

 

#### 🧾 `mvn scm:checkin`

Commits files to source control (e.g., Git).

👉 **Use it when:** Automating version-controlled builds.

 

#### 📈 `mvn jacoco:report`

Generates code coverage reports.

👉 **Use it when:** Enforcing or reviewing test coverage.

 

#### 🧪 `mvn sonar:sonar`

Runs a SonarQube scan.

👉 **Use it when:** Performing static code analysis and quality checks.

> 📝 Requires a SonarQube server and plugin config.

 

#### 🌐 `mvn site:deploy`

Deploys the generated site to a remote server.

👉 **Use it when:** Publishing docs to GitHub Pages or internal portals.

 

#### 📦 `mvn dependency:copy-dependencies`

Copies all dependencies to a local folder.

👉 **Use it when:** Building Docker images or local distros.

 

#### 🧰 `mvn exec:exec`

Executes a system command or Java class.

👉 **Use it when:** Custom build tasks or setup scripts.


---
In Maven, command-line flags (options) allow you to customize the behavior of the build process. Two commonly used flags are `-D` and `-P`
---

## 🔹 `-D` (Define a system property or user property)

Used to set a **system property** or **user property** for Maven or the build process.

### ✅ Syntax:

```bash
mvn clean install -DpropertyName=propertyValue
```

### ✅ Common examples:

* Set the Maven build to use a specific environment:

  ```bash
  mvn package -Denvironment=production
  ```
* Run tests selectively:

  ```bash
  mvn test -Dtest=MyTestClass
  ```
* Skip tests during build:

  ```bash
  mvn install -DskipTests
  ```

**Note:** These properties can be accessed in your `pom.xml` using `${propertyName}`.

---

## 🔹 `-P` (Activate Maven profile)

Used to activate one or more **build profiles** defined in your `pom.xml` or `settings.xml`.

### ✅ Syntax:

```bash
mvn install -Pprofile1,profile2
```

### ✅ Example:

```bash
mvn deploy -Pproduction
```

A profile may contain custom build configurations, dependencies, plugins, etc., tailored to a specific environment like `dev`, `staging`, or `prod`.

---

## 🔹 Other Common Maven Flags

### ✅ `-f` (Specify the POM file)

Run Maven using a custom `pom.xml` file.

```bash
mvn -f path/to/other-pom.xml clean install
```

---

### ✅ `-X` (Debug output)

Shows **debug-level logs** for troubleshooting Maven builds.

```bash
mvn clean install -X
```

---

### ✅ `-e` (Show exception stack traces)

Prints the **full stack trace** of errors if the build fails.

```bash
mvn clean install -e
```

---

### ✅ `-B` (Batch mode)

Used in **CI/CD scripts** to disable interactive prompts.

```bash
mvn clean install -B
```

---

### ✅ `-U` (Force update of snapshots)

Forces Maven to check **remote repositories** for updated SNAPSHOT versions.

```bash
mvn clean install -U
```

---

### ✅ `-T` (Parallel builds)

Enables **parallel build execution**.

```bash
mvn clean install -T 4    # Use 4 threads
mvn clean install -T 1C   # Use 1 thread per CPU core
```

---

## 📝 Summary Table

| Flag          | Purpose                      |
| ------------- | ---------------------------- |
| `-Dkey=value` | Define a property            |
| `-Pprofile`   | Activate build profile       |
| `-f`          | Use specific POM file        |
| `-X`          | Debug logging                |
| `-e`          | Show stack traces            |
| `-B`          | Batch mode (non-interactive) |
| `-U`          | Update snapshot dependencies |
| `-T`          | Parallel builds              |

---

### 🐳 Maven in CI/CD & Docker Workflows

#### ✅ Jenkins Pipelines

* 🏃 Use `mvn clean install -DskipTests` for fast early-stage builds.
* 🔍 Use `mvn test`, `verify`, and `jacoco:report` for quality gates.

#### 🐋 Docker Builds

Use multi-stage Dockerfiles to keep images slim:

```Dockerfile
# 🛠 Build stage
FROM maven:3.9.4 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# 🚀 Runtime stage
FROM openjdk:17-alpine
COPY --from=builder /app/target/app.jar /app.jar
CMD ["java", "-jar", "/app.jar"]
```

👉 **Use it when:** Creating efficient, production-ready containers.



