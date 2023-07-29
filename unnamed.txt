#my code
"""arm_controller controller."""

# You may need to import some classes of the controller module. Ex:
#  from controller import Robot, Motor, DistanceSensor
from controller import Robot, Keyboard, DistanceSensor


# create the Robot instance.
robot = Robot()
keyboard = Keyboard()
# get the time step of the current world.
timestep =64
sensor = robot.getDevice("distance sensor")

shoulderLift = robot.getDevice("shoulder_lift_joint")
shoulderPan = robot.getDevice("shoulder_pan_joint")
elbow = robot.getDevice("elbow_joint")
wrist1 = robot.getDevice("wrist_1_joint")
wrist2 = robot.getDevice("wrist_2_joint")
wrist3 = robot.getDevice("wrist_3_joint")
finger1 = robot.getDevice("palm_finger_1_joint")
finger1_lowerknucle = robot.getDevice("finger_1_joint_1")
finger1_middleknucle = robot.getDevice("finger_1_joint_2")
finger1_upperknucle = robot.getDevice("finger_1_joint_3")

finger2 = robot.getDevice("palm_finger_2_joint")
finger2_lowerknucle = robot.getDevice("finger_2_joint_1")
finger2_middleknucle = robot.getDevice("finger_2_joint_2")
finger2_upperknucle = robot.getDevice("finger_2_joint_3")


finger3_lowerknucle = robot.getDevice("finger_middle_joint_1")
finger3_middleknucle = robot.getDevice("finger_middle_joint_2")
finger3_upperknucle = robot.getDevice("finger_middle_joint_3")


shoulderLiftPos = 0
shoulderPanPos=0
elbowPos=0
wrist1Pos=0
wrist2Pos=0
wrist3Pos=0
fingerPos = 0.17
lowerknucklepos=0.05
middleknucklepos=0
upperknucklepos=-0.06
keyboard.enable(timestep)
sensor.enable(timestep)
def moveBot(a=0, b=0, c=0, d=0, e=0, f=0, g=0.17, h=0.05, i=0, j=-0.06):
    shoulderLift.setPosition(a)
    shoulderPan.setPosition(b)
    elbow.setPosition(c)
    wrist1.setPosition(d)
    wrist2.setPosition(e)
    wrist3.setPosition(f)
    finger1.setPosition(g)
    finger2.setPosition(g)
    
    finger3_lowerknucle.setPosition(h)
    finger2_lowerknucle.setPosition(h)
    finger1_lowerknucle.setPosition(h)

    finger3_middleknucle.setPosition(i)
    finger2_middleknucle.setPosition(i)
    finger1_middleknucle.setPosition(i)
    
    finger3_upperknucle.setPosition(j)
    finger2_upperknucle.setPosition(j)
    finger1_upperknucle.setPosition(j)
moveBot()
def add_delay(time):
    timeCounter = 0
    while robot.step(timestep)!=1:
        if timeCounter>time:
            break;
        timeCounter=timeCounter+1
while robot.step(timestep)!=1:
    val = sensor.getValue()

    if(val<400):
        moveBot(0.15, 1.57, -0.1, -0.04, h=0.4, i=0.4) #to grab the object
        add_delay(10)
        moveBot(0, 1.57, 0, 0, h=0.45, i=0.45) #to lift the shoulder and straighten up the elbow and the wrist
        
        add_delay(10)
        print("shoulder straighend up")
        moveBot(0, 0.4, 0, 0, h=0.4, i=0.4) #to move the arm over the box
        add_delay(22)
        moveBot(0, -0.1, 0, 0, h=0.05, i=0) #to open the claw
        add_delay(10)
        moveBot(-0.1, 1.57) #go back to the conveyor belt
        add_delay(10)
    else:
        print("else")
        moveBot(0.0, 1.5, -0.1, -0.04) #to remain on the coveyor belt

        
        
        
        
        
        
# You should insert a getDevice-like function in order to get the# instance of a device of the robot. Something like:
#  motor = robot.getDevice('motorname'
#  ds = robot.getDevice('dsname'
#  ds.enable(timestep

# Main loop:
# - perform simulation steps until Webots is stopping the controller
while robot.step(timestep) != -1:
        keyPressed=keyboard.getKey()

        if(keyPressed==317):#down arrow
            shoulderLiftPos+=0.01
        if(keyPressed==315):#UParrow
            shoulderLiftPos-=0.01
        if(keyPressed==314):#left arrow
            shoulderPanPos+=0.01
        if(keyPressed==316):#right arrow
            shoulderPanPos-=0.01          
        if(keyPressed==87):#w key
            elbowPos-=0.01
        if(keyPressed==83):#s key
            elbowPos+=0.01  
        if(keyPressed==65):#a arrow
            wrist1Pos+=0.01
        if(keyPressed==68):#d 
            wrist1Pos-=0.01
        if(keyPressed==49):#1 
            wrist2Pos+=0.01
        if(keyPressed==50):#2 
            wrist2Pos-=0.01
        if(keyPressed==51):#3 
            wrist3Pos+=0.01
        if(keyPressed==52):#4 
            wrist3Pos-=0.01

        if(keyPressed==53):#5
            fingerPos+=0.01
        if(keyPressed==54):#6 
            fingerPos-=0.01
        if(keyPressed==55): #7
            lowerknucklepos+=0.01
        if(keyPressed==56):#8 
            lowerknucklepos-=0.01 
        if(keyPressed==57):#9 
            middleknucklepos+=0.1
        if(keyPressed==48):#0
            middleknucklepos-=0.01  
        if(keyPressed==45):#-
            upperknucklepos-=0.01  
        if(keyPressed==61):#+
            upperknucklepos+=0.01  
        moveBot(shoulderLiftPos, shoulderPanPos, elbowPos, wrist1Pos, wrist2Pos, wrist3Pos, fingerPos, lowerknucklepos, middleknucklepos, upperknucklepos)
    # Process sensor data here.

    # Enter here functions to send actuator commands, like:
    #  motor.setPosition(10.0)


# Enter here exit cleanup code.
