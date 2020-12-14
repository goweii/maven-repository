# 如何引用

```
maven { url "https://raw.githubusercontent.com/goweii/maven-repository/master/releases/" }
```

# 如何发布

新建maven-upload.gradle，并在需要发布的module引用

- module/build.gradle
```
apply from: "$rootDir/maven-upload.gradle"
```

- maven-upload.gradle
```
apply plugin: 'maven'

project.group "per.goweii.demo"
project.version = "1.0.0"
def fullProjectName = getFullProjectName()

String getFullProjectName() {
    def fullProjectName = project.name
    def parentProject = project.parent
    while (project.rootProject != parentProject) {
        fullProjectName = "${parentProject.name}-${fullProjectName}"
        parentProject = parentProject.parent
    }
    return fullProjectName
}

uploadArchives {
    repositories {
        mavenDeployer {
            snapshotRepository(url: uri("替换/maven-repository/snapshots"))
            repository(url: uri("替换/maven-repository/releases"))
            pom.packaging = "aar"
            pom.groupId = project.group
            pom.artifactId = fullProjectName
            pom.version = project.version
        }
    }
}
```
