# Teleop_motor_control_without_encoder_using_arduino

## Intsall the rosserial in raspberry pi and your laptop

Replace the <distro> with your distro running in your raspberry pi:
  
If *noetic* is running in your raspberry pi replace it as like: sudo apt-get install ros-noetic-rosserial-arduino && sudo apt-get install ros-noetic-rosserial
```
sudo apt-get install ros-<distro>-rosserial-arduino
sudo apt-get install ros-<distro>-rosserial
  
```
## Install the Arduino IDE in your Laptop or System.
  By using this link install the rosserial libraries in you (http://wiki.ros.org/rosserial_arduino/Tutorials/Arduino%20IDE%20Setup)
  
  **After finishing installation open Arduino-ide** 
  
 copy and paste the code and then upload it.
  
  ```
  #if defined(ARDUINO) && ARDUINO >= 100
#include "Arduino.h"
#else
#include <WProgram.h>
#endif
#include <stdlib.h>
#include <ros.h>
#include <std_msgs/String.h>
#include <ros/time.h>
#include <geometry_msgs/Twist.h>
#include <Wire.h>



int ena=3;           
int enb=6;
int in1=2;
int in2=4;
int in3=7;
int in4=8;


ros::NodeHandle nh;
geometry_msgs::Twist msg;

float move1;
float move2;

void callback(const geometry_msgs::Twist& cmd_vel)
{
  
  move1 = cmd_vel.linear.x;
 
  move2 = cmd_vel.angular.z;
  
  if (move1 > 0 && move2 == 0)
  {
    front();
  }
  else if (move1 > 0 && move2 > 0 )
  {
    left();
  }
  else if (move1 > 0 && move2 < 0 )
  {
    right();
  }
  else if (move1 < 0 && move2==0)
  {
    back();
  }
  else
  {
    die();
  }
}


ros::Subscriber <geometry_msgs::Twist> sub("/cmd_vel", callback);                           
void setup()
{

  nh.initNode();

  nh.subscribe(sub);
  pinMode(ena,OUTPUT);
  pinMode(enb,OUTPUT);
  pinMode(in1,OUTPUT);
  pinMode(in2,OUTPUT); //Arduino Pin#7 as Output 
  pinMode(in3,OUTPUT);
  pinMode(in4,OUTPUT);
  

}


/// @brief 
void loop()
{

  
   
  nh.spinOnce();
  delay(1);



  
}
void back()
{
  nh.loginfo("FRONT");
  analogWrite(ena,120);
  analogWrite(enb,120);
  digitalWrite(in1,LOW);   //Pin#7 as High
  digitalWrite(in2,HIGH); 
  digitalWrite(in3,HIGH);   //Pin#7 as High
  digitalWrite(in4,LOW);
  delay(100);

  die(); 
    
}
void left()
{
  nh.loginfo("LEFT");
  analogWrite(ena,120);
  analogWrite(enb,120);
  digitalWrite(in1,HIGH);   //Pin#7 as High
  digitalWrite(in2,LOW); 
  digitalWrite(in3,HIGH);   //Pin#7 as High
  digitalWrite(in4,LOW);
  delay(100);
  die();   
}
void right()
{
  nh.loginfo("RIGHT");
  analogWrite(ena,120);
  analogWrite(enb,120);
  digitalWrite(in1,LOW);   //Pin#7 as High
  digitalWrite(in2,HIGH); 
  digitalWrite(in3,LOW);   //Pin#7 as High
  digitalWrite(in4,HIGH);
  
  delay(100);

  
  die();

}
void front()
{
  nh.loginfo("BACK");
  analogWrite(ena,120);
  analogWrite(enb,120);
  digitalWrite(in1,HIGH);   //Pin#7 as High
  digitalWrite(in2,LOW); 
  digitalWrite(in3,LOW);   //Pin#7 as High
  digitalWrite(in4,HIGH);
  
  
  
  delay(100);
  //motor1.run(BACKWARD);

  die(); 
}
void die()
{
  nh.loginfo("STOP");
  analogWrite(ena,120);
  analogWrite(enb,120);
  digitalWrite(in1,LOW);   //Pin#7 as High
  digitalWrite(in2,LOW); 
  digitalWrite(in3,LOW);   //Pin#7 as High
  digitalWrite(in4,LOW);
}
```
 Then make it your laptop as a "Master" and Raspberry pi as a "slave"
  To do this follow the link[https://github.com/sasuke-shadowHokage/master-and-slave-configuration-for-pc-to-raspberry-pi/blob/main/README.md]

## Run **roscore** in your Laptop
 if any issue while run roscore check the ip or contact me **karunakarans@karunya.edu.in**
  
## Run this command on Raspberry pi before that connect the arduino serial cable to raspberry pi
  ```rosrun rosserial_arduino serial_node.py _port:=/dev/ttyACM0```
## Run teleop command
  ```rosrun teleop_twist_keyboard teleop_twist_keyboard.py```
  
  
  After this by pressing the key I,U,O,< you can move your robot.
  
 

 
  
  

