# Soundproblems-Pulseaudio-Ubuntu-20.04.4
How to fix sound &amp; mic problems in Ubuntu 20.04.4

<hr>

<b>This guide is for everyone using Ubuntu 20.04.4 and is experiencing sound issues like to following:</b>

- Static sounds
- Echo's
- Bad Microphone ( unclear sound, cracking voice ect)

Before performing the following commands make sure you understand linux basics as these commands will change a root file. Always before fixing a issue in Linux or any other system-operator make sure you make a <b>BACKUP.</b>

<hr>

<b>Before performing the following commands make sure you check if the problem comes from external sources.
Alot of laptops give sounds problems due to electical problems. </b>

- Check if your laptop is using a grounded adapter for power charging.
- Check if your house is grounded. 
- Check if your electrical extention cable or outlet in the wall is grounded. 

Many laptops experience sound problems due to missing grounding. 

<b>( How do you check if your outlet / house is grounded? )</b>

- Call a electrician to measure your house. 

If you are familair with using a electric voltage meter and you have such a device use the following steps:
- Put your voltage meter device on ACV 720 
- Put the red and the black probe in the electrical outlet that is connectect your laptop. (!Make sure you dont touch any metal parts!*) 
- This measure should give you any between 200 and 240 volt depending on your standard voltage in you home
- Now remove the black probe and touch the grounding plate in top or bottom middle of our outlet. (if there is none this outlet is not grounded. )
- If there is you should get a measurement equal to the voltage you measured before with +- 5 volt marge.
(This means your outlet is grounded)
- If your measurement equals 0. 
(This means your outlet is not grounded) 

If your outlet is not grounded look for another outlet to see if your sound problem resolves. 

If you cant find a outlet that is grounded test your device at another location or computer shop with a grounded outlet to see if your problems resolves before continue following the following steps.

<hr> 

<b> Peform a power drain on your laptop </b>

After you fixed yourself a grounded outlet you want to know that the static electricity that was build up in your laptop is gone before starting finding solutions for your sound.
Because you had a grounded adapter but didnt use a grounded outlet for a while electricity can be build up in you motherboard creating hardware problems. 

Perform the following actions to remove this static electricity from your laptop:

- Unplug your laptop from any power source.
- Remove the laptop battery.
- Hold the power-button for around 30 seconds. ( this removes the build up electricity )
- Insert the laptop battery.
- Plug your laptop with the power source. ( Make sure its grounded ;) )
- Power on the laptop.

Now the build up static electric should be gone. 

<hr>

<b> Fixing sound problems due to Pulse Audio </b>

Audio problems are common using linux on different devices. Because of the difference in sound card drivers, new driver updates. New laptops with new soundcards. As linux is a open source operating system made by developers some changes will not be inmediate be updated in the source code. If they ever will...

I came to this solution using a Lenovo thinkpad while I had updated from Dual boot Windows + Kali Linux to Ubuntu 20.04.4, I experienced crazy sound problems I never had before. Tried over 20 different solutions on the web but only one solution worked for me and for many others. 

My problem started when I had to do online calls & meetings and people mentioned that my voice was bad and I could not hear people properly. After trying multiple optional solutions nothing worked. 
So in my research I came to the problem that my outlet was not grounded. Due to this there was static electricity building up in the motherboard and gave problems. I fixed the grounding issue but still had problems with my audio. 

After doing research and testing of multiple optional solutions I came up with one solution that worked for me and many others. And it goes as follow:

`Open the command prompt with ctrl + alt + T `

Follow these steps: 

1. Edit /etc/pulse/default.pa as root with a text editor, like Nano (command line text editor) for example: 

`sudo nano /etc/pulse/default.pa`

2. Scroll down to the end of this file and paste the following and save the file:

`.ifexists module-echo-cancel.so
load-module module-echo-cancel aec_method=webrtc source_name=echocancel sink_name=echocancel1
set-default-source echocancel
set-default-sink echocancel1
.endif`

This is what this does: if your system PulseAudio is compiled with the echo / noise cancellation module, load this module, use webrtc as the echo cancellation method (it should be better than the default speex), specify a source and sink names, then set that source and sink as default.

3. Go to ~./config/pulse 
`Delete all the files in this folder`

4. Reload PulseAudio using this command:

`pulseaudio -k`

5. Go to your sound settings and select as the following option:

OUTPUT: 
`echo cancelled`

INPUT:
`echo cancelled`

Now your sound and microphone should work properly. 

Everytime you reboot your device you should follow step 3 till 5 if you come accros the same sound problem.

I am working on a permanent solution so this don't have to be done. Feel free to contact me or request a pull request with a permanent solution. 

