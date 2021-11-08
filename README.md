# Prepare
On the Raspberry Pi install jinput:

    sudo apt install libjinput-java libjinput-jni

To test the gamepad, install jstest:

    sudo apt jstest-gtk
    jstest-gtk

# Build

    cd JinputTest
    mvn clean package

# Running
## Maven
You can run the app as follows after building:

    mvn exec:exec

On the Raspberry Pi this will crash as follows:

    OpenJDK Server VM warning: You have loaded library /home/pi/src/JinputTest/target/lib/libjinput-linux64.so which might have disabled stack guard. The VM will try to fix the stack guard now.
    It's highly recommended that you fix the library with 'execstack -c <libfile>', or link it with '-z noexecstack'.
    java.lang.UnsatisfiedLinkError: /home/pi/src/JinputTest/target/lib/libjinput-linux64.so: /home/pi/src/JinputTest/target/lib/libjinput-linux64.so: wrong ELF class: ELFCLASS64 (Possible cause: architecture word width mismatch)
        at java.base/java.lang.ClassLoader$NativeLibrary.load0(Native Method)
        at java.base/java.lang.ClassLoader$NativeLibrary.load(ClassLoader.java:2442)
        at java.base/java.lang.ClassLoader$NativeLibrary.loadLibrary(ClassLoader.java:2498)
        at java.base/java.lang.ClassLoader.loadLibrary0(ClassLoader.java:2694)
        at java.base/java.lang.ClassLoader.loadLibrary(ClassLoader.java:2659)
        at java.base/java.lang.Runtime.loadLibrary0(Runtime.java:830)
        at java.base/java.lang.System.loadLibrary(System.java:1873)
        at net.java.games.input.LinuxEnvironmentPlugin.lambda$loadLibrary$0(LinuxEnvironmentPlugin.java:67)
        at java.base/java.security.AccessController.doPrivileged(Native Method)
        at net.java.games.input.LinuxEnvironmentPlugin.loadLibrary(LinuxEnvironmentPlugin.java:61)
        at net.java.games.input.LinuxEnvironmentPlugin.<clinit>(LinuxEnvironmentPlugin.java:93)
    Found no controllers.
        at java.base/java.lang.Class.forName0(Native Method)
        at java.base/java.lang.Class.forName(Class.java:315)
        at net.java.games.input.DefaultControllerEnvironment.getControllers(DefaultControllerEnvironment.java:133)
        at org.example.ReadAllEvents.<init>(ReadAllEvents.java:22)
        at org.example.ReadAllEvents.main(ReadAllEvents.java:90)

This is because Jinput doesn't properly know of the ARM 32 bit platform. A workaround is as follows:

    cp /lib/jni/libjinput.so target/lib/libjinput-linux64.so

Now you can run the jinput application:

    mvn exec:exec

## Non-Maven
To start the application without maven one would need to set the classpath properly:

    java -cp JinputTest-1.0-SNAPSHOT.jar:/usr/share/java/jinput.jar org.example.ReadAllEvents

Using the installed jinput, solves the JNI issue, but this only works on Linux systems.

# Resources
https://stackoverflow.com/questions/20851911/jinput-on-raspberrypi
https://github.com/jinput/jinput
https://github.com/jinput/jinput/wiki/Getting-started-with-JInput
https://jinput.github.io/jinput/
