The following is a repository of different project that utilize the Android Native Development kit, which enables us to implement parts of an app in native C/C++ code. I am hoping to track my progress in integrating the Clarius APIs, written in C++ and provided as a shared object file, in an Android application using the Android Native Development kit. There are two repositories that are interesting to look at. 

1. hello-jni. This repo provides a basic idea of how we can call C/C++ code in an Android app.
2. hello-libs. This repo provides a basic idea of how we can manage 3rd party C/C++ libraries with Android Studio. As we are only provided a shared object file (no C/C++ code), this repo is more relevant to our current use case.

I am treating liblisten.so similarly to how gperf.so is treated when attempting to import this library. We see that both are provided as .so files here: 

gperf.so is found here (and armeabi-v7a, x86, x86_64) hello-libs/distribution/gperf/lib/arm64-v8a/
liblisten.so is found here (and armeabi-v7a, x86, x86_64) hello-libs/distribution/listen/lib/arm64-v8a/

Note that hello-libs/distribution/listen/lib/arm64-v8a/ is full of additional QT shared object files as well. This is because without these files, this error is observed during runtime:

03-18 13:08:59.210 12948 12948 E AndroidRuntime: java.lang.UnsatisfiedLinkError: dlopen failed: library "libQt5OpenGL.so" not found: needed by /data/app/~pW_8vzM6r5D_AQNwXKMKkw==/com.example.hellolibs-K3tiF92oRwA3H4cb2FAb-A==/lib/arm64/liblisten.so in namespace classloader-namespace

I continually added additional additional shared object Qt dependencies until I no longer had Unsatisified Link errors. However, now I am facing this error during runtime: 

JNI DETECTED ERROR IN APPLICATION: JNI NewGlobalRef called with pending exception java.lang.ClassNotFoundException: Didn't find class "org.qtproject.qt5.android.extras.QtNative" on path: DexPathList[[zip file "/data/app/~tynxHFjVuOpvGQfZMgmTA==/com.example.hello-libs-LLNtuhrtKcuc_Wjqvy9mtw==/base.apk"],nativeLibraryDirectories=[/data/app/~tynxHFjVu0pvGQfZMgmTgA==/com.example.hellolibs-LLn.....]]

I have tried importing org/qtproject/qt5/android/extras/QtNative by adding this to the same directory as the other .so files to no avail. 

Relevant changes thus far: https://github.com/tejaskhorana/ndk-samples/commit/cf98d545fa1f0a71e096cf1bc11e2e09073be24c


Official Documentation Below:

NDK Samples [![Build Status](https://travis-ci.org/googlesamples/android-ndk.svg?branch=master)](https://travis-ci.org/googlesamples/android-ndk) [![Build status](https://ci.appveyor.com/api/projects/status/48tbtqwg4heytmnq?svg=true)](https://ci.appveyor.com/project/proppy/android-ndk)
===========

This repository contains [Android NDK][0] samples with Android Studio [C++ integration](https://www.youtube.com/watch?v=f7ihSQ44WO0&feature=youtu.be).

These samples use the new [CMake Android plugin](https://developer.android.com/studio/projects/add-native-code.html) with C++ support.

Samples could also be built with other build systems:
- for ndk-build with Android Studio, refer to directory [other-builds/ndkbuild](https://github.com/googlesamples/android-ndk/tree/master/other-builds/ndkbuild)
- for gradle-experimental plugin, refer to directory other-builds/experimental. Note that gradle-experimental does not work with unified headers yet: use NDK version up to r15 and Android Studio up to version 2.3. When starting new project, please use CMake or ndk-build plugin.

Additional Android Studio samples:    
- [Google Play Game Samples with Android Studio](https://github.com/playgameservices/cpp-android-basic-samples)
- [Google Android Vulkan Tutorials](https://github.com/googlesamples/android-vulkan-tutorials)
- [Android Vulkan API Basic Samples](https://github.com/googlesamples/vulkan-basic-samples)
- [Android High Performance Audio](https://github.com/googlesamples/android-audio-high-performance)	

Documentation
- [Add Native Code to Your Project](https://developer.android.com/studio/projects/add-native-code.html)
- [CMake for NDK](https://developer.android.com/ndk/guides/cmake.html)
- [Hello-CMake Codelab](https://codelabs.developers.google.com/codelabs/android-studio-cmake/index.html)

Known Issues
- Some are documented at [Android Studio](http://tools.android.com/knownissues) page

For samples using `Android.mk` build system with `ndk-build` see the [android-mk](https://github.com/googlesamples/android-ndk/tree/android-mk) branch.

Build Steps
----------
- With Android Studio: "Open An Existing Android Studio Project" or "File" > "Open", then navigate to & select project's build.gradle file.
- On Command Line: set up ANDROID_HOME and ANDROID_NDK_HOME to your SDK and NDK path, cd to individual sample dir, and do "gradlew assembleDebug"

Support
-------

For any issues you found in these samples, please
- submit patches with pull requests, see [CONTRIBUTING.md](CONTRIBUTING.md) for more details, or
- [create bugs](https://github.com/googlesamples/android-ndk/issues/new) here.

For Android NDK generic questions, please ask on [Stack Overflow](http://stackoverflow.com/questions/tagged/android), Android teams are periodically monitoring questions there.


License
-------

Copyright 2018 The Android Open Source Project, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.

[LICENSE](LICENSE)

[0]: https://developer.android.com/ndk
