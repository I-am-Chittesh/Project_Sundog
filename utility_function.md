# Utilities for cleaning bot and rover and drone
---
# Rover
---
1. deploy cleaning bot on solar panel
2. collect the cleaning bot and return to power station
3. active obstacle avoidance
4. path planning
5. fail safe mechanism

---
# dev criterias for rover
1. active coordinate system: actively determine coordinates for path planning
2. if rover gets stuck and unable to proceed further, using rover's current coordinates, the user should get the panel number
3. after deploying the cleaning bot, the rover has to remain stationary at its original position until the cleaning bot is loaded back
4. activating window for cleaning
5. charging dock for cleaning bot


---
# Drone
---
1. scan for abnormalities and return the coordinates of the respective panel
2. determine flying algorithm
3. proper telemetry connection with the digital twin
4. store data in proper format, reach place with internet and transfer data appropriately
5. fail safe mechanism

---
# Cleaning Bot
---
1. proper cleaning mechanism & situational awareness (make sure it doesn't fall off the panel)
2. cleaning algorithm
3. proper telemetry connection with the rover & return back to the same point where it started
4. either tiny whoop or extendible platform for loading bot onto the panel

--- 
# dev criteria for bot
1. proper torque and traction
2. safety algorithm to ensure that bot doesn't fall off
3. should cover all parts of the panel (algorithm)
4. proper awareness of power ( before loading onto the panel, it should check if it can clean the entire panel)
   
