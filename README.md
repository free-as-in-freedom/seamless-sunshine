# seamless-sunshine
Seamless Sunshine is basically a mix of a few toold to turn your Windows Machine into the best possible Sunshine Streaming for a steam deck. This is accomplished by creating hotkeys that automatically set up a virtual third monitor and disable your main monitors, and creating other hotkeys that reset your setup back to normal. Much of this is easily attributable to other devices besides the Steam Deck if you know how to set up a remote streaming application (e.g. [Moonlight](https://github.com/moonlight-stream)) and know the device's resolution and framerate. 

## Tutorial
### Install Sunshine
Firstly, download and install [Sunshine](https://github.com/LizardByte/Sunshine) and make sure it is installed and running. [This](https://www.youtube.com/watch?v=Wb8j8Ojd4YQ) is a good tutorial to get Sunshine set up on your Windows machine.

### Install Moonlight
Secondly, install [Moonlight](https://github.com/moonlight-stream) on your Steam Deck. [Here](https://youtu.be/SewuUleDVug?si=4Pi-u2O48CuU6h6e&t=298) is a good tutorial to get Moonlight set up on your Steam Deck. Make sure that you are able to connect to your computer using Sunshine.

### Install Virtual Display Driver to your Windows System.
Here is a tutorial to install [Virtual Display Driver](https://www.youtube.com/watch?v=byfBWDnToYk) on Windows 10/11.

After extracting the **IddSampleDriver** folder, make sure that you edit the `option.txt` file and add a line that says `1280, 800, 60` if you have a Steam Deck LCD, and `1280, 800, 90` to match the Steam Deck OLED's configuration.
![image](https://github.com/user-attachments/assets/d3a14dfb-f610-4223-945a-46ac756414bc)

After that, go along with the Virtual Display driver install as normal for your system.

If everything is done correctly, you should have an extra monitor with a 1280x800 resolution.
![image](https://github.com/user-attachments/assets/dc6adb5a-fcd8-4d56-a082-1e3a66332583)


### Configure Sunshine for your resolution and framerate.
Next, go into your Sunshine management console (likely at [https://localhost:47990/](https://localhost:47990/), and log in and go to **Configuration > Audio/Video** and scroll to the bottom.

Make sure your settings in Sunshine match your Steam Deck's resolution/display:

![image](https://github.com/user-attachments/assets/23162299-4fc7-4365-82c6-2f62633b7a49)

### Install DisplayConfig
The next step is to install [DisplayConfig](https://www.powershellgallery.com/packages/DisplayConfig/2.0), a Powershell package that allows you to control your display state.

Install this by opening up a PowerShell or Terminal as Administrator, then type in `Install-Module -Name DisplayConfig` and hit Enter.

### Create your display configurations
Create a new directory for all of your scripts that will be used to control your display configuration. in this example, I will be using `C:\Scripts`, but you can realistically store these scripts anywhere if you know how to.

Open up a Powershell/Terminal and run the following commands, along with doing the following steps:
1. Create a Scripts directory:
  `mkdir 'C:\Scripts\`
2. Make a Display Configuration directory
  `mkdir 'C:\Scripts\display-configs`
3. Set your monitors to your "Default Configuration", which is essentially your regular monitor configuration when you are not using Sunshine/Moonlight. This can be done by going into **Settings > System > Display** and disabling your Virtual Monitor, and making sure the desired monitor is set as your "Main Display".

   For example, my "Monitor 3" (the Virtual Monitor) is set to "disconnect this display"
   ![image](https://github.com/user-attachments/assets/52f87bda-378b-410d-947f-e5f16438d8ad)
   My "Monitor 1" (my primary physical monitor) is set to "extend this display" and is set as my main monitor
   ![image](https://github.com/user-attachments/assets/9edbd817-84c9-4ba3-8712-a6fc1afa52b1)
   And my "Monitor 2" (my secondary physical monitor) is set to "extend this display"
   ![image](https://github.com/user-attachments/assets/c3e4aa38-d3c1-48b1-bed8-03b537bf681e)

4. Create your monitor profile for the Default Configuration by using the following command:
  `Get-DisplayConfig | Export-Clixml C:\Scripts\display-configs\default-profile.xml`
5. Set your monoitors to your "Sunshine Configuration". You can do this by disabling all of your phyiscal monitors, and enabling your virtual display and making it your main display. **NOTE**: You will need to connect to your PC with your Steam Deck during this step to be able to see the disabled monitor.

  For example, my Monitor 3 (The Virtual Monitor) is set to "extend this display"
  ![image](https://github.com/user-attachments/assets/4b4a2748-67dd-4648-ac28-0a23a7e0ac5f)

  And all other monitors are set to "Disconnect this display"
  ![image](https://github.com/user-attachments/assets/17c74cf2-5a24-4333-8a4f-ff856231b559)

6.  Create your monitor profile for the Sunshine configuration by using the following command:
  `Get-DisplayConfig | Export-Clixml C:\Scripts\display-configs\sunshine-profile.xml`

7. Manually reset your monitors back to your Default Configuration, so that you can use your normal displays for the rest of the setup.

### Create your Scripts to switch display configurations
Next, go into `C:\Scripts` and create 2 new files, `set-default.ps1` and `set-sunshine.ps1`.

Edit `set-default.ps1` to contain the following lines of code:
`
Import-Module DisplayConfig
Import-Clixml C:\Scripts\display-configs\default-profile.xml | Use-DisplayConfig -UpdateAdapterIds
`
And save the file.

Edit `set-sunshine.ps1` to contain the following lines of code:
`
Import-Module DisplayConfig
Import-Clixml C:\Scripts\display-configs\sunshine-profile.xml | Use-DisplayConfig -UpdateAdapterIds
`
And save the file.

### Assign Keyboard Shortcuts to your display configuration scripts
Open up File Explorer and navigate to `C:\Scripts`.

Right click in the folder and click **New > Shortcut**.
![image](https://github.com/user-attachments/assets/bc021e80-8c8e-4211-89a8-82ceee5d95e3)





## Credits
A lot of this is possible thanks to [MikeTheTech](https://www.youtube.com/channel/UCZ0hznff92f_i1wpYyi7tPQ/join).
