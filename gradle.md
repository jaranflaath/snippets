# Gradle

* [AppCenter distribution](#appcenter-distribution)
* [Bundle/APK signing](#bundleapk-signing)

## AppCenter distribution
```groovy
plugins {
    ...
    id "com.betomorrow.appcenter" version "1.2.1"
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
