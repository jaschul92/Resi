# Resi
Site to display my resume

## Testing for HW2
Since there was not much to test in reagards to this website I went ahead and ran some live testing on the actual project I am doing for the course. So far, our project had only been ran locolly on the simulator that xcode provides. The app runs smoothly and so I assumed everything would go the same for running it live on my phone. I went ahead and cofigured deployment, installed the app on my phone, clicked launch, tried to login and...crash. My enthusiasm was short lived. I had to do some research on how to debug apps that are installed on your phone. Since the app was running independent of the xcode encironment, there was no way to set breakpoints. After browsing the internet I came upon the ```diagnostics and usage``` tab in the ```privacy``` settings page. Within this tab is where I found the crash report. The full report can be found in the file ```testReport.txt```. This file had a lot to digest, it was very foreign at first. After walking through it I noticed the lines:
```
Exception Type:  EXC_BREAKPOINT (SIGTRAP)
Exception Codes: 0x0000000000000001, 0x0000000100847b54
Termination Signal: Trace/BPT trap: 5
Termination Reason: Namespace SIGNAL, Code 0x5
Terminating Process: exc handler [0]
Triggered by Thread:  0

Filtered syslog:
None found

Thread 0 name:  Dispatch queue: com.apple.main-thread
Thread 0 Crashed:
```
At this point I searched the internet with what is the cause of a ```SIGTARP``` exception with reason: ```Namespace SIGNAL, Code 0x5```. I came upon some apple developer documentation where I foud this:
```
Trace Trap [EXC_BREAKPOINT // SIGTRAP]
Similar to an Abnormal Exit, this exception is intended to give an attached debugger the chance to interrupt the process at a specific point in its execution. You can trigger this exception from your own code using the __builtin_trap() function. If no debugger is attached, the process is terminated and a crash report is generated.

Lower-level libraries (e.g, libdispatch) will trap the process upon encountering a fatal error. Additional information about the error can be found in the Additional Diagnostic Information section of the crash report, or in the device's console.

Swift code will terminate with this exception type if an unexpected condition is encountered at runtime such as:

a non-optional type with a nil value
a failed forced type conversion
```
This is where I got a scent of where the issue may lie... on the second view page a value was returning nil somewhere. From analyzing my code I found my bug. I was trying to load a label on the page with information stored in the database; however, that infromation wasn't there yet due to the asynchronous calls of db functions. I removed this function and it worked! Now I just need to find a way to rewrite this function...
