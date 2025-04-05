# <img src="https://github.com/HijelHub/Stationeers_Data_Logger/blob/f91b9f7929f572897b17f0cdf473b1e4f52e5ca3/Assets/small_ic.png"> Stationeers Data Logger
Stationeers Data Logger is a Windows Application that is used with the Rocketwerkz game [Stationeers](https://store.steampowered.com/app/544550/Stationeers/). It allows you to collect and log in-game IC10 Stack variables and convert them to csv format for later visualization using any spreadsheet app.
 
Supports:

<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/blue/microsoft.svg"> Windows 11

<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/blue/windows.svg"> Windows 10 64 Bit [Should Work - Untested]

<br>

[<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/blue/cloud-download.svg"> Installer Download](https://)
<hr>

### Contents

[<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/orange/joystick.svg"> In-Game Setup](#-in-game-setup)

[<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/green/window.svg"> Using The App](#--using-the-app)

[<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/red/file-earmark-text.svg"> Custom IC10](#--custom-ic10)


&nbsp;
 
 


## <img width="30" height="30" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/orange/joystick.svg"> In-Game Setup

**There are of course, multiple ways to set this up**  but I would suggest using this "Process Housing-DataLogger Housing" setup to get familiar with the system before writing your own scripts.
You can have as many Process Housings and DataLogger Housings as you wish, and they can be named whatever you wish.

### **Process Housing(s)**
In an existing housing (one in which contains some variables you want to log) add the following "DATALOGGER" IC10  function to your code. The purpose of the function is to load variables into registers, reset the stack pointer to 0 (so that each time it writes data it is always overwritten), and then write the variables to the stack. 

    DATALOGGER:
      l r0 StorageTank Pressure #or some other variable
      l r1 StorageTank Temperature #or some other variable
      # add more here
      move sp 0 #reset stack pointer
      push r0
      push r1
      #add more here 
    j ra

Call this function from within your main loop, and within any subsequent loops so that it runs and updates constantly.

&nbsp;

### **DataLogger Housing(s)**
There are two types of DataLogger Housings, Master and Slaves. The Master housing generates an iteration counter and controls the timing of actual Stack writes for itself and any connected Slaves. The Slaves connect in series to get the iteration counter they require from the master and they write data to their stack memory only when the iteration counter changes.

They are setup this way so that all housings remain in sync with each other and log data at the same time.

The number of DataLogger Housings you will need is determined by how many variables you want to log, how often, and what your games AutoSave Setting is set to.

If you only require one DataLogger Housing, it should be setup as a Master. If you need more than one, use a Master and then as many Slaves as you need.

> <img width="20" height="20"
> src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/red/exclamation-circle.svg"> Warning Do NOT Alias all DataLogger Slave Housings to the DataLogger Master Housing, if you have 3 DataLogger Housings then Slave2 would Alias to Slave1 and Slave1 would Alias to Master. 
> If you try to Alias all Slave Housings directly to the Master Housing they will go out of sync and you will lose data.

<hr>

Build a IC10 Housing and Chip and add them to the same network as your Process Housing(s). Load the example DataLogger Master program onto the chip and modify it for your needs. (Found in the IC10 Folder of this repository or on the [Steam Workshop](https://steamcommunity.com/profiles/76561198000943434/myworkshopfiles/))

 - Edit the LoggingInterval and VariableCount to reflect your desired settings
 - Edit the Variables you want to get from the Process Housing
 - Don't Forget to screwdriver the housing to change the value of d0 to reflect your process housing.
 
[//]: # (Terminator)

If you also need to use Slave Housings, follow the same procedure. (While obviously loading and modifying the Slave program instead of the Master program)

> <img width="20" height="20"
> src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/green/info-circle.svg"> If you want to download and use the Master and Slave IC10 Programs from this repository you will need to add them to your Game Save Scripts folder. The default location of this folder is:
>  C:\Users\[USERNAME]\Documents\My Games\Stationeers\scripts
>  Create a folder for each script, and place the script inside it. When you load your game they should show up in the LIBRARY section of the laptop or computer script editing screen.


  <br>  

[[TOP]](#contents)

<br>


## <img width="30" height="30" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/green/window.svg">  Using The App

Using the App is fairly straight forward.

 1. After Installing the App, open the calculator to figure out how many DataLogger Housings you will need.
 2. Go In-Game and setup the required housings and SAVE your game.
 3. In the App, Click the refresh button (or re-launch the app) and Select your desired Game Save from the dropdown located in [World Selection]
 4. Select a DataLogger Housing in the [Housings] column and click the Arrow button (or just double click)
 5. Repeat this step to add all of your required DataLogger Housings
 6. Select a Housing in the [Selected Housings] List
 7. If the Housing is recognized as a DataLogger Housing the header will turn green and the [Data Points] Interface will be displayed. If it does not recognize the Housing as a DataLogger Housing the header will turn red and you will get a warning message.
 8. Add your desired Data Points to the [Data Points To Log] list by selecting the Item and clicking the Arrow button (or just double click)
 9. You will be asked to name the Data Point, and this name will be shown when you open the csv file in your spreadsheet app.
 10. Click [Start Recording] and choose a file save location. The App will then start monitoring your Game Save file and when there is a change it will go in and parse the new data for you and append the new data to the existing .csv file.

If there is a missing DataPoint in a Housings Stack Variable set, then the App will replace it with "NAN". If you are seeing a lot of "NAN" data points in your data it most likely means you have something configured wrong.

  <br>  

[[TOP]](#contents)

<br>


## <img width="30" height="30" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/red/file-earmark-text.svg">  Custom IC10

As stated before, there are many ways to do this besides using the provided "Process Housing-DataLogger Housing" example.

If you want to come up with your own system and scripts, you will need to follow these guidlines in order for the App to recognize a Housing as a DataLogger Housing.

A DataLogger Housing **MUST** have:

<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/gray/arrow-return-right.svg"> These two constants defined and initialized at the start of your script

    define LoggingInterval [some Decimal/Integer value]
    define VariableCount [some Integer value]

The names must be exactly as shown and must have a value within the defined limits (see the calculator function in the app for limits)


<br>

<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/gray/arrow-return-right.svg"> A way to initialize and keep track of an iterative counter. The iteration counter must be at the start of every data packet.

    alias IterationCounter r15 [or whatever register you want]
    move IterationCounter 0
     
    YOURMAINLOOP:
     add IterationCounter IterationCounter 1 [add 1 to the counter]
     [GETYOURDATA]
     [WRITEYOURDATATOSTACK]
     [WAITFORNEXTLOGGINGINTERVAL]
    j YOURMAINLOOP


<br>


<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/gray/arrow-return-right.svg"> In addition to a function that Gets the data you want, and Writes the data you want to the stack, you need a function that checks to see if the data you are about to write will actually fit on the stack and if not it needs to write it to the beginning of the stack instead.

    CHECKSTACK:
      # check to see if next data packet will fit on the stack, if not set sp to 0
      add r13 VariableCount 1
      add r12 sp r13
      blt r12 511 ra
      move sp 0
    j ra


<br>

<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/gray/arrow-return-right.svg"> You will need a function that sleeps for [LoggingInterval] seconds. I did not use SLEEP because I was never able to get it to sleep for fractions of a second. It seems like the SLEEP function only accepts an Integer as a parameter, although I could be wrong.

       WAITSLEEP:
          #not using Sleep function to allow sub second waits
          sub TimeSleep TimeSleep 1
          yield
          brgtz TimeSleep -2
          mul TimeSleep LoggingInterval 2
        j ra



<br>

<img width="20" height="20" src="https://raw.githubusercontent.com/HijelHub/GitStrap_SVG_Icons/b674246b8f46d8bc2c75f3cf5cf395a370b86ae2/icons/gray/arrow-return-right.svg"> You will also need a scheme to pass timing and current iteration count information onto any other DataLogger housings running on the same system so they stay synchronized. I chose to do this through the housings db Setting, and then monitored this number in all subsequent housings in series. 


  <br>  

[[TOP]](#contents)

<br>
