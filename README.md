# Android-CustomSDK

Building Custom SDK with all Hidden APIs to work with Android System Applications with Android Studio.

This can be done either by customizing the sdk itself or importing the library into Android Studio. But the convenient way is that to build a custom sdk provided as the first step.

## I. Building Custom SDK

1) Download the aosp repo

2) $source build/envsetup.sh

3) $lunch [your-build-variant]

4) <b>[your-build-variant]</b> could be <b>sdk_phone_x86_64</b>, <b>sdk_phone_x86</b>, <b>sdk-eng</b> if you're building for <b>phone/generic</b> build and it could be <b>aosp_car_x86_64</b>, <b>aosp_car_x86</b> if you're building for generic car. Always prefer to use <b>x86_64 bit</b> variant.

5) $make sdk

6) After <b>$make sdk</b>, the sdk wil be generated at <b>/root_aosp/out/host/linux-x86/sdk/</b> on Linux and <b>/root_aosp/out/host/darwin-x86/sdk/</b> on Mac OS

7) If you want build SDK for Windows OS, then run <b>$make win_sdk</b> instead of <b>$make sdk</b>. The generated sdk will be at <b>/root_aosp/out/host/windows-x86/sdk/</b>

8) Go to /aosp_root/out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/ and copy classes-header.jar file

9) Copy your SDK's android.jar file under /Sdk/platforms/android-[xx]/ and copy android.jar file
  
10) Create a new folder and extract android.jar contents.

11) Extract classes-header.jar on the same new folder where we extracted android.jar file. Replace all the files if the file name already exists.

12) Make a new android.jar file by <b>"/new_folder$jar cfM ../android.jar ."</b> inside the new folder.

13) The above command will generate a new android.jar file outside the new folder.

14) Copy the generated android.jar file and replace it in your SDK directory /Sdk/platforms/android-[xx]/

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

## IV. Building Custom Emulator

1) Download the aosp repo

2) $source build/envsetup.sh

3) $lunch [your-build-variant]

4) <b>[your-build-variant]</b> could be <b>sdk_phone_x86_64</b>, <b>sdk_phone_x86</b> if you're building for <b>phone/generic</b> build and it could be <b>aosp_car_x86_64</b>, <b>aosp_car_x86</b> if you're building for generic car. Always prefer to use <b>x86_64 bit</b> variant.

5) $make -j16

6) $emulator

7) Your emulator will be launched automatically from <b>/aosp_root/prebuilts/android-emulator/linux-x86_64/</b> if you are building on <b>Linux</b> and <b>/aosp_root/prebuilts/android-emulator/darwin-x86_64/</b> if you are building on <b>Mac OS</b> based on the system images generated on <b>/aosp_root/out/target/product/[your-build-variant]</b>

## V. Packaging and sharing Custom Emulator

1) Download the aosp repo

2) $source build/envsetup.sh

3) $lunch [your-build-variant]

4) <b>[your-build-variant]</b> could be <b>sdk_phone_x86_64</b>, <b>sdk_phone_x86</b> if you're building for <b>phone/generic</b> build and it could be <b>aosp_car_x86_64</b>, <b>aosp_car_x86</b> if you're building for generic car. Always prefer to use <b>x86_64 bit</b> variant.

5) $make -j16 sdk sdk_repo

6) This will generate two files under <b>/aosp-root/out/host/linux-x86/sdk/[your-build-variant]/</b> or in <b>/aosp-root/out/host/darwin-x86/sdk/[your-build-variant]/</b> if you are building on <b>Mac OS</b>

    <b>sdk-repo-linux-system-images-eng.[username].zip</b>
  
    <b>repo-sys-img.xml</b>
  
7) <b>sdk-repo-linux-system-images-eng.[username].zip</b> is the actual emulator system image that you normally see in <b>/Sdk/system-images/</b> in an unzipped format.

8) If you want to share this emulator among many developers, then host the both files <b>repo-sys-img.xml</b> and <b>sdk-repo-linux-system-images-eng.[username].zip</b> in an online environment. 

9) Make sure that you change the <b>URL</b> in <b>repo-sys-img.xml</b> of <b>sdk-repo-linux-system-images-eng.[username].zip</b> hosted file.

10) After hosting the file, Go to Android Studio > Tools > SDK Manager > SDK Update Sites > Tap Add Button and provide name and <b>URL</b> of <b>repo-sys-img.xml</b> hosted file and then, tap ok.

11) This adds your custom emulator system image to the System Images page of the SDK <b>(/Sdk/system-images/)</b>.

12) Then go to Android Studio > Tools > AVD Manager > and create a new virtual device with your custom emulator system image.

13) If you want to use the custom emulator locally, then just unzip the file <b>sdk-repo-linux-system-images-eng.[username].zip</b> in <b>/Sdk/system-images/</b> directory and should be in the below folder structure.

14) The folder structure for the system images in <b>/Sdk/system-images/</b> should be,

    <b>android-[xx]/[TagId]/x86_64/</b> - this is for x86_64 version
    
    <b>android-[xx]/[tagId]/x86/</b> - this is for x86 version
    
      <b>[xx]</b> is the android API level (ex. 29)
      
      <b>[TagId]</b> is the emulator variant. It could be <b>android-automotive</b> if you are building for <b>Car</b> and could be <b>default</b> if you are building for others.
      
15) And then, edit <b>source.properties</b> and <b>package.xml</b>:

    <b>Abi</b> - should be x86_64 or x86
    
    <b>TagId</b> - is the emulator variant. It could be <b>android-automotive</b> if you are building for <b>Car</b> and could be <b>default</b> if you are building for others.
    
    <b>TagDisplay or display</b> - Give the name as you like. ex) "Android Automotive System Image" for Car Variant.
    
    <b>display-name</b> - could be "Intel x86 Atom_64 System Image" for "x86_64" or "Intel x86 Atom System Image" for "x86"
    
16) If you can't find <b>package.xml</b> and was not auto-generating then just copy it from other emulator system images from your SDK.

17) Then go to Android Studio > Tools > AVD Manager > and create a new virtual device with your custom emulator system image.

## VI. Error Handling when building SDK / Emulator

1) The Product Variants <b>sdk_phone_x86_64</b>, <b>aosp_x86_64</b> is in <b>/aosp_root/build/target/product/</b> directory.

2) The Product Variant <b>aosp_car_x86_64</b> is in <b>/aosp_root/device/generic/car/</b> directory.

3) Building <b>sdk_phone_x86_64</b> for phone variant will not produce any error and will produce SDK and Emulator System Image without any problem.

4) But for building <b>aosp_car_x86_64</b> for Car will produce some development tools missing errors.

5) The cause of the error for the Car variant <b>aosp_car_x86_64</b> is that it does not include host tools and libs unlike <b>sdk_phone_x86_64</b> has host and tools defined.

    Copy the host tools and libs from <b>sdk_phone_x86_64</b> to <b>aosp_car_x86_64</b>
  
    <b># Define the host tools and libs that are parts of the SDK</b><br>
    -include sdk/build/product_sdk.mk<br>
    -include development/build/product_sdk.mk
    
6) <b>Step 5 was not even tried before and it is still a suggestion only</b>.

7) <b>The errors that I experienced are</b>:

    deployagent.jar was not found in <b>/aosp_root/out/target/product/[your-build-variant]/system/framework/</b> directory.
    
    deployagent was not found in <b>/aosp_root/out/target/product/[your-build-variant]/system/bin/</b> directory.
    
    These errors are produced by <b>/aosp_root/development/build/sdk.atree</b>.
    
8) To solve this problem, first build for <b>aosp_car_x86_64-userdebug</b>, then build <b>aosp_car_x86</b> variant and then build <b>aosp_car_x86_64</b>.

9) After all, copy <b>deployagent.jar</b> from <b>/aosp_root/out/target/product/aosp_car_x86/system/framework/</b> and <b>deployagent</b> from <b>/aosp_root/out/target/product/aosp_car_x86/system/bin/</b> to <b>/aosp_root/out/target/product/aosp_car_x86_64/system/framework/</b> and <b>/aosp_root/out/target/product/aosp_car_x86_64/system/bin/</b>

10) Then try <b>$make sdk sdk_repo</b> again and sdk and emulator system images will be produced.

11) The whole idea is to solve the error is, copy all the missing packaging development tools from other product variants from <b>aosp_root/out/target/product/[other-build-variant]/</b> to your <b>aosp_root/out/target/product/[your-build-variant]/</b>

## References

1) <b>Building Custom SDK by kwagjj ></b> https://kwagjj.wordpress.com/2017/10/27/building-custom-android-sdk-from-aosp-and-adding-it-to-android-studio/

2) <b>Standalone SDK ></b> https://grapheneos.org/build#standalone-sdk

3) <b>Importing Framework Library into Android Studio by kwagjj ></b> https://kwagjj.wordpress.com/2017/08/10/using-framework-jar-in-android-studio/

4) <b>Building JAR File ></b> https://www.codejava.net/java-core/tools/using-jar-command-examples

5) <b>Restrictions on Non-SDK interfaces ></b> https://developer.android.com/distribute/best-practices/develop/restrictions-non-sdk-interfaces

6) <b>Embedded Android by Karim Yaghmour ></b> https://www.oreilly.com/library/view/embedded-android/9781449327958/ch04.html

7) <b>Building and Sharing AVD ></b> https://source.android.com/setup/create/avd

8) <b>Building Android Car Emulator ></b> https://www.embien.com/blog/building-android-car-emulator/ 

9) <b>Creating Acloud Instance ></b> https://source.android.com/setup/start

10) <b>Cuttlefish Virtual Android Devices ></b> https://source.android.com/setup/create/cuttlefish

11) <b>Carbon - Creates images for your source code ></b> https://carbon.now.sh/

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
