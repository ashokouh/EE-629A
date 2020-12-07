# EE-629A Project
  
# Project title: Security camera
## Description
This project is applicable for monitoring an environment by streaming the camera recording to an online cloud. The video could be accessed on the cell phone by authentication. What I need to use for online monitoring include:
Raspberry Pi.
Raspberry Pi mini camera for sensing the environment.
A cloud for up/down-streaming the video.


### Step 1.
Before powering up the Raspberry Pi, I connected the camera into the camera serial interface (CSI) slot as shown in Fig. 1. It should be noted that the blue part on the head of ribbon cable should face the USB ports.



### Step 2.
After connecting the camera to Raspberry Pi, we need to power on the board. In order to be able to use the camera, we should activate the camera in Raspberry Pi configuration using the code below. Otherwise, we may not access the camera through different libraries.
sudo raspi-config 
Upon running the code, a blue panel pops up, where one can change the configuration in Raspberry Pi. Then, We should go to Interfacing Options as illustrated in Fig. 2. Finally, P1 Camera should be selected and enabled according to Fig. 3, and Fig. 4, respectively.



### Step 3.
In this project, YouTube was selected as the online cloud where the video captured by Raspberry Pi camera is supposed stream to. Following this idea, we need to encode the videos. To  this end, ffmpeg library was chosen. Thus, using the following command, we may install ffmpeg library. [reference]
sudo apt-get install ffmpeg

### Step 4.
After installing the library for encoding, I signed in to my YouTube account. In my account, I had to activate the live stream feature. For the activation, one is required to enter a cell phone number, and a text message including the activation code is sent. Upon entering the code, it will take 24 hours for your live stream to be activated. Be careful about that. I did not expect that 24 hours. It changed my time schedule!!  As shown in Fig. 5, one may find the Live feature on the left side of the page on a desktop computer.



As shown in Fig. 6, the camera button should be clicked and Go live option should be selected from the top-right menu.


Then, stream URL and stream key should be copied as illustrated in Fig. 7. These two let other softwares to upload their videos on YouTube live account.


### Step 5.
Following copying the stream key and URL, we need to get back to our Raspberry Pi terminal. In order to stream the video on YouTube, we should use the following command that uses the ffmpeg library to encode the video, and change some settings such as number of frames per second.
raspivid -o - -t 0 -vf -hf -fps 10 -b 500000 | ffmpeg -re -ar 44100 -ac 2 -acodec pcm_s16le -f s16le -ac 2 -i /dev/zero -f h264 -i - -vcodec copy -acodec aac -ab 128k -g 50 -strict experimental -f flv rtmp://a.rtmp.youtube.com/live2/[STREAM-KEY] 
In this command, we should replace the [Stream-Key] by our own stream key copied in the precious step. After almost half a minute within entering the command above, log information of the camera is shown continuously.
raspivid is the command line tool for capturing video with Raspberry Pi camera module. [reference]
As we just want to stream the video over the YouTube live channel we use -t 0. This means that we want to save 0 milliseconds of the video on a local memory. -vf -hf helps flip the video 180 degrees. It is used when the camera orientation is upside-down. Using -fps could specify the number of frames being streamed per second. The less frames being streamed per second, the faster the streaming system, and in turn, the less the time lag for online application.

### Step 6.
After streaming the camera, we may use our security camera on any device on which we sign in to our YouTube account. For instance, I used my desktop computer to monitor my room as shown in Fig. 9. However, my internet connection is not good at home. Therefore, video streaming delayed a bit on the YouTube account.


To recapitulate, we developed a security camera using Raspberry Pi board. In this system, one can monitor his room using the authentication on YouTube as the server. Here, you can find the reference used for this project.
