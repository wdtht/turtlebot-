# turtlebot-
很简单的控制turtlebot避障的c++代码
```
#include "ros/ros.h"
#include "sensor_msgs/LaserScan.h"
#include <sstream>
#include "geometry_msgs/Twist.h"

geometry_msgs::Twist v;
//int mark=0;

void turn(const sensor_msgs::LaserScanConstPtr& range)
{
  if(range->ranges[0]<0.9||range->ranges[319]<0.9)
  { 
    /*if(mark=2)
    {
      v.angular.z=1; 
      v.linear.x=0;
      sleep(2);
      return;
    }*/
    v.angular.z=-1;
    v.linear.x=0;
    sleep(1);
    //mark=1;
    //return;
  }
  else if(range->ranges[638]<0.9)
  { 
   /* if(mark=1)
    {
      v.angular.z=-1;
      v.linear.x=0;
      sleep(2);
      return;
    }*/
    v.angular.z=1; 
    v.linear.x=0;
    sleep(1);
    //mark=2;
    //return;
  }     
  //mark=0;
}

int main(int argc,char **argv)
{
  ros::init(argc,argv,"turn");
  ros::NodeHandle n;
  ros::Subscriber sub=n.subscribe("scan",1,turn);
  ros::Publisher pub=n.advertise<geometry_msgs::Twist>("mobile_base/commands/velocity",10);
  while(ros::ok())
  {
    v.angular.z=0;
    v.linear.x=0.2;
    ros::spinOnce();
    pub.publish(v);
  }
  return 0;
}
```
