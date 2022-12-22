# Cohab_Processes
This Aggressor script is intended to help internal Red Teams identify suspicious or foreign processes ("Cohabitation") running in their environments.  

Red Teams may assemble a list of "known" processes (either independently or in collaboration with Blue force) and feed it to the aggressor script.  This list of processes might include those seen "at least x times in our network" in order to establish a baseline.  Once the script has been loaded into CobaltStrike, beacon output will be altered/color coded for the following commands:

1. shell tasklist
2. ps
3. [TrustedSec's tasklist BOF](https://github.com/trustedsec/CS-Situational-Awareness-BOF) (You do not have to use this BOF, but it is highly recommended)

Any processes returned as output from the above commands that are NOT found on the "known" processes list will be highlighted in RED for further investigation/scrutiny.

**shell tasklist:**

![image](https://user-images.githubusercontent.com/91164728/207487959-85b367ab-7d19-4567-92c7-3ef358122f79.png)

**ps:**

![image](https://user-images.githubusercontent.com/91164728/208021688-2be93bd7-4b7f-484b-a600-edcbcf166dc1.png)


**TrustedSec tasklist BOF:**

![image](https://user-images.githubusercontent.com/91164728/207488182-2722265a-f5cb-4562-bf6e-05819f8642d3.png)


## Setup
The list of known processes must be in a text file, one process per line.  
Edit line 3 in Cohab_Processes.cna and specify your list of "known" processes.  
Load the .cna script into CobaltStrike and enjoy.  

I have included 'knownprocesses.txt' as an example so that users can observe the behaviour of this script before going to the effort to assemble the list of "known" processes in their environment.

## TrustedSec Tasklist BOF
A small modification is required to TrustedSec's tasklist BOF in order to make it compatible with this script. I had to effectively add a "end of BOF" tag to the output in order for the Aggressor script to identify that the BOF was done sending output and to stop trying to read/color code things.

![image](https://user-images.githubusercontent.com/91164728/207489753-a7340b5c-da64-4c2f-8ea3-d68048ae08da.png)

I recommend grabbing the full package from their repo and then modifying the tasklist BOF (CS-Situational-Awareness-BOF/src/SA/tasklist/entry.c) to match the one I have included here.  This is necessary because the full repo contains some helper/header files that you will need to recompile entry.c.

## Limitations
This methodology for identifying potential cohabitation IS severely limited.  It will not for example identify or catch things like:

1. Process injection- where malware has been injected into and is running in a legitimate, "known" process.
2. DLL sideloading or hijacking- where malware has been loaded by a legitimate, "known" process by residing in a DLL that the process loads/runs.

This script serves as a way to help catch the "low hanging fruit".

## Credit

Thanks to TrustedSec for their great BOF repo  
Inspiration and some code snippets taken from harleyQu1nn's [ProcessColor.cna](https://github.com/harleyQu1nn/AggressorScripts/blob/master/ProcessColor.cna)
