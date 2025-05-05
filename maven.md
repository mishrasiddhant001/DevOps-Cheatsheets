
# 🧰 DevOps & Maven Commands Cheat Sheet


## 🔧 DevOps Commands

### 🐧 Linux Basics
```bash
ls -l       # List files in long format
cd /path    # Change directory
pwd         # Show current directory
cp src dest # Copy files
mv src dest # Move/rename files
rm file     # Delete file
ps aux      # List all processes
kill PID    # Kill process by ID
chmod +x file.sh  # Make file executable
````

### 📦 Package Managers

#### Debian/Ubuntu

```bash
sudo apt update
sudo apt install <package>
sudo apt remove <package>
```

#### Red Hat/CentOS

```bash
sudo yum install <package>
sudo yum update
```

### 🐳 Docker

```bash
docker build -t name .
docker run -d -p 8080:80 name
docker ps
docker exec -it <container> bash
docker stop <container>
```

### ☸️ Kubernetes

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl apply -f config.yaml
kubectl delete -f config.yaml
kubectl exec -it <pod-name> -- bash
```

### 🧪 CI/CD (Jenkins Pipeline Example)

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
```

---

## ☕ Maven Commands

### 🔹 Build Lifecycle Phases

```bash
mvn clean              # Deletes /target directory
mvn validate           # Validates the project
mvn compile            # Compiles the code
mvn test               # Runs unit tests
mvn package            # Packages the project into JAR/WAR
mvn verify             # Verifies the package
mvn install            # Installs to local repo (~/.m2)
mvn deploy             # Deploys to remote repo
```

### 🔹 Project Creation

```bash
# Java project
mvn archetype:generate -DgroupId=org.example -DartifactId=app

# Web project
mvn archetype:generate -DgroupId=org.example -DartifactId=webapp -DarchetypeArtifactId=maven-archetype-webapp

# Create archetype from current project
mvn archetype:create-from-project
```

### 🔹 Common Options

```bash
-DskipTests=true             # Compile but skip test run
-Dmaven.test.skip=true       # Skip test compile & run
-T 4                         # Use 4 threads
-rf, --resume-from module    # Resume build from module
-pl, --projects module1,...  # Build specific modules
-am, --also-make             # Include dependencies
-o, --offline                # Run offline
-X, --debug                  # Debug mode
-P profile                   # Activate profile
-U                           # Update snapshots
-ff                          # Fail fast
```

### 🔹 Useful Plugins

**Help Plugin**

```bash
mvn help:describe
mvn help:effective-pom
```

**Dependency Plugin**

```bash
mvn dependency:analyze
mvn dependency:tree
```

**Compiler Plugin Setup (`pom.xml`)**

```xml
<plugin>
 <groupId>org.apache.maven.plugins</groupId>
 <artifactId>maven-compiler-plugin</artifactId>
 <version>3.6.1</version>
 <configuration>
   <source>1.8</source>
   <target>1.8</target>
 </configuration>
</plugin>
```

**Versions Plugin**

```bash
mvn versions:display-dependency-updates
```

**Spring Boot**

```bash
mvn spring-boot:run
```

**Exec Plugin**

```bash
mvn exec:exec
```

```

---

Let me know if you'd like this formatted differently or if you want to add Git, Terraform, or Ansible commands too.
```
