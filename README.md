# Android-CustomSDK

Building Custom SDK with all Hidden APIs to work with Android System Applications with Android Studio.

This can be done either by customizing the sdk itself or importing the library into Android Studio. But the convenient way is that to build a custom sdk provided as the first step.

## I. Building Custom SDK

1) Download the aosp repo

2) source build/envsetup.sh

3) lunch [your-build-variant]
  
4) make sdk

5) Go to /aosp_root/out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/ and copy classes-header.jar file

6) Copy your SDK's android.jar file under /Sdk/platforms/android-[xx]/ and copy android.jar file
  
7) Create a new folder and extract android.jar contents.

8) Extract classes-header.jar on the same new folder where we extracted android.jar file. Replace all the files if the file name already exists.

9) Make a new android.jar file by <b>"/new_folder$jar cfM ../android.jar ."</b> inside the new folder.

10) The above command will generate a new android.jar file outside the new folder.

11) Copy the generated android.jar file and replace it in your SDK directory /Sdk/platforms/android-[xx]/

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
