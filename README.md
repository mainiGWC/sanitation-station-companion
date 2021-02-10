# uOttaHack4
uOttaHack 4 Submission

## Inspiration

The general challenge of UottaHack 4 was to create a hack surrounding COVID-19. We got inspired by a COVID-19 restriction in the province of Quebec which requires stores to limit the number of people allowed in the store at once (depending on the store floor size). This results in many stores having to place an employee at the door of the shop to monitor the people entering/exiting, if they are wearing a mask and to make sure they disinfect their hands. Having an employee dedicated to monitoring the entrance can be a financial drain on a store and this is where our idea kicks in, dedicating the task of monitoring the door to the machine so the human resources could be best used elsewhere in the store.

## What it does

Our hack monitors the entrance of a store and does the following:
1. It counts how many people are currently in the store by monitoring the number of people that are entering/leaving the store.
2. Verifies that the person entering is wearing PPE ( a mask ). If no PPE was recognized, and a reminder to wear a mask is played from a speaker on the Raspberry Pi.
3. Verify that the person entering has used the sanitation station and displays a message thanking them for using it.
4. Display information to people entering such as. how many people are in the store and what is the store's max capacity, reminders to wear a mask, and thanks for using the sanitation station
5. Provides useful stats to the shop owner about the monitoring of the shop.

## How we built it
**Hardware:**  The hack uses a Raspberry Pi and it PiCam to monitor the entrance.

**Monitoring backend:** The program starts by monitoring the floor in front of the door for movement this is done using OpenCV. Once movement is detected pictures are captured and stored. the movement is also analyzed to estimate if the person is leaving or entering the store. Following an event of someone entering/exiting, a secondary program analyses the collection of a picture taken and submits chooses one of them to be analyzed by google cloud vision API. The picture sent to the google API looks for three features: faces, object location (to identify people's bodies), and labels (to look for PPE). Using the info from the Vision API we can determine first if the person has PPE and if the difference in the number of people leaving and entering by comparing the number of faces to the body detected. if the is fewer faces than bodies then that means people have left, if there is the same amount then only people entered. Back on the first program, another point is being monitored which is the sanitation station. if there is an interaction(movement) with it then we know the person entering has used it.

**cloud backend:**
The front end and monitoring hardware need a unified API to broker communication between the services, as well as storage in the mongoDB data lake; This is where the cloud backend shines. Handling events triggered by the monitoring system, as well as user defined configurations from the front end, logging, and storage. All from a highly available containerized Kubernetes environment on GKE.  

**cloud frontend:**
The frontend allows the administration to set the box parameters for where the objects will be in the store. If they are wearing a mask and sanitized their hands, a message will appear stating "Thank you for slowing the spread." However, if they are not wearing a mask or sanitized their hands, then a message will state "Please put on a mask." By doing so, those who are following protocols will be rewarded, and those who are not will be reminded to follow them.

## Challenges we ran into

On the monitoring side, we ran into problems because of the color of the pants. Having bright-colored pants registered as PPE to Google's Cloud Vision API (they looked to similar to reflective pants PPe's). 

On the backend architecture side, developing event driven code was a challenge, as it was our first time working with such technologies.

## Accomplishments that we're proud of
The efficiency of our computer vision is something we are proud of as we initially started with processing each frame every 50 milliseconds, however, we optimized the computer vision code to only process a fraction of our camera feed, yet maintain the same accuracy. We went from 50 milliseconds to 10 milliseconds

## What we learned

**Charles:** I've learn how to use the google API

**Mingye:** I've furthered my knowledge about computer vision and learned about google's vision API

**Mershab:** I built and deployed my first Kubernetes cluster in the cloud. I also learned event driven architecture.

## What's next for Sanitation Station Companion
We hope to continue improving our object detection and later on, detect if customers in the store are at least six feet apart from the person next to them. We will also remind them to keep their distance throughout the store as well. Their is also the feature of having more then on point of entry(door) monitored at the same time.
