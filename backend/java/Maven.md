Maven is a build automation tool , it is a project under Apache Software Foundation , created in 2003 and the current version : 3.3.1

#### what is Maven?
- Maven is a build tool
- Dependency management tool
- Project management tool
- Standardized approach to building software
- Command line tool
- IDE integration

##### what is Build tools?
- Creates deployable artifacts from source code
	- A **project artifact** is any document, file, or deliverable created during a project's lifecycle that helps define, develop, or support the project. Artifacts can be used for planning, development, communication, and documentation.
	  
- Automated / repeatable builds
- Deploying artifacts on servers
- IDE independence
- Integration with other build tools

##### Dependency Management
- Download project dependencies from centralized repositories
- Automatically resolve the libraries required by project dependencies
- Dependency scoping

##### Project Management
- Artifact versioning
- Change logs
- Documentation
- java docs
- Reports

##### Standardized software Builds
- Uniformity across projects through patterns
- Convention over configuration
- Consistent path for all the projects


#### Maven Landscape (all components that make up Maven)

##### Project Object Model (POM)
- it describes , configures and customizes a Maven Project
- Maven reads the pom.xml file to build a project
- Defines the "address" for the project artifact using a coordinate system
- Specifies project information , plugins , goals , dependencies and profiles

##### Repositories
- Holds build artifacts and dependencies of varying types
- Local repositories (local cache)
- Remote repositories
- Local repository takes precedence during dependency resolution

##### Plugins and Goals

In **Maven**, **plugins** and **goals** are essential components that define how your build process is executed.

Plugins in Maven

- A **Maven plugin** is a collection of one or more **goals** that perform specific tasks during the build lifecycle. Plugins allow you to extend Maven's functionality by adding features such as compiling code, running tests, packaging applications, generating reports, and more.

Types of Plugins

- **Build Plugins**: These execute during the build process (e.g., `maven-compiler-plugin`, `maven-surefire-plugin`).
- **Reporting Plugins**: These generate project reports (e.g., `maven-site-plugin`).

Goals in Maven

A **goal** is a specific task within a plugin. Each plugin can have multiple goals, and each goal performs a specific function.

Example Goals

- `mvn compiler:compile` → Calls the `compile` goal of `maven-compiler-plugin`
- `mvn clean` → Calls the `clean` goal of `maven-clean-plugin`
- `mvn package` → Calls the `package` goal of `maven-jar-plugin` or `maven-war-plugin`

Maven automatically binds certain plugin goals to phases in the **build lifecycle**


##### Life cycle and phases
- lifecycle is a sequence of name phases
- phases are executed sequentially
- 3 life cycles: clean , default , site
- Executing a phase executes all previous phases


##### How does Maven Work?
![[Pasted image 20250211012111.png]]

-------------
#### Practical Maven (configuration and build)

You can use Maven to generate project Template  
```
mvn archetype:generate "-Dfilter=org.apache.maven.archetypes:"
```
without the filter you see all project templates which is huge , with filter you see what you want

information important to maven :
- groupId: unique id for the organization (ex: com.khalwsh.maven)
- artifactId: project name (ex: text editor)
- version: version number (ex: 1.0)
- package : the package contain your project (default: groupId )

if you configure your project using maven it creates pom.xml , creates the package contain your project and also create test folder to include your tests 

You can use Maven to compile your code by 
```
mvn compile
```
it creates target folder which will contain your .class files

```
mvn test
```
this command compile and test

```
mvn clear
```
delete the target folder

```
mvn package
```
compile and test then package the code into jar file

```
mvn install
```
compile , test , package and install the jar file in the local maven repository 

in order to add dependencies you just modify the pom.xml file adding this dependency  

--------------------------------
#### plugin
plugin (or plug-in) is a software component that adds specific features or functionalities to an existing application without modifying its core structure. Plugins allow software to be **extensible**, meaning developers can introduce new capabilities without altering the main codebase.

##### Key Characteristics of Plugins:

1. **Modularity** – Plugins are separate modules that can be added or removed dynamically.
2. **Reusability** – They provide reusable functionalities that can be shared across multiple applications.
3. **Customization** – Users can enhance or modify software behavior without modifying the core code.
4. **Encapsulation** – They are self-contained, reducing dependency on the main application.

##### Examples of Plugins in SWE:

- **Web Browsers**: Extensions like ad blockers or password managers in Chrome or Firefox.
- **IDEs & Editors**: VS Code and JetBrains IDEs support plugins for linters, themes, and language support.
- **CMS Platforms**: WordPress plugins add SEO, security, and analytics features.
- **Game Engines**: Unreal Engine and Unity support plugins for new rendering techniques or AI behaviors.
- **Back-End Frameworks**: Spring Boot allows integrating features like security, database access, and monitoring via plugins.

##### How Plugins Work:

- They typically follow a **plugin architecture**, where the main application provides a standard interface or API that plugins implement.
- Common approaches:
    - **Dynamic Loading** – Plugins are loaded at runtime (e.g., using reflection in Java or dynamic imports in Python/Node.js).
    - **Dependency Injection** – Frameworks like Spring or Angular use DI to inject plugin functionality.


#### **Plugins in Maven**

In **Maven**, plugins are essential for executing tasks such as compiling code, running tests, packaging applications, and deploying artifacts. A **Maven plugin** is a collection of goals, where each goal represents a specific task.

##### **Types of Maven Plugins**

Maven has two types of plugins:

1. **Build Plugins**
    - Used during the build lifecycle (e.g., compiling code, packaging JARs, running tests).
    - Example: `maven-compiler-plugin`, `maven-surefire-plugin`.
2. **Reporting Plugins**
    - Generate reports and documentation (e.g., test reports, code coverage).
    - Example: `maven-javadoc-plugin`, `maven-site-plugin`.

##### **How Plugins Work in Maven**

Maven follows a **plugin-based architecture**, where plugins extend its core functionalities. Plugins are executed during different phases of the **Maven build lifecycle**.

##### **Example of Using a Plugin**

A common example is the **Maven Compiler Plugin**, which compiles Java source code.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>17</source>
                <target>17</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

- **`groupId`**: Identifies the plugin provider (Apache in this case).
- **`artifactId`**: Specifies the plugin name.
- **`version`**: Defines which version of the plugin to use.
- **`configuration`**: Provides custom settings for the plugin (e.g., Java version).

##### **Executing Plugins Manually**

Maven plugins can be executed using the `mvn` command:
```sh
mvn compiler:compile
```
This runs the `compile` goal of the `maven-compiler-plugin`.

##### **Commonly Used Maven Plugins**

maven-compiler-plugin (Compiles Java code)
```xml
<plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.1</version>
    <configuration>
        <source>17</source>
        <target>17</target>
    </configuration>
</plugin>
```

**maven-surefire-plugin** (Runs unit tests)
```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.0.0-M7</version>
</plugin>
```

```sh
mvn surefire:test
```

maven-jar-plugin (creates Jar file)
```xml
<plugin>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.2.0</version>
</plugin>
```

```sh
mvn jar:jar
```

**maven-assembly-plugin** (Creates an executable JAR with dependencies)
```xml
<plugin>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>3.5.0</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
            <configuration>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
            </configuration>
        </execution>
    </executions>
</plugin>
```

```sh
mvn clean package
```
This generates a `jar-with-dependencies.jar`.

##### **Custom Maven Plugins**

You can create your own Maven plugin by defining a Java class annotated with `@Mojo`.

```java
@Mojo(name = "say-hello", defaultPhase = LifecyclePhase.PACKAGE)
public class HelloMojo extends AbstractMojo {
    public void execute() throws MojoExecutionException {
        getLog().info("Hello, Maven Plugin!");
    }
}
```

```sh
mvn myplugin:say-hello
```

