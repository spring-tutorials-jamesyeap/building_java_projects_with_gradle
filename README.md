# Building Java Projects with Gradle
A short tutorial from Spring: https://spring.io/guides/gs/gradle/

# Plugins in `build.gradle`
Indicates what tasks gradle can run.
```
apply plugin: '<PLUGIN_NAME_1>'
apply plugin: '<PLUGIN_NAME_2>'
apply plugin: '<PLUGIN_NAME_3>'
```

# Repositories in `build.gradle`
Indicates where the build should get its dependencies from.
```
repositories { 
    mavenCentral() 
}
```

# Dependencies in `build.gradle`
Indicates what external dependencies used in the build.

Note that dependencies are read from right-to-left. For example, "joda-time:joda-time:2.2" asks for `version 2.2` of the `joda-time library`, in the `joda-time group`.

## Dependency types
* `implementation`: Required dependencies for compiling the project code, but that will be provided at runtime by a container running the code (for example, the Java Servlet API).
* `testImplementation`: Dependencies used for compiling and running tests, but not required for building or running the project’s runtime code.

```
sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    implementation "joda-time:joda-time:2.2"
    testImplementation "junit:junit:4.12"
}
```

# JAR artifact in `build.gradle`
```
jar {
    archiveBaseName = 'gs-gradle'
    archiveVersion =  '0.1.0'
}
```
This renders the JAR file named "gs-gradle-0.1.0.jar"

# Adding Gradle Wrapper
To allow other systems to run a Gradle build without having Gradle installed, add Gradle Wrapper scripts to the project by running the following command:
```bash
gradle wrapper --gradle-version 6.0.1
```

Then, in other systems, the gradle build can be executed using:
```bash
./gradlew build
```

# Commands
```bash
# lists all tasks available to run
# 	- needs to have "build.gradle" for this command to be run
# 	- specifying new plugins in "build.gradle" can add new tasks to this list
gradle tasks

# this task compiles, tests, and assembles the code into a JAR file
gradle build
```

## Running `gradle build`
After running this command, a folder "build" will be created, which contains the compiled Java code `.class` and the `.jar` file.

## Creating a fat JAR
If the project uses external dependencies, these have to be included into the JAR file for them to be runnable on other systems.

Include the task in `build.gradle`:
```
// creates a runnable JAR file that includes all external dependencies
task fatJar(type: Jar) {
    manifest {
        attributes 'Main-Class': "${mainClassName}"
    }
    archiveBaseName = "${rootProject.name}"
    from { configurations.compileClasspath.collect { it.isDirectory() ? it : zipTree(it) } }
    with jar
}
```

Then, run the task using the following command:
```bash
./gradlew fatJar
```