# EE-629A Project
  
# Project title: Security camera
## Description
This project is applicable for monitoring an environment by streaming the camera recording to an online cloud. The video could be accessed on the cell phone by authentication. What I need to use for online monitoring include:
Raspberry Pi.
Raspberry Pi mini camera for sensing the environment.
A cloud for up/down-streaming the video.


### Step 1.
Before powering up the Raspberry Pi, I connected the camera into the camera serial interface (CSI) slot as shown in Fig. 1. It should be noted that the blue part on the head of ribbon cable should face the USB ports.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002210-42656880-3cc8-11eb-843a-0aa3d0e7cf15.jpg"  width="300">
  <figcaption>Fig. 1. Raspberry Pi CSI port and ribbon cable orientation.</figcaption>
</figure>


### Step 2.
After connecting the camera to Raspberry Pi, we need to power on the board. In order to be able to use the camera, we should activate the camera in Raspberry Pi configuration using the code below. Otherwise, we may not access the camera through different libraries.
sudo raspi-config 
Upon running the code, a blue panel pops up, where one can change the configuration in Raspberry Pi. Then, We should go to Interfacing Options as illustrated in Fig. 2. Finally, P1 Camera should be selected and enabled according to Fig. 3, and Fig. 4, respectively.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002359-c53af300-3cc9-11eb-80d1-6e4aa4114857.jpg"  width="550">
  <figcaption>Fig. 2. Raspberry Pi configuration panel.</figcaption>
</figure>

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002360-c66c2000-3cc9-11eb-8286-379195d2523c.jpg"  width="550">
  <figcaption>Fig. 3. Selection of camera on Raspberry Pi configuration panel under interfacing options.</figcaption>
</figure>

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002361-c79d4d00-3cc9-11eb-9072-763d365b09d2.jpg"  width="550">
  <figcaption>Fig. 4. Camera activation panel.</figcaption>
</figure>

### Step 3.
In this project, YouTube was selected as the online cloud where the video captured by Raspberry Pi camera is supposed stream to. Following this idea, we need to encode the videos. To  this end, ffmpeg library was chosen. Thus, using the following command, we may install ffmpeg library. [[reference](https://www.google.com/url?q=https%3A%2F%2Fffmpeg.org%2F&sa=D&sntz=1&usg=AFQjCNGOaytuiXAoH-y-RNvBhPOY43qdTg)]
sudo apt-get install ffmpeg

### Step 4.
After installing the library for encoding, I signed in to my YouTube account. In my account, I had to activate the live stream feature. For the activation, one is required to enter a cell phone number, and a text message including the activation code is sent. Upon entering the code, it will take 24 hours for your live stream to be activated. Be careful about that. I did not expect that 24 hours. It changed my time schedule!!  As shown in Fig. 5, one may find the Live feature on the left side of the page on a desktop computer.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002421-55793800-3cca-11eb-8683-a4103260a716.jpg"  width="550">
  <figcaption>Fig. 5. YouTube menu for Live panel activation.</figcaption>
</figure>


As shown in Fig. 6, the camera button should be clicked and Go live option should be selected from the top-right menu.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002424-59a55580-3cca-11eb-8184-53f7f49fd0ab.jpg"  width="550">
  <figcaption>Fig. 6. Go live option for streaming the video on YouTube account.</figcaption>
</figure>

Then, stream URL and stream key should be copied as illustrated in Fig. 7. These two let other softwares to upload their videos on YouTube live account.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002432-6033cd00-3cca-11eb-8c58-ca20556f077b.jpg"  width="550">
  <figcaption>Fig. 7. Stream URL and stream key panel.</figcaption>
</figure>

### Step 5.
Following copying the stream key and URL, we need to get back to our Raspberry Pi terminal. In order to stream the video on YouTube, we should use the following command that uses the ffmpeg library to encode the video, and change some settings such as number of frames per second.  
raspivid -o - -t 0 -vf -hf -fps 10 -b 500000 | ffmpeg -re -ar 44100 -ac 2 -acodec pcm_s16le -f s16le -ac 2 -i /dev/zero -f h264 -i - -vcodec copy -acodec aac -ab 128k -g 50 -strict experimental -f flv rtmp://a.rtmp.youtube.com/live2/[STREAM-KEY]  
In this command, we should replace the [Stream-Key] by our own stream key copied in the precious step. After almost half a minute within entering the command above, log information of the camera is shown continuously.
raspivid is the command line tool for capturing video with Raspberry Pi camera module. [[reference](https://www.google.com/url?q=https%3A%2F%2Fwww.raspberrypi.org%2Fdocumentation%2Fusage%2Fcamera%2Fraspicam%2Fraspivid.md&sa=D&sntz=1&usg=AFQjCNEKciOzTjb4PhoWLN6OviUY5Dq3xA)]
As we just want to stream the video over the YouTube live channel we use -t 0. This means that we want to save 0 milliseconds of the video on a local memory. -vf -hf helps flip the video 180 degrees. It is used when the camera orientation is upside-down. Using -fps could specify the number of frames being streamed per second. The less frames being streamed per second, the faster the streaming system, and in turn, the less the time lag for online application.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002433-65911780-3cca-11eb-8e96-cb07aef36a99.jpg"  width="550">
  <figcaption>Fig. 8. Video streaming log on Raspberry Pi terminal.</figcaption>
</figure>

### Step 6.
After streaming the camera, we may use our security camera on any device on which we sign in to our YouTube account. For instance, I used my desktop computer to monitor my room as shown in Fig. 9. However, my internet connection is not good at home. Therefore, video streaming delayed a bit on the YouTube account.

<figure class="image">
  <img src="https://user-images.githubusercontent.com/71201920/102002434-6de95280-3cca-11eb-9895-57a6ed568866.jpg"  width="550">
  <figcaption>Fig. 9. Streaming output on YouTube account.</figcaption>
</figure>



To recapitulate, we developed a security camera using Raspberry Pi board. In this system, one can monitor his room using the authentication on YouTube as the server. Here, you can find the [[reference](https://www.google.com/url?q=https%3A%2F%2Fwww.digikey.com%2Fen%2Fmaker%2Fblogs%2Fstreaming-live-to-youtube-and-facebook-using-raspberry-pi-camera&sa=D&sntz=1&usg=AFQjCNECELvqQOUP_hf_-ZfzKQ604Gegow)] used for this project.
