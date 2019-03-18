
<p align="center">
  <img src="https://thumbs.gfycat.com/MindlessLargeDunlin-size_restricted.gif" />
</p>

# Background
Washington Avenue in Natick is scheduled to be improved during the 'fall cycle' construction season of 2019. As part of the town's capital improvement budget the road will be resurfaced and continuous sidewalks will be installed on both sides of the roads. Currently the road is in desperate need of attention. The road is narrow and lacks adequate markings, there are numerous utility poles which block sight lines, sidewalks are obstructed in some places, lack curbing and demarkation from the roadway. 

## The Problem
The town hosted a public forum on December 3, 2018 to share its results of a land survey which was conducted during the fall of 2018. Town engineers along with a consultant from Environmental Partners, Inc. shared their goals of resurfacing the road, adding continuous sidewalks, redesigning the route 27 / Lake Ave entrance of Washington Ave. In addition citizens were able to voice concerns about the street with the hopes that their concerns could aid in the final design and project. 

There were a number of concerns voiced during the meeting. Many folks were concerned about the ineffectiveness and lack of crosswalks on the street. Further there was discussion about various traffic calming measures that could be introduced. Finally, and what struck me as being the most surprising was a comment from the traffic officer that there had been little if any complaints from residents regarding the speed of traffic on the road. Further, it was shared that a traffic study to 'count axles' could be completed at some point during the spring. 

## Setup
I had gotten a Raspberry Pi for myself at some point during the fall and forgetting what I had originally purchased it for decided this would be an interesting project to take on. A couple of google searches lead me to a few implementations using the raspberry pi, a webcam, and OpenCV to detect motion and track an object. From there it's a matter of timing the object as it moves through the field of view and using that timing informaiton and some information about or camera to determine a fairly accurate speed. 

The setup consists of a Raspberry Pi 3b+ running Debian Stretch, a Microsoft LifeCam HD-3000 web, with OpenCV4 used to do the image processing. Here is everything as it's currently setup in my front window: 
<p align="center">
  <img height="320" width="240" src="https://i.imgur.com/vjRcmYAl.jpg">
</p>

According to the [specs from the webcam](https://dl2jx7zfbtwvr.cloudfront.net/specsheets/WEBC1010.pdf) the FoV is 68.5° diagonal. I used Google Maps to measure the distance from my window to the road. Since the angle, and distance are known, it's a matter of using some basic trig to calculate the total range within the field of view of the web cam. Finally, I used OpenCV for image processing to detect movement via that camera's captured frames and then time the duration of an object moving across the field of view. 

<p align="center">
  <img src="https://i.imgur.com/cK1l6rz.png">
</p>

**Where:** 
* V = .5 * Field of View
* D = Distance to the road
* R = 2 * D * tan(V)

The angular FoV of the Microsoft LifeCam HD-3000 is 59.1 and the distance from the road is 58. So using the formula above to calculate the value for R: 

      R = 2 * 58 * tan(.5 * 59.1)

Solving for R yields ~65. The camera sees approximately 65 feet of road when it's positioned in my window 58 feet away. The camera is set to capture images 1024 pixels wide, so we know that each pixel traveled is ~0.06 feet. 

### Sample Pictures and Data
When the camera determines the speed of a tracked object it captures and image of the object and stores an entry regarding the time, speed and direction of travel. I've uploaded the images and data to AWS so that it's available to those that are interested.

<p align="center">
  <img src="https://s3.amazonaws.com/washingtonave-speedcam/car_at_20190220_101653.jpg" />
</p>

| Date | Day | Time | Direction | Speed | URL |
| --- | --- | --- | --- | --- | --- |
| 2019.02.20 | Wednesday | 1016 | S | 28 | https://s3.amazonaws.com/washingtonave-speedcam/car_at_20190220_101653.jpg |

If there is interest in the source code, I can upload it to github as well. 

## Results
To my surprise, the average speeds on Washington Ave (at least on the 65' in front of my house) are just about at the speed limit. It seems to me that the perception of speed on the street likely enabled by the lack of markings, defined curbcuts, all of the things which were spoken to during the previous meetings. The table below shows the average speed and count of vehicles during each day of the week for the period 3/1 - 3/14.

| Day | Avg Speed (mph) | Avg Cars |
| --- | --- | --- |
| Sunday | 28  | 2088 |
| Monday | 27  | 2747 |
| Tuesday | 27 | 3692 |
| Wednesday | 29 | 3901 | 
| Thursday | 28 | 3892  | 
| Friday | 28 | 3724  |
| Saturday | 28 | 2448 | 

[I used Excel to create the table above, there is probably a lot more information hiding within](https://s3.amazonaws.com/washingtonave-speedcam/carspeed.xlsx). I did clean up the data a bit to remove most of the false positives from the results, any captures that weren't during daylight hours, etc. If you're interested in the raw data it's here too, [going back to 2/17 and until 3/17](https://s3.amazonaws.com/washingtonave-speedcam/carspeed_raw.csv).

## Limitations
I tested the setup with cars going at known speeds of 20, 25, 30, 35 and 40 mph. The results are accurate to within a single MPH or two, which is probably good enough for my study. The software can't detect when multiple objects are within the tracking area, so the number of cars counted is definitely less than reality. The camera also only works during daylight hours, so the measurements much before or after sunrise/sunset aren't accurate at all. 




