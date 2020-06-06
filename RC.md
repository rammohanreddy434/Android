The init file is a key component of the Android boot sequence. It is a program to initialize the elements of the Android system.  Unlike Linux, Android uses its own initialization program.

This Android init program processes 2 files, and it executes the commands it finds in both programs. These programs are: ‘init.rc’ and ‘init<machine name>.rc’ (this machine name is the name of the hardware that Android is running on).

 What does each program contain?:
* init.rc provides the generic initialization instructions
* init<machine name>.rc provides specific initialization instructions


What is the syntax of these .rc files?
The android init language consists of 4 classes of statements:
`Actions, Commands, Services and Options.`

Actions and Services declare new sections. All the commands or options belong to the section most recently declared.

Actions and Services have to have unique names. If a second Action or Service has the same name of a previous one, it is ignored.

 ### Action
Actions are sequences of commands. They have a trigger which is used to determine when the action should occur.

Actions take following form.
```
On <trigger>
<command>
<command>
<command>…
```

### Services
Services are programs which init launches and (optionally) restart when it exists

Services take the following form.
```
Service <name> <patchname>  [argument]
<option>
<option>…
```

### Options
Options are modifiers to services. These affect how and when init runs a service.

**Critical**<br/>
This is a device-critical service. If it exits more than four times in four minutes, the device will reboor into recovery mode.

 

**Disabled**<br/>
This service will not automatically start with its class. It must be explicitly started by name.

 

**Setenv <name> <value>**<br/>
Set the environment variable <name> to <value> in the launched process.

 

**User <username>**<br/>
Change to username before executing this service. Currently defaults to root.

 

**Group <groupname> [<groupname>]**<br/>
Change to groupname before executing this service.

 

**Oneshot**<br/>
Do not restart the service when it exists

 

**Class <name>**<br/>
Specify a class name for the service. All services in a named class may be started or stopped together.

 

**Onrestart**<br/>
Execute a command when service restarts

 

### Triggers
Triggers are strings which can be used to match certain kinds of events and used to cause an action to occur.



**Boot**<br/>
This is the first trigger that will occur when init starts (after /init.conf is loaded)

 

**Commands:**<br/>

 

**Exec <path> [<arguments>]**<br/>
Fork and execute a program (<path>). This will block until the program completes execution.

 

**Export <name> <value>**<br/>
Set the environment variable <name> equal to <value> in the global environment.
Ifup <interface>**<br/>
Bring the network interface <interface> online.

 

**Import <filename>**<br/>
Parse and init config file, extending the current configuration.

 

**Hostname <name>**<br/>
Set the host name

 

**Chdir <directory>**<br/>
Change working directory

 

**Chmod <octal-dmoe> <path>**<br/>
Change file access permissions

**Chwon <owner> <group> <path>**<br/>`
Change file owner and group

 

**Chroot <directory>**<br/>
Change process root directory

 

**Class_start <serviceclass>**<br/>
Start all services of the specified class if they are not already running.

 

**Class_stop <serviceclass>**<br/>
Stop all services of the specified class if they are currently running.

 

**Domainname <name>**<br/>
Set the domain name

 

**Enable <servicename>**<br/>
Turns a disabled service into an enabled one.

 

**Insmod <path>**<br/>
Install the module at <path>

 

**Mkdir <path>**<br/>
Create a directory at <path>

**Mount <type><device><dir>**<br/>
Attempt to mount the named device at the directory.

**Restorecon <path>**<br/>
Restore the file named by <path> to the security context specified in the file_contexts configuration.

 

**Setcon <securitycontext>**<br/>
set the current process security context to the specified string.

 

**Setenforce 0|1**<br/>
Set the SELinux system wide enforcing status. 0 = permissive. 1 = enforcing.

 

**Setprop <name><value>**<br/>
Set system property <name> to <value>

**Setrlimit <resource><cur><max>**<br/>
Set the rlimit for a resource

 

**Setsebool <name><value>**<br/>
Set SELinux Boolean <name> to <value>

 

**Start <service>**<br/>
Start a service

 

**Stop <service>**<br/>
Stop a service

 

**Symlink <target><path>**<br/>
Create a symbolic link at <path> with the value <target>
Trigger <event>
Trigger an event. Used to queue an action from another action

 

**Wait <path>**<br/>
Poll for the existence of the given file and return when found.

 

**Write <path> <string>**<br/>
Open the file at <path> and write a string to it.


###Examples
How to run a script:

```
service my_service /data/test
  class main
  oneshot
```
Here we are declaring the service named 'my service' with location in /data/test. It belongs to the main class and will start along with any other service that belongs with that class and we declare that the service wont restart when it exits (oneshot).



**Change file access permissions:**<br/>

`chmod 0660   /sys/fs/cgroup/memory/tasks`

Here we are changing access permissions in path /sys/fs/cgroup/memory/tasks



**Write a string to a file in a path:**<br/>

`write  /sys/fs/cgroup/memory/memory/memory.move_charge_at_immigrate   1`



**Create a symbolic link:**<br/>

`symlink  /system/etc /etc`

Here we are creating a symbolic link to /system/etc -> /etc



**Set a specific density of the display:**<br/>

`setprop ro.sf.lcd_density 240

Here we are setting a system property of 240 to ro.sf.lcd_density



**Set your watchdog timer to 30 seconds:**<br/>
```
service watchdog /sbin/watchdogd 10 20

class core
```
We are petting the watchdog every 10 seconds to get a 20 second margin



**Change file owner:**<br/>
`chown root system /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq`

The new owner being 'root' from the group 'system'

Refer:
https://community.nxp.com/docs/DOC-102537 


 
