# Android-CustomSDK

Building Custom SDK with all Hidden APIs to work with Android System Applications with Android Studio.

This can be done either by customizing the sdk itself or importing the library into Android Studio. But the convenient way is that to build a custom sdk provided as the first step.

## I. Building Custom SDK

1) Download the aosp repo

2) $source build/envsetup.sh

3) $lunch [your-build-variant]
  
4) $make sdk

5) After <b>$make sdk</b>, the sdk wil be generated at <b>/root_aosp/out/host/linux-x86/sdk/</b> on Linux and <b>/root_aosp/out/host/darwin-x86/sdk/</b> on Mac OS

6) If you want build SDK for Windows OS, then run <b>$make win_sdk</b> instead of <b>$make sdk</b>. The generated sdk will be at <b>/root_aosp/out/host/windows-x86/sdk/</b>

7) Go to /aosp_root/out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/ and copy classes-header.jar file

8) Copy your SDK's android.jar file under /Sdk/platforms/android-[xx]/ and copy android.jar file
  
9) Create a new folder and extract android.jar contents.

10) Extract classes-header.jar on the same new folder where we extracted android.jar file. Replace all the files if the file name already exists.

11) Make a new android.jar file by <b>"/new_folder$jar cfM ../android.jar ."</b> inside the new folder.

12) The above command will generate a new android.jar file outside the new folder.

13) Copy the generated android.jar file and replace it in your SDK directory /Sdk/platforms/android-[xx]/

## II. Importing the library into Android Studio

1) Copy "classes.jar" from /aosp_root/out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/

2) Paste the library under /root_project_dir/app/system_libs/ as "framework.jar"

3) Add it as a dependency in your app module [build.gradle] file - <b>implementation files('system_libs/framework.jar')</b>

4) Configure your project's [build.gradle] file with the below lines of code for java compilation.

<div align="center">
<img src="https://github.com/svignesh93/Android-CustomSDK/blob/master/assets/root_project_build_gradle.png" alt="Project Build Gradle"/>
</div>

5) Configure your module's [build.gradle] file with the below code that brings the framework.jar file as a top library and moves SDK to the last. So that it points to framework.jar APIs instead of SDK.

<div align="center">
<img src="https://github.com/svignesh93/Android-CustomSDK/blob/master/assets/module_project_build_gradle.png" alt="Project Build Gradle"/>
</div>

## III. AOSP Prebuilt SDK

1) Prebuilt SDK in AOSP can be found at <b>/aosp_root/prebuilts/sdk/current/system/android.jar</b>. But access to Hidden APIs and Framework AIDL services were limited.

## References

1) [Building Custom SDK by kwagjj](https://kwagjj.wordpress.com/2017/10/27/building-custom-android-sdk-from-aosp-and-adding-it-to-android-studio/)

2) [Importing Framework Library into Android Studio by kwagjj](https://kwagjj.wordpress.com/2017/08/10/using-framework-jar-in-android-studio/)

3) [Building JAR File](https://www.codejava.net/java-core/tools/using-jar-command-examples)

4) [Restrictions on Non-SDK interfaces](https://developer.android.com/distribute/best-practices/develop/restrictions-non-sdk-interfaces)

5) [Embedded Android by Karim Yaghmour](https://www.oreilly.com/library/view/embedded-android/9781449327958/ch04.html)

6) [Carbon - Creates images for your source code](https://carbon.now.sh/)

## License

~~~

Copyright 2020 Vignesh S

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, 
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

~~~
