# Gradle

* [AppCenter distribution](#appcenter-distribution)
* [Bundle/APK signing](#bundleapk-signing)
* [Include build / replace dependency](#include-build-and-dependency-substitution)

## AppCenter distribution
```groovy
buildscript {
    repositories {
        maven {
            url "https://plugins.gradle.org/m2/"
        }
    }

    dependencies {
        classpath "gradle.plugin.com.betomorrow.gradle:appcenter-plugin:1.2.1"
    }
}
appcenter {
    apiToken = "api-token-created-from-any-user"
    ownerName = "owner/org"
    notifyTesters = false
    apps {        
        release {
            distributionGroups = ["Public"]
            appName = "App-Name"
            releaseNotes = "Release description"
        }
    }
}
```

## Bundle/APK signing

```groovy
android {
    signingConfigs {
        release {
            storeFile file("./keystore.jks")
            storePassword "store-password"
            keyAlias "key-alias"
            keyPassword "key-password"
        }
        someOtherSigningConfig {
            storeFile file("./another-keystore.jks")
            storePassword "store-password"
            keyAlias "key-alias"
            keyPassword "key-password"
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
        beta { 
            signingConfig signingConfigs.someOtherSigningConfig
        }
    }
}

```

## Include build and dependency substitution

During development, replace a dependency by including a local build for faster debugging and modifications.

```
includeBuild("/path/to/another/build") {
    dependencySubstitution {
        substitute module('group.id:artifact') with project(':module-from-another-build')
    }
}
```
