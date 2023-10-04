# Metasploit - 4.5



[https://www.sans.org/blog/sans-cheat-sheet-python-3/?msc=Cheat%20Sheet%20Blog](https://www.sans.org/blog/sans-cheat-sheet-python-3/?msc=Cheat%20Sheet%20Blog)

![](<../../../.gitbook/assets/image (113).png>)

[https://cheatography.com/huntereight/cheat-sheets/metasploit-4-5-0-dev-15713/](https://cheatography.com/huntereight/cheat-sheets/metasploit-4-5-0-dev-15713/)

## Database Commands <a href="#title_560_2083" id="title_560_2083"></a>

| **Connect**                   | db\_connect     |
| ----------------------------- | --------------- |
| **Disconnect**                | db\_dis­connect |
| **Export Database**           | db\_export      |
| **Import Scan Result**        | db\_import      |
| **Status of Database**        | db\_status      |
| **Display Hosts**             | hosts           |
| **Display Loot**              | loot            |
| **Display Notes**             | notes           |
| **Display Services**          | services        |
| **Display Vulner­abi­lities** | vulns           |
| **Switch Between Workspaces** | workspace       |
| **NMAP Scan into Database**   | db\_nmap        |



## Core Commands <a href="#title_560_2084" id="title_560_2084"></a>

| **Display Help**                                                       | ? or help |
| ---------------------------------------------------------------------- | --------- |
| **Go Back**                                                            | back      |
| **Change Directory**                                                   | cd        |
| **Toggle Color**                                                       | color     |
| **Commun­icate with a Host**                                           | connect   |
| **Exit Metasploit**                                                    | exit      |
| **Display Info of Module**                                             | info      |
| **Go into irb**                                                        | irb       |
| **Display and Manage Jobs**                                            | jobs      |
| **Stop a Job**                                                         | kill      |
| **Load a Plugin**                                                      | load      |
| **Load a Plugin from Path**                                            | loadpath  |
| **Print Commands Entered to a Path**                                   | makerc    |
| **Set Previous Module as Current Module**                              | previous  |
| **Pops the Latest Module Off of the Module Stack and Makes it Active** | popm      |
| **Pushes the Active or List of Modules onto the Module Stack**         | pushm     |
| **Quit the Console**                                                   | quit      |
| **Run Commands Stored in a File**                                      | resource  |
| **Route Traffic Through a Connection**                                 | route     |
| **Save Datastores**                                                    | save      |
| **Search for Modules**                                                 | search    |
| **Dump Session Listings and Display Inform­ation about Sessions**      | sessions  |
| **Set Variable of a Module**                                           | set       |
| **Set a Global Variable**                                              | setg      |
| **Display Modules of a Type, or All Modules**                          | show      |
| **Do Nothing for X Seconds**                                           | sleep     |
| **Write All Output to a Files**                                        | spool     |
| **Manipulate Threads**                                                 | threads   |
| **Unload a Plugin**                                                    | unload    |
| **Unset a Variable**                                                   | unset     |
| **Unset a Global Variable**                                            | unsetg    |
| **Use a Module (by Name)**                                             | use       |
| **Show Metasploit Info**                                               | version   |

## Meterp­reter Core and File System <a href="#title_560_2088" id="title_560_2088"></a>

| **Background the Current Session**                | background                      |
| ------------------------------------------------- | ------------------------------- |
| **Kill a Background Meterp­reter Script**         | bgkill                          |
| **Displays Info About Active Channels**           | channel                         |
| **Close a Channel**                               | close                           |
| **Disables Encoding of Unicode Strings**          | disabl­e\_u­nic­ode­\_en­coding |
| **Enable Encoding of Unicode Strings**            | enable­\_un­ico­de\_­enc­oding  |
| **Exit Meterp­reter Shell**                       | exit                            |
| **Display Help**                                  | help                            |
| **Display Info About Active Post Module**         | info                            |
| **Interact with a Channel**                       | interact                        |
| **Drop into irb Scripting Mode**                  | irb                             |
| **Load One or More Meterp­reter Extensions**      | load                            |
| **Migrate the Server to Another Process**         | migrate                         |
| **Terminate the Meterp­reter Sessions**           | quit                            |
| **Reads Data from a Channel**                     | read                            |
| **Run the Commands Stored in a File**             | resource                        |
| **Executes a Meterp­reter Script or Post Module** | run                             |
| **Write Data to a Channel**                       | write                           |
| **Read the Contents of a File to the Screen**     | cat                             |
| **Change Directory**                              | cd                              |
| **Download File to Your Computer**                | download                        |
| **Edit a File**                                   | edit                            |
| **Print Local Working Directory**                 | getlwd                          |
| **Print Working Directory**                       | getwd                           |
| **Change Local Working Directory**                | lcd                             |
| **Print Local Working Directory**                 | lpwd                            |
| **List Files**                                    | ls                              |
| **Make Directory**                                | mkdir                           |
| **Print Working Directory**                       | pwd                             |
| **Delete the Specified File**                     | rm                              |
| **Remove Directory**                              | rmdir                           |
| **Search for Files**                              | search                          |
| **Upload File to Target**                         | upload                          |

## Meterp­reter User Interface Commands <a href="#title_560_2091" id="title_560_2091"></a>

| **List All Accessible Desktops and Window Stations**  | enumde­sktops   |
| ----------------------------------------------------- | --------------- |
| **Get the Current Meterp­reter Desktop**              | getdesktop      |
| **Display the Amount of Time the User has been Idle** | idletime        |
| **Start Capturing Keystrokes**                        | keysca­n\_start |
| **Stop Capturing Keystrokes**                         | keysca­n\_stop  |
| **Dump the Keystroke Buffer**                         | keysca­n\_dump  |
| **Screenshot of the GUI**                             | screenshot      |
| **Change the Meterp­reters Current Desktop**          | setdesktop      |
| **Control Some of the User Interface Components**     | uictl           |

## Meterp­reter System Commands <a href="#title_560_2090" id="title_560_2090"></a>

| **Clear the Event Log**                                               | clearev       |
| --------------------------------------------------------------------- | ------------- |
| **Relinq­uishes Any Active Impers­onation Token**                     | drop\_token   |
| **Execute a Command**                                                 | execute       |
| **Get the Current Process Identifier**                                | getpid        |
| **Attempt to Enable All Privileges Available to the Current Process** | getprivs      |
| **Get the User that the Server is Running as**                        | getuid        |
| **Terminate a Process**                                               | kill          |
| **List Running Processes**                                            | ps            |
| **Reboots the Remote Computer**                                       | reboot        |
| **Interact with the Remote Registry**                                 | reg           |
| **Calls Revert­ToS­elf() on the Remote Machine**                      | rev2self      |
| **Drop into a System Command Shell**                                  | shell         |
| **Shuts Down the Remote Computer**                                    | shutdown      |
| **Attempt to Steal an Impers­onation Token from the Process**         | steal\_­token |
| **Gets Inform­ation About the Remote System**                         | sysinfo       |



## Meterp­reter Priv Commands <a href="#title_560_2092" id="title_560_2092"></a>

| **List Webcams**                                               | webcam­\_list |
| -------------------------------------------------------------- | ------------- |
| **Take a Snapshot from the Specified Webcam**                  | webcam­\_snap |
| **Attempt to Elevate your Priviledge to that of Local System** | getsystem     |
| **Dumps the Contents of the SAM Database**                     | hashdump      |
| **Manipulate MACE Attributes**                                 | timestomp     |

