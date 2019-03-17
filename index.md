
<p align="center">
  <img src="https://thumbs.gfycat.com/MindlessLargeDunlin-size_restricted.gif">
</p>

# Background
Washington Avenue in Natick is scheduled to be improved during the 'fall cycle' construction season of 2019. As part of the town's capital improvement budget the road will be resurfaced and continuous sidewalks will be installed on both sides of the roads. Currently the road is in desperate need of attention. The road is narrow and lacks adequate markings, there are numerous utility poles which block sight lines, sidewalks are obstructed in some places, lack curbing and demarkation from the roadway. 

## The Problem
The town hosted a public forum on December 3, 2018 to share its results of a land survey which was conducted during the fall of 2018. Town engineers along with a consultant from Environmental Partners, Inc. shared their goals of resurfacing the road, adding continuous sidewalks, redesigning the route 27 / Lake Ave entrance of Washington Ave. In addition citizens were able to voice concerns about the street with the hopes that their concerns could aid in the final design and project. 

There were a number of concerns voiced during the meeting. Many folks were concerned about the ineffectivenes and lack of crosswalks on the street. Further there was discussion about various traffic calming measures that could be introduced. Finally, and what struck me as being the most surprising was a comment from the traffic officer that there had been little if any complaints from residents regarding the speed of traffic on the road. Further, it was shared that a traffic study to 'count axles' could be completed at some point during the spring. 

## Setup
I had gotten a Raspberry Pi for myself at some point during the fall and forgetting what I had originally purchased it for decided this would be an interesting project to take on. A couple of google searches lead me to a few implementations using the raspberry pi, a webcam, and OpenCV to detect motion and track an object. From there it's a matter of timing the object as it moves through the field of view and using that timing informaiton and some information about or camera to determine a fairly accurate speed. 

The setup consists of a Raspberry Pi 3b+ running Debian Stretch, a Microsoft LifeCam HD-3000 web, with OpenCV4 used to do the image processing. Here is everything as it's currently setup in my front window: 
<p align="center">
  <img height="320" width="240" src="https://i.imgur.com/vjRcmYAl.jpg">
</p>

According to the [specs from the webcam](https://dl2jx7zfbtwvr.cloudfront.net/specsheets/WEBC1010.pdf) the FoV is 68.5° diagonal. I used Google Maps to measure the distance from my window to the road. Since the angle, and distance are known, it's a matter of using some basic trig to calculate the total range within the field of view of the web cam. Finally, I used OpenCV for image processing to detect movement via that camera's captured frames and then time the duration of an object moving across the field of view. 

### Sample Pictures and Data
The speed camera is running a

## Limitations
I tested the setup with cars going at known speeds of 20, 25, 30, 35 and 40 mph. The results are accurate to within a single MPH or two, which is probably good enough for my study. The software can't detect when multiple objects are within the tracking area, so the number of cars counted is definitely less than reality. The camera also only works during daylight hours, so the measurements much before or after sunrise/sunset aren't accurate at all. 




