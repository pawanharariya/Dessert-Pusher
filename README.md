# Desset Pusher #
Dessert Pusher is a simple game, in which the user presses the desserts displayed on the screen to make them. For each dessert created, you get some points and you can share them. The purpose of this app is to learning about activity lifecyle, and how to keep track of the score during configuration changes.

This apps implements various activity lifecycle callbacks, and shows how they can be used for setup and cleanup via the use of a timer. It also uses Lifecycle Library so that the setup and clean up can be done by the timer class itself. Also, the save instance callback is implemented to save and restore data during configuration changes and process-related shutdown.

### App Preview ###
<img src="https://github.com/pawanharariya/Dessert-Pusher/assets/43620548/0a6c5909-976c-43d0-8035-d63d345f25f8" alt="App preview 1" width="250">
<img src="https://github.com/pawanharariya/Dessert-Pusher/assets/43620548/cdefd4d3-27f0-4e77-8324-3175da2fda8d" alt="App preview 2" height="250">

## Android Activity Lifecycle ##
Every activity has a lifecycle. It represents the different states the activity goes through, from being initialised, to being destroyed. Consider following cases : 

1. When we are using an app on our phone, it should run smoothly, but when the user navigates away from the app, the cpu and memory extensive tasks should be paused.
  
2. Meanwhile the android OS also watches the state of the activites and to improve battery life and app performance, it may kill the apps in background. So we need to program proactively so that we clean up unused resources, and defensively in case OS restarts the app. 

To handle such situations and manage the activities we need to know about the current state of our activity. 

## Lifecycle States ##
Lifecycle states are the different stages an acitivity goes through during its lifetime. These are the same for both the Fragment Lifecycle and the Activity Lifecycle. Following are the lifecycle states.

**Initialized** - This is the starting state whenever we make a new activity. This is a transient state and it immediately goes to Created.

**Created** - Activity has just been created, but it’s not visible and it doesn’t have focus (we’re not able to interact with it).

**Started** - Activity is visible but doesn’t have focus.

**Resumed** - The state of the activity when it is running. It’s visible and has focus.

**Destroyed** - Activity is destroyed. It can be ejected from memory at any point and should not be referenced or interacted with.


## Activity Lifecycle States ##
<img src="https://github.com/pawanharariya/Dessert-Pusher/assets/43620548/2686bd81-ee52-4ebc-b585-e695e03b1ea5" width="500" height="300" alt="Activity Lifecycle States">

## Activity Lifecycle Diagram ##
<img src="https://github.com/pawanharariya/Dessert-Pusher/assets/43620548/641a8b0c-b993-457f-884a-d0a64ecd8a5c" width="400" alt="Activity Lifecycle Diagram">

## Fragment Lifecycle Diagram ##
<img src="https://github.com/pawanharariya/Dessert-Pusher/assets/43620548/24522250-c568-4580-ad0c-2b529a1efd7c" width="400" alt="Fragment Lifecycle Diagram">


## Activity Lifecycle Callbacks ##
Lifecycle callbacks are the methods that are triggered when the activity reaches a state.

**onCreate**: This is called the first time the activity starts and is therefore only called once during the lifecycle of the activity. It represents when the activity is created and initialized. The activity is not yet visible and we can't interact with it. We must implement onCreate. In onCreate we should:

* Inflate the activity's UI, whether that's using findViewById or databinding.
* Initialize variables.
* Do any other initialization that only happens once during the activity lifetime.

**onStart**: This is triggered when the activity is about to become visible. It can be called multiple times as the user navigates away from the activity and then back. Examples of the user "navigating away" are when they go to the home screen, or to a new activity in the app. At this point, the activity is not interactive. In onStart we should:

* Start any sensors, animations or other procedures that need to start when the activity is visible.

**onResume**: This is triggered when the activity has focus and the user can interact with it. Here we should:

* Start any sensors, animations or other procedures that need to start when the activity has focus (the activity the user is currently interacting with).

**onPause**: The mirror method to onResume. This method is called as soon as the activity loses focus and the user can't interact with it. An activity can lose focus without fully disappearing from the screen (for example, when a dialog appears that partially obscures the activity). Here we should:

* Stop any sensors, animations or other procedures that should not run when the activity doesn't have focus and is partially obscured.
* We should make sure execution is fast as the next activity is not shown until this completes.

**onStop**: This is the mirror method to onStart. It is called when we can no longer see the activity. Here we should:

* Stop any sensor, animations or other procedures that should not run when the activity is not on screen.
* We can use this to persist (permanently save) data.
* Stop logic that updates the UI. This should not be running when the activity is off-screen, it's a waste of resources.
* There are also restrictions as soon as the app goes into the background, which is when all activities in our app are in the background. 

**onDestroy**: This is the mirror method to onCreate. It is called once when the activity is fully destroyed. This happens when we navigate back out of the activity (as in press the back button), or manually call finish(). It is the last chance to clean up resources associated with the activity. Here we should:

* Tear down or release any resources that are related to the activity and are not automatically released for us. Forgetting to do this could cause a memory leak! Logic that refers to the activity or attempts to update the UI after the activity has been destroyed could crash the app.

## Logging with Timber ##
Logs are messages shown on the console, which can be reviewed for debugging purposes, by looking at when they are trigerred and in which order.

Logs have different levels which are used in different situations. The levels and their usages are listed below:

* **Verbose**: Show all log messages (the default).

* **Debug**: Show debug log messages that are useful during development only, as well as the message levels lower in this list.

* **Info**: Show expected log messages for regular usage, as well as the message levels lower in this list.

* **Warn**: Show possible issues that are not yet errors, as well as the message levels lower in this list.

* **Error**: Show issues that have caused errors, as well as the message level lower in this list.

* **Assert**: Show issues that the developer expects should never happen.

Timber is popular logging library, which provides following benefits:
1. It automatically generates the tag names for logs based on class names, so we don't have to do it manually.
2. It avoids showing logs in released version of the app.
3. It allows easy integration with crash reporting libraries.

### Application Class ###
Application class is a base class that contains global application state for the entire app. It is also the main object the Android OS uses to interact with our app. It is useful when we want to do some specific setup for a utility that will be used by entire app. It is also used when we want to initialize something, before anything else is initialized, like logging library. 

By default there is an application class, that is created for our app. Hence we need to extend the base Application class to override and provide custom implementation.

Note : To setup Timber library, we need to extend the base Application class and add it to AndroidManifest.xml file.

## Playing with Activity Lifecycle ##
In this section, it is shown what activity lifecycle callbacks are triggered due to different actions perfomed on the app.

1. Opening the app and hitting back Button :  Opening the app results in `onCreate()`, `onStart()` and `onResume()` callbacks in order. Hitting back button results in `onPause()` and `onStop()` callbacks in order.

2. Removing app from recent apps screen : In this case, `onDestroy()` callback is triggered. `onPause()` and `onStop` are already called when the app went in background.

3. Opening app again, from recent apps screen : `onRestart()` , `onStart()` and `onResume()` are trigerred.

4. Opening the share dialog from `MainActivity` screen, and clicking outside to close it : When opening the share dialog `onPause()` is called, and when closing it `onResume()` is called. When the share dialog is open, the main activity is still on partially on screen and is considered in foreground. Hence instead of `onStop`, `onPause` gets trigerred.

5. Rotating the phone : On rotating the phone the activity is destroyed and recreated, hence associated callbacks are called.

6. Opening another activity : When opening a new activity first activity's `onPause()` is called. Then second's activity's `onCreate()`, `onStart()` and `onResume()` is called. And, only after that first activity's `onStop()` is called.

### onPause() vs onStop() ###
`onStart()` is called when activity is visible and `onStop()` when activity goes off screen. Hence they are related to visibility of the activity. `onResume()` and `onPause()` are related to focus. In focus means, we can interact with the activity, hence `onPause()` is called when activity loses focus, but is still visible.

### onCreate() vs onStart() vs onRestart() ###
`onCreate()` is called only once when the activity is first launched. Whereas, `onStart()` is called every time we come back to the activity after we have navigated away. `onRestart()` is similar to `onCreate()` either `onCreate()` or `onRestart()` will be called before activity becomes visible. `onCreate()` is called for the first time, but later `onRestart()` is called once the activity is already created. For example when we come back to the app from recent app's screen.

## Lifecycle Library ##
When we setup any resources, we must tear them down as well, like a timer. Also, the setup and tear down should happen in the mirrored callback methods, for example, if we start our timer in `onStart()`, we must stop it in `onStop()` and if we setup something in `onCreate()`, we should tear it down in `onDestroy()`.

This is easy when we have a single resource, and in case of multiple resources (like animations, sensors, timers and music) we would have to take care of setting up and removing each of the resource. To solve this, we use Lifecycle Library. Lifecylce Library allows the class that manages the resource take care of its own setup and tear down based on the lifecycle.

Lifecycle Library has Android Lifecycle as an actual object, which can be queried and checked for its state. It works on two components listed below:

**Lifecycle Owner** - A class that has a Lifecycle, eg. Activity and Fragment. We can take an activity, ask for its Lifecycle object and get its current state.

**Lifecycle Observer** - They can observe the lifecycle of Lifecycle Owners. eg a Timer class.

This is based on Observer Design Pattern, in which the subject keeps tracks of all its observers and the observers keep watching the events of the subject. When the state of the subject changes it notifies all its observers. 

We can add `@OnLifecyclerEvent` annotation to any method of the observer, which will be trigerred when thr lifecycle state of its owner changes.

## Android App Shutdown Process ##
The Android OS has to make sure that app currently in foreground runs smoothly, this is maintained by limiting the amount of processing, apps in background can do and in some cases, even shutting the apps in background to get resources. The shutdown kills every activity of the app, even without triggering callbacks. Android OS automatically tries to store some UI information and uses it, when the user navigates back and restarts the app. However to restore the exact status of activity, we need to save critical data ourself.

NOTE: Shutting down the backgroud app, doesn't remove it from recent apps screen. And when we go back to our app, its restarted.

## Handling App shutdown by OS ##
When the OS shutdowns the app, it tries to save data, so on restart is resets the app to its previous state. It does this by taking state of some of the views and saving them to a bundle, each time we navigate away from the activity. For example, the data in EditText or the activity's backstack. OS then uses the bundle to restore the activity back to same state. It is done each time when we navigate away, because in case of shutdown there won't be time to back up the data.

However, sometimes the OS doesn't know about the data, so we have to manually add data to the bundle using `onSaveInstanceState` callback.

NOTE: To save the text in our `EditText` in case of shutdown, we must have an `id` assigned to the `EditText`.

### How to kill the app manually ###
We can simulate the app shutdown, by killing our app process manually with adb command line tool, using the following command.
```
adb shell am kill <app-package-name>
```

## onSaveInstanceState callback ##
This callback is where we save data, that we may need in case Android OS destroys our app. It is called after `onStop()` when the activity is in backgroud. We save the data in the instanceState bundle, and can query the bundle in our `onCreate()`. There is a separate callback also, called the `onRestoreInstanceState()`, where we can get the data from bundle, however it is called after `onStart()`.

NOTE: We must save only small and limited data in instance bundle, as this bundle is in system's RAM and it has limited size. In case, we store large data, our app may crash with `TransactionTooLargeException`.

## Configuration Changes ##
Configuration change is a major change a device undergoes, that causes the activity to be recreated, like rotating the phone. It uses the `savedInstanceState` bundle to restore the activity. Other examples of configuration changes are : 
1. User changes device language
2. Plug in or remove hardware keyboard

This happens because the UI maybe completely different in these cases, as explained below:
1. In case of rotating screen, the UI can be different for portrait and landscape mode.
2. In case of language change, for RTL languages like Arabic, the whole UI and text will be different.
3. Attaching physical hardware provides space, that is otherwise occupied by softboard.
