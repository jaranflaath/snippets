# Gradle

* [AppCenter distribution](#appcenter-distribution)
* [Bundle/APK signing](#bundleapk-signing)
* [Include build / replace dependency](#include-build-and-dependency-substitution)
* [Set release priority with Play Publisher Plugin](#set-release-priority-with-play-publisher-plugin)

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

## Set release priority with Play Publisher Plugin

The following configuration for the [Gradle Play Publisher Plugin](https://github.com/Triple-T/gradle-play-publisher) enables setting of the priority for a specific version name via gradle.properties. 

If the release should have a priorty other than default, simply set the `google_play.1.0.priority=3` to set priority for version 1.0 to 3.
```
play {
    defaultToAppBundles.set(true)
    track.set("internal")

    def googlePlayUpdatePriority = project.properties["google_play.${project.android.defaultConfig.versionName}.priority"]
    if (googlePlayUpdatePriority != null) {
        updatePriority.set(Integer.parseInt(googlePlayUpdatePriority))
    }

    def serviceAccountFile = project.properties["serviceAccountFile"]
    if (serviceAccountFile == null) {
        serviceAccountFile = "gradle-play-publishing.json"
    }

    serviceAccountCredentials.set(file(serviceAccountFile))
}
```
