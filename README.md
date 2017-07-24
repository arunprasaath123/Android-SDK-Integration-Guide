# Android-SDK-Integration-Guide


Android SDK Integration Guide










Version
2.1
Created By
Arnold (arnold@appunfold.com)
Last Updated
26th Jan 2017
Reviewed By
Sudhakar (sudhakar@appunfold.com)






Table of Contents

Integration Criterion
Admin SDK Installation
Capturing screens from your app
Creating Questionnaire / Walkthroughs
Appunfold Production SDK Installation
Appendix
Access HelpCenter from your Menu
Mapping per-user analytics
TroubleShooting



Integration Criterion
3 Components:

Admin SDK: This SDK is used to take screenshots ONLY. These screenshots sync up with browser-based bashboard in real-time and you use them to compose content.


Appunfold SDK: This SDK goes into deployment. Appunfold SDK does not have permissions to take screenshots and one can only view composed content.


Browser-based Dashboard: Product Managers/Content Creators will use this dashboard to compose/edit/delete content.



Admin SDK and Appunfold SDK require a different set of permissions and support different versions of Android. The following table provides more detail.

Component/Version support
Admin SDK
Appunfold SDK
Android version
4.2 and above
4.0 and above
minSDK
14
14
Permissions
Draw over app
(for Android 6 and above)
NA



Recommendations: 

DO NOT INITIALIZE both Admin SDK & Appunfold SDK together.


Admin Users: Integrate Admin SDK in your app to be used by Product Managers, Content Creators and Support Managers.


Appunfold Users: Integrate Appunfold SDK for deployment to your intended audiences.
Admin SDK Installation

Open your app specific build.gradle (NOT the top level build.gradle)  and include the following lines:
android {...}

repositories {
    jcenter()
}

In the module/app level
--------------------
android {...}

dependencies {
    compile 'com.appunfold:admin:1.0.8'
}


In the root level
--------------------
allprojects {
  repositories {
    maven {
        url "http://dl.bintray.com/appunfold/android" 
     }
    jcenter()
  }
}

AdminSDK should be initialized in the main Application of your app to make sure contextual help is available across your app. 

Create your custom Application class by extending android.app.Application (Skip to step B if you already have a custom application).
Add it to the manifest.

<application
   android:name="com.example.MyApplication">
</application>

Update your custom Application class.
Open up your custom Application (eg. class MyApplication extends android.app.Application).
Initialize AdminSDK by including following lines in onCreate() method of MyApplication.

public class MyApplication extends Application {
   @Override
   public void onCreate() {
       super.onCreate();
       AdminSDK.init(this);
   }
}


Capturing screens from your app
After integration is complete, open your app and you’ll see a button at the left-bottom corner of your app with a ‘?’ symbol. 



1. Click on the Login button to go to the Login screen.
2. Enter your login credentials.


3. This screen will list out all the screens captured. Click on the start capturing button to start capturing images
4. Click on Camera icon to capture image and cross icon to exit image capture.

Creating Questionnaire / Walkthroughs

Go to login page and enter your login credentials. On successful login, you’ll see Dashboard for your app.		

View all your Q&A’s in the Questions page. You can also create questions from this page. To create questions click on New Question.






Add the question and answer and click Save. This will create only contextual FAQs. To create walkthrough click on Add Screen just below the answer.

Add images for the walkthrough you want to create and select any part of the image. Add title, description, audio (optional) and click on OK. Do the same for other images and click on Save. This will save the walkthrough.
	

Appunfold Production SDK Installation
Get the API Key from the Integration page of your dashboard.

Open build.gradle and include the SDK

In the module/app level
--------------------
android {...}

dependencies {
    compile 'com.appunfold:sdk:1.0.8'
}


In the root level
--------------------
allprojects {
  repositories {
    maven {
        url "http://dl.bintray.com/appunfold/android" 
     }
    jcenter()
  }
}

Initialise SDK
Create your custom Application class by extending android.app.Application ( Skip to step B if you already have a custom application. )
Add it to the manifest.

<application
   android:name="com.example.MyApplication">
</application>

Update your custom Application class
Open up your custom Application (eg. class MyApplication extends android.app.Application)
Initialize AppunfoldSDK by including following lines in onCreate() method of MyApplication



public class MyApplication extends Application {
   @Override
   public void onCreate() {
       super.onCreate();
       AppunfoldSDK.init(this, "API-KEY"); 
   }
}











































Appendix
Access HelpCenter from your Menu
For apps who do not want a floating Fab button, you can access HelpCenter from a menu.

If your menu is inside an activity, call this method from where you want to invoke.
AppunfoldSDK.showHelpCenter(this);
		
		or

If your menu is inside a fragment, use this:
		AppunfoldSDK.showHelpCenter(getActivity());
Mapping per-user analytics
To gather user-level analytics, use this API. This API identifies individual users and maps corresponding analytical data. (AppunfoldSDK must initialised before you call this method.)

		AppunfoldSDK.getUser()
      			.setId("EMP124")
       			.setEmail("arnold@appunfold.com")
      			.setName("Arnold")
      			.addParam("Role", “DELIVERY”)
      			.addParam("Location", “Bangalore”)
			.save();

Note: All the parameters are optional. If you need to add more parameters use addParam("key",“value”)

Quiz  Integration
	Include the AppunfoldQuiz SDK in your build.gradle.
		dependencies {
    compile 'com.appunfold:quiz:1.0.1'
}

	For launching quiz from your app, add the line of code mentioned below
			AppunfoldQuiz.launch(context);
Proguard Configuration
For releasing your app in production, If Proguard is enabled in your app, include the following in the app's proguard-rules.pro

              -keep class com.appunfold.**

Troubleshooting
Dependency on RecyclerView and CardView
Appunfold SDK depends on support library of RecyclerView and CardView with version 23.4.0 and higher. Normally, this is automatically taken care by Android Studio and Manifest Merger, but for apps that have dependency on older support libraries, make sure that your support library dependencies are updated to 23.4.0 or greater.
dependencies {
   ...

   compile 'com.android.support:cardview-v7:23.4.0'
   compile 'com.android.support:recyclerview-v7:23.4.0'
   compile 'com.appunfold:sdk:1.0.6'

   ...
}

	





