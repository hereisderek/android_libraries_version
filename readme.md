# android_libraries_version

## usage:
1. fork (recommended*) and clone into your project as submodule
`git submodule add git@github.com:1and1get2/android_libraries_version.git recorder_android_mvvm/versions/`

2. hook it up
`ln ./recorder_android_mvvm/versions/versions.gradle ./recorder_android_mvvm/`

3. use it

```
buildscript {
    apply from: 'versions.gradle'

    dependencies {
            **classpath deps.android_gradle_plugin**
            **classpath deps.kotlin.plugin**
            **classpath deps.androidx.navigation.classpath**

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
        addRepos(repositories)
        repositories {
            // any other repos you wish to add
        }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```

and in your app.gradle

```

apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'kotlin-kapt'
apply plugin: 'realm-android'
apply plugin: 'androidx.navigation.safeargs'

android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "whatever"
        minSdkVersion build_versions.min_sdk
        // or 
        minSdkVersion 20
        targetSdkVersion build_versions.target_sdk
        multiDexEnabled true

        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    /** kotlin */
    implementation deps.kotlin.stdlib
    implementation deps.kotlin.reflect
    implementation deps.kotlin.anko

    /** google libraries */
    implementation deps.androidx.constraintlayout
    implementation deps.androidx.material
    implementation deps.androidx.appcompat
    implementation deps.androidx.cardview

    implementation deps.androidx.navigation.legacy_fragment_ktx
    implementation deps.androidx.navigation.legacy_ui_ktx

    /** firebase */
    implementation deps.firebase.core
    implementation deps.firebase.database

    /** tools */
    implementation deps.timber
    implementation deps.rxjava2
    implementation deps.rx_android
    implementation deps.glide.runtime
    kapt deps.glide.compiler
    implementation deps.ktx
    implementation deps.rxpermissions2

    /** Dagger 2 */
    implementation deps.dagger.runtime
    implementation deps.dagger.android
    implementation deps.dagger.android_support

    kapt deps.dagger.compiler
    kapt deps.dagger.android_support_compiler

    /** debug */
    debugImplementation deps.leakcanary
    releaseImplementation deps.leakcanary_no_op
    implementation deps.stetho.core
    implementation deps.stetho.okhttp3
    implementation 'com.uphyca:stetho_realm:2.2.2'


    /** testing */
    // Dependencies for local unit tests
    testImplementation deps.junit
    testImplementation deps.mockito.all
    testImplementation deps.hamcrest
    testImplementation deps.kotlin.stdlib
    testImplementation deps.kotlin.test

    // Android Testing Support Library's runner and rules
    androidTestImplementation deps.atsl.runner
    androidTestImplementation deps.atsl.rules
    androidTestImplementation deps.androidx.room.testing

    // Dependencies for Android unit tests
    androidTestImplementation deps.junit
    androidTestImplementation deps.mockito.core, { exclude group: 'net.bytebuddy' }
    androidTestImplementation deps.dexmaker

    // Espresso UI Testing
    androidTestImplementation deps.espresso.core
    androidTestImplementation deps.espresso.contrib
    androidTestImplementation deps.espresso.intents
}

apply plugin: 'com.google.gms.google-services'
```

## future updates
I'll try to keep up with all the version upgrades when possible, and I'll see if I can get around to create an automatic script that keep things up-to-date. In the mean time, feel free to create pull requests.


