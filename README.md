# Building Java Projects with Gradle
A short tutorial from Spring: https://spring.io/guides/gs/gradle/

# `build.gradle`
```
apply plugin: '<PLUGIN_NAME_1>'
apply plugin: '<PLUGIN_NAME_2>'
apply plugin: '<PLUGIN_NAME_3>'
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