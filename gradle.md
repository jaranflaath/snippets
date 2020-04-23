# Gradle

* [Bundle/APK signing](#Bundle_APK_signing)

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