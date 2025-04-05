# Stationeers Data Logger
Stationeers Data Logger is a Windows Application that is used with the Rocketwerkz game [Stationeers](https://store.steampowered.com/app/544550/Stationeers/). It allows you to collect and log in-game IC10 Stack variables and convert them to csv format for later visualization using any spreadsheet app.
 
Supports:
<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/blue/microsoft.svg"> Windows 11
<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/blue/windows.svg"> Windows 10 64 Bit [Should Work - Untested]

-------------

### Contents
<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/orange/joystick.svg"> In-Game Setup
<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/green/window.svg"> Using The App
<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/red/file-earmark-text.svg"> Custom IC10

&nbsp;
 
 


## <img width="30" height="30" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/orange/joystick.svg"> In-Game Setup

**There are of course, multiple ways to set this up**  but I would suggest using this "Process Housing-DataLogger Housing" setup to get familiar with the system before writing your own scripts.
You can have as many Process Housings and DataLogger Housings as you wish, and they can be named whatever you wish.

**Process Housing(s)**
In an existing housing (one in which contains some variables you want to log) add the following "DATALOGGER" IC10  function to your code. The purpose of the function is to load variables into registers, reset the stack pointer to 0 (so that each time it writes data it is always overwritten), and then write the variables to the stack. 

    DATALOGGER:
      l r0 StorageTank Pressure #or some other variable
      l r1 StorageTank Pressure #or some other variable
      # add more here
      move sp 0 #reset stack pointer
      push r0
      push r1
      #add more here 
    j ra

Call this function from within your main loop, and within any subsequent loops so that it runs and updates constantly.

&nbsp;

**DataLogger Housing(s)**
There are two types of DataLogger Housings, Master and Slaves. The Master housing generates an iteration counter and controls the timing of actual Stack writes. The Slaves connect in series to get the iteration counter they require from the master and they write data to their stack memory only when the iteration counter changes.

They are setup this way so that all housings remain in sync. with each other and log data at the same time.

Build a IC10 Housing and Chip and add them to the same network as your Process Housing(s). Load the example DataLogger Master program onto the chip and modify it for your needs. (Found in the IC10 Folder of this repository or on the [Steam Workshop](https://steamcommunity.com/profiles/76561198000943434/myworkshopfiles/))

 - Change the alias of d0 to reflect the name of your process housing (Don't forget to screwdriver it)
 - Edit the LoggingInterval and VariableCount to reflect your desired settings
 - Edit the Variables you want to get from the Process Housing
 
[//]: # (Terminator)


    

[[TOP]](#Stationeers-Data-Logger)
