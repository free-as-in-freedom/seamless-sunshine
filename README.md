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

### Configure Sunshine for your resolution and framerate.
Next, go into your Sunshine management console (likely at [https://localhost:47990/](https://localhost:47990/), and log in and go to **Configuration > Audio/Video** and scroll to the bottom.

Make sure your settings in Sunshine match your Steam Deck's resolution/display:

![image](https://github.com/user-attachments/assets/23162299-4fc7-4365-82c6-2f62633b7a49)


## Credits
A lot of this is possible thanks to [MikeTheTech](https://www.youtube.com/channel/UCZ0hznff92f_i1wpYyi7tPQ/join).
