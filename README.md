# seamless-sunshine
Seamless Sunshine is basically a mix of a few tools to turn your Windows Machine into the best possible Sunshine Streaming for a steam deck. This is accomplished by creating hotkeys that automatically set up a virtual third monitor and disable your main monitors, and creating other hotkeys that reset your setup back to normal. Much of this is easily attributable to other devices besides the Steam Deck if you know how to set up a remote streaming application (e.g. [Moonlight](https://github.com/moonlight-stream)) and know the device's resolution and frame rate. 

## Tutorial
### Prerequisites
1. Make sure PowerShell is installed.
2. Make sure that your user is able to execute PowerShell scripts via the PowerShell Execution Policy.
   To do this, run `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` in an Administrator PowerShell while logged in as the user that you intend to use Sunshine with.
3. Make sure [AutoHotKey](https://www.autohotkey.com/) is installed.

### Install Sunshine
Firstly, download and install [Sunshine](https://github.com/LizardByte/Sunshine) and make sure it is installed and running. [This](https://www.youtube.com/watch?v=Wb8j8Ojd4YQ) is a good tutorial to get Sunshine set up on your Windows machine.

### Install Moonlight
Secondly, install [Moonlight](https://github.com/moonlight-stream) on your Steam Deck. [Here](https://youtu.be/SewuUleDVug?si=4Pi-u2O48CuU6h6e&t=298) is a good tutorial to get Moonlight set up on your Steam Deck. Make sure that you are able to connect to your computer using Sunshine.

### Install Virtual Display Driver to your Windows System.
[Here](https://www.youtube.com/watch?v=byfBWDnToYk) is a tutorial to install [Virtual Display Driver](https://github.com/itsmikethetech/Virtual-Display-Driver) on Windows 10/11.

After extracting the **IddSampleDriver** folder, make sure that you edit the `option.txt` file and add a line that says `1280, 800, 60` if you have a Steam Deck LCD, and `1280, 800, 90` to match the Steam Deck OLED's configuration.
![image](https://github.com/user-attachments/assets/d3a14dfb-f610-4223-945a-46ac756414bc)

After that, go along with the Virtual Display driver install as normal for your system.

If everything is done correctly, you should have an extra monitor with a 1280x800 resolution.
![image](https://github.com/user-attachments/assets/dc6adb5a-fcd8-4d56-a082-1e3a66332583)


### Configure Sunshine for your resolution and frame rate.
Next, go into your Sunshine management console (likely at [https://localhost:47990/](https://localhost:47990/), and log in and go to **Configuration > Audio/Video** and scroll to the bottom.

Make sure your settings in Sunshine match your Steam Deck's resolution/display:

![image](https://github.com/user-attachments/assets/23162299-4fc7-4365-82c6-2f62633b7a49)

### Install DisplayConfig
The next step is to install [DisplayConfig](https://www.powershellgallery.com/packages/DisplayConfig/2.0), a PowerShell package that allows you to control your display state.

Install this by opening up a PowerShell or Terminal as Administrator, then type in `Install-Module -Name DisplayConfig` and hit Enter.

### Create your display configurations
Create a new directory for all of your configs that will be used to control your display configuration. in this example, I will be using `C:\display-cfgs`, but you can realistically store these scripts anywhere if you know how to.

Open up a PowerShell/Terminal and run the following commands, along with doing the following steps:
1. Create a display configs directory:
  `mkdir 'C:\display-configs'`
3. Set your monitors to your "Default Configuration", which is essentially your regular monitor configuration when you are not using Sunshine/Moonlight. This can be done by going into **Settings > System > Display** and disabling your Virtual Monitor, and making sure the desired monitor is set as your "Main Display".

   For example, my "Monitor 3" (the Virtual Monitor) is set to "disconnect this display"
   ![image](https://github.com/user-attachments/assets/52f87bda-378b-410d-947f-e5f16438d8ad)
   My "Monitor 1" (my primary physical monitor) is set to "extend this display" and is set as my main monitor
   ![image](https://github.com/user-attachments/assets/9edbd817-84c9-4ba3-8712-a6fc1afa52b1)
   And my "Monitor 2" (my secondary physical monitor) is set to "extend this display"
   ![image](https://github.com/user-attachments/assets/c3e4aa38-d3c1-48b1-bed8-03b537bf681e)

4. Create your monitor profile for the Default Configuration by using the following command:
  `Get-DisplayConfig | Export-Clixml C:\display-configs\default-profile.xml`
5. Set your monitors to your "Sunshine Configuration". You can do this by disabling all of your phyiscal monitors, and enabling your virtual display and making it your main display. **NOTE**: You will need to connect to your PC with your Steam Deck during this step to be able to see the disabled monitor.

  For example, my Monitor 3 (The Virtual Monitor) is set to "extend this display"
  ![image](https://github.com/user-attachments/assets/4b4a2748-67dd-4648-ac28-0a23a7e0ac5f)

  And all other monitors are set to "Disconnect this display"
  ![image](https://github.com/user-attachments/assets/17c74cf2-5a24-4333-8a4f-ff856231b559)

6.  Create your monitor profile for the Sunshine configuration by using the following command:
  `Get-DisplayConfig | Export-Clixml C:\display-configs\sunshine-profile.xml`

7. Manually reset your monitors back to your Default Configuration, so that you can use your normal displays for the rest of the setup.

### Create your Autohotkey scripts
**NOTE: this tutorial is for AutoHotkey 2.0. If you are using Autohotkey 1.x, translate the scripts it by reading their documentation (or just ask ChatGPT to do it for you).**
1. Open up **Run** by hitting `Win + R`. Type in `shell:startup` in the textbox and hit `Enter`. 

![image](https://github.com/user-attachments/assets/75109ed0-a429-4a88-87e5-d2668b8cbc22)

2. Right click in the folder and click **New > AutoHotkey Script**.
![image](https://github.com/user-attachments/assets/853b83af-3746-4590-99f2-b3a80166b8b6)

3. Name the file `default-cfg`, select `Empty` and click `edit`.
5. Put the following into this file:
`
^!d:: {  ; Ctrl + Alt + D hotkey
    psCommand := "Import-Clixml C:\display-configs\default-cfg.xml | Use-DisplayConfig -UpdateAdapterIds"
    Run('powershell.exe -Command "' psCommand '"',, 'Hide') 
}
`
You can edit the key combination to anything you like. Refer to the [AutoHotkey documentation](https://www.autohotkey.com/docs/v2/).
6. Save the file and close the editor.
7. In File Explorer, double click `default-cfg.ahk` to allow the Hotkey script to be ran.
8. Repeat steps 2-7 but with the following filename: `sunshine-cfg` and the following script text:
`
^!s:: {  ; Ctrl + Alt + S hotkey
    psCommand := "Import-Clixml C:\display-configs\sunshine-cfg.xml | Use-DisplayConfig -UpdateAdapterIds"
    Run('powershell.exe -Command "' psCommand '"',, 'Hide') 
}
`

## Result
If you followed all the instructions correctly, you should be able to use your shortcut keys (`Ctrl + Alt + D` for default and `Ctrl + Alt + S` for Sunshine by default) at any time to switch between your Default and Sunshine display configurations.


## Troubleshooting
There is a chance that plugging in a certain monitor combination will cause your computer to be difficult to use due to certain monitors being disabled.
To return to your computer's default monitor configurations, do the following:
1. Open up **Run** by hitting `Win + R`. Type in `regedit` in the textbox and hit `Enter`.

![image](https://github.com/user-attachments/assets/99a5c2e0-32a0-45af-adb4-0d96a9b58200)

2. Navigate to `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GraphicsDrivers\Configuration`
![image](https://github.com/user-attachments/assets/44d3a46e-cf09-49bd-93ab-26071a564b37)

3. Delete each one of the folders under `Configuration`.
![image](https://github.com/user-attachments/assets/637248ac-f078-4407-95fb-185474aa7983)

This will reset your display configurations.

4. Restart your computer and repeat all of the steps in the **Create your display configurations** section.


## Credits
Thanks to [MikeTheTech](https://www.youtube.com/channel/UCZ0hznff92f_i1wpYyi7tPQ/join) for his work on the Virtual Display Drivers and for his Sunshine/Moonlight tutorials.

Thanks to everyone credited in the [Sunshine](https://github.com/LizardByte/Sunshine) and [Moonlight](https://github.com/moonlight-stream) repositories. 

Thanks to [Steam Deck Guy](https://www.youtube.com/@steamdeckguy) for the Moonlight install tutorial.

Thanks to [MartinGC94](https://github.com/MartinGC94) for his work on [DisplayConfig](https://github.com/MartinGC94/DisplayConfig).

Thanks to [AutoHotkey](https://www.autohotkey.com/).
