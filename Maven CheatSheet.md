# Maven Commands Reference

## `mvn clean`
This command cleans the Maven project by deleting the target directory.

Example of the output:

```

\[INFO] --- maven-clean-plugin:2.5\:clean (default-clean) @ maven-example-jar ---
\[INFO] Deleting /Users/sammy/Desktop/maven-examples/maven-example-jar/target
\[INFO] ------------------------------------------------------------------------
\[INFO] BUILD SUCCESS
\[INFO] ------------------------------------------------------------------------

```

---

## `mvn compiler:compile`
This command compiles the Java source classes of the Maven project.

Example of the output:

```

\[INFO] --- maven-compiler-plugin:3.8.1\:compile (default-cli) @ maven-example-jar ---
\[INFO] Changes detected - recompiling the module!
\[INFO] Compiling 1 source file to /Users/sammy/Desktop/maven-examples/maven-example-jar/target/classes
\[INFO] ------------------------------------------------------------------------
\[INFO] BUILD SUCCESS
\[INFO] ------------------------------------------------------------------------

```

---

## `mvn compiler:testCompile`
This command compiles the test classes of the Maven project.

Example of the output:

```

\[INFO] --- maven-compiler-plugin:3.8.1\:testCompile (default-cli) @ maven-example-jar ---
\[INFO] Changes detected - recompiling the module!
\[INFO] Compiling 1 source file to /Users/sammy/Desktop/maven-examples/maven-example-jar/target/test-classes
\[INFO] ------------------------------------------------------------------------
\[INFO] BUILD SUCCESS
\[INFO] ------------------------------------------------------------------------

```

---

## `mvn package`
This command builds the Maven project and packages it into a JAR, WAR, etc.

Example of the output:

```

\[INFO] --- maven-compiler-plugin:3.8.1\:compile (default-compile) @ maven-example-jar ---
\[INFO] Changes detected - recompiling the module!
...
\[INFO] --- maven-jar-plugin:2.4\:jar (default-jar) @ maven-example-jar ---
\[INFO] Building jar: /Users/sammy/Desktop/maven-examples/maven-example-jar/target/maven-example-jar-0.0.1-SNAPSHOT.jar
\[INFO] ------------------------------------------------------------------------
\[INFO] BUILD SUCCESS

```

---

## `mvn install`
This command builds the Maven project and installs the project files (JAR, WAR, pom.xml, etc.) to the local repository.

Example of the output:

```

\[INFO] --- maven-install-plugin:2.4\:install (default-install) @ maven-example-jar ---
\[INFO] Installing /Users/sammy/Desktop/maven-examples/maven-example-jar/target/maven-example-jar-0.0.1-SNAPSHOT.jar to /Users/sammy/.m2/repository/com/example/maven/maven-example-jar/0.0.1-SNAPSHOT/maven-example-jar-0.0.1-SNAPSHOT.jar
\[INFO] ------------------------------------------------------------------------
\[INFO] BUILD SUCCESS

```

---

## `mvn deploy`
This command deploys the artifact to the remote repository. The remote repository should be configured properly in the `pom.xml` file's `distributionManagement` tag.

---

## `mvn validate`
This command validates the Maven project to ensure that everything is correct and all the necessary information is available.

---

## `mvn dependency:tree`
This command generates the dependency tree of the Maven project.

Example of the output:

```

\[INFO] --- maven-dependency-plugin:2.8\:tree (default-cli) @ Mockito-Examples ---
\[INFO] com.example.mockito\:Mockito-Examples\:jar:1.0-SNAPSHOT
\[INFO] +- org.junit.platform\:junit-platform-runner\:jar:1.2.0\:test
...

```

---

## `mvn dependency:analyze`
This command analyzes the Maven project to identify the unused declared and used undeclared dependencies.

Example of the output:

```

\[INFO] --- maven-dependency-plugin:2.8\:analyze (default-cli) @ Mockito-Examples ---
\[WARNING] Used undeclared dependencies found:
\[WARNING]    org.junit.jupiter\:junit-jupiter-api\:jar:5.2.0\:test
\[WARNING] Unused declared dependencies found:
\[WARNING]    org.junit.platform\:junit-platform-runner\:jar:1.2.0\:test

```

---

## `mvn archetype:generate`
This command generates skeleton Maven projects of different types, such as JAR, web application, Maven site, etc.

Example of the output:

```

\[INFO] ----------------------------------------------------------------------------
\[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
\[INFO] ----------------------------------------------------------------------------
\[INFO] Project created from Archetype in dir: /Users/sammy/Desktop/maven-examples/maven-example-jar
\[INFO] ------------------------------------------------------------------------
\[INFO] BUILD SUCCESS

```

---

## `mvn site:site`
This command generates a site for the project. You will notice a `site` directory in the `target` directory.

Example path: `/Users/sammy/Desktop/maven-examples/maven-example-jar/target/site/index.html`

---

## `mvn test`
This command runs the test cases of the project.

Example of the output:

```

\[INFO] -------------------------------------------------------
\[INFO]  T E S T S
\[INFO] -------------------------------------------------------
\[INFO] Running TestSuite
...
\[INFO] Results:
\[INFO] Tests run: 5, Failures: 0, Errors: 0, Skipped: 0
\[INFO] BUILD SUCCESS

```

---

## `mvn compile`
This command compiles the source Java classes of the project.

Example of the output:

```

\[INFO] --- maven-compiler-plugin:3.7.0\:compile (default-compile) @ Mockito-Examples ---
\[INFO] Changes detected - recompiling the module!
\[INFO] Compiling 10 source files to /Users/sammy/Desktop/maven-examples/Mockito-Examples/target/classes

```

---

## `mvn verify`
This command builds the project, runs all the test cases, and runs any checks on the results of the integration tests to ensure quality criteria are met.

---

## Maven Options

### `mvn -help`
This command-line option prints the Maven usage and all the available options.

### `mvn -f dir/pom.xml package`
This command-line option builds a project from a different location. It’s useful when you have to run a Maven build from a script.

### `mvn -o package`
This command-line option runs the Maven build in offline mode, so Maven won't look for any JARs in the remote repository.

### `mvn -q package`
This command-line option runs the Maven build in quiet mode, so that only the test case results and errors are displayed.

### `mvn -X package`
This command-line option prints the Maven version and runs the build in debug mode, so that all messages are displayed.

### `mvn -v`
This command-line option displays the Maven version information.

### `mvn -DskipTests package`
This command-line option applies the skipTests system property to skip the unit test cases from the build cycle.

### `mvn -T 4 clean install`
This command-line option tells Maven to run parallel builds using the specified thread count.
