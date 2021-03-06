# Cexec Task - Custom Execute

This is a simple executor, sometimes the default `exec` task does not work as expected.

I wrote this simple task just to trigger some terminal commands from a gradle task.
It may looks strange if you only know the basic gradle commands.

Try to imagine the following situations:
  * Run something before install the app on android. Like the `development backEnd App`, `mocked Api` or anything like thad.
  So we can use a task to do that for us.
  * Send a command to adb. I use it to clean app data.

I use the `cexec` task to do those tasks.

# How to use

## Add JitPack in your `build.gradle`.

```gradle
buildscript {
    repositories {
        maven { url "https://jitpack.io" }
    }
    dependencies {
        classpath 'com.github.brunodles:cexec:-SNAPSHOT'
    }
}
```

You can add this just before use, in your all level `projectPath/app/build.gradle`.
Or in your root level `projectPath/build.gradle`. Suggested if you plan to use it in more than one module.

## Create a Task

To Create a new task you just need to edit the code bellow.

```gradle
task name(type: com.github.brunodles.cexec.Cexec) {
    description "<say something>" // <- this will appear when you list tasks for your project
    command "<command>" // <- the terminal command you want to run
    printCommand <true|false> // <- true if you want to see the command printed before it's execution. Useful if you have some complex command with variables.
}
```

### Samples

This code just calls the `ifconfig`, to get information about the network

```gradle
task ifconfig(type: com.github.brunodles.cexec.Cexec) {
    description "Just print something on screen"
    command "ifconfig"
    printCommand false
}
```

This code calls adb to clean the app data.

```gradle
android.applicationVariants.all { variant ->
    task ("cleanAppData${variant.name.capitalize()}", type:com.github.brunodles.cexec.Cexec) {
        description "Clean app data on device"
        command "adb shell pm clear ${variant.applicationId}"
    }
}
```

### Shortening task type

If you have multiple tasks extending `Cexec`, you can do as follows.

In your `build.gradle` add the import line, before the first task, it can also be on the top of the file.
Now yu can put only `Cexec` for the task type.

 ```gradle
import com.github.brunodles.cexec.Cexec
task ifconfig(type: Cexec) {
 ```

# Contributing

You can contribute but showing your custom tasks, or submitting a new task.
Issues are welcome, create one and we will discourse about it.
If you saw any error, please reports, it will be a great help.

# Licence
You can use any code you found here, some of then I found on the internet too.

I'm using the MIT Licence, take a look on [Licence](LICENCE.md).

If you're using this plugin, please give me some credits too.

# Sources

## Gradle
* [Writing Custom Task Classes](https://docs.gradle.org/current/userguide/custom_tasks.html)
* [Writing Custom Plugins](https://docs.gradle.org/current/userguide/custom_plugins.html)

## Ip Inject
This one have many sources, I don't remember where I found it at first, it was on 2014.
Now we have many articles about it, so I'll link then.
* [Bartinger](http://bartinger.at/inject-dynamic-host-ip-address-with-gradle/)
* [jeremie-martinez](http://jeremie-martinez.com/2015/05/05/inject-host-gradle/)

## JitPack
* [JitPack](https://jitpack.io/)
* [Guide for Gradle Projects](https://jitpack.io/docs/BUILDING/#gradle-projects)
* [GradleModular](https://github.com/jitpack/gradle-modular)
