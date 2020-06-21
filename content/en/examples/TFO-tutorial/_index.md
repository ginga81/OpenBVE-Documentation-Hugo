---
title: "TrackFollowingObject Tutorial"
linktitle: "TrackFollowingObject Tutorial"
weight: 1
---

##  ■ Index

{{% indent %}}

[Overview](#overview)

[Example route and example car data of outer-view](#sampleroute)

[The file list of sample routes](#filelist)

[How to use TFO](#howtousetfo)

[The characteristic of TFO](#tfo-characteristic)

[First theme](#Firsttheme)

[Individual configuration of XML](#Individual-configuration-of-XML)

[Definition section](#Definition-section)

{{% indent %}}

[AppearanceTime subsection](#AppearanceTime-subsection)

[AppearanceStartPosition subsection](#AppearanceStartPosition-subsection)

[AppearanceEndPosition subsection](#AppearanceEndPosition-subsection)

[LeaveTime subsection](#LeaveTime-subsection)

{{% /indent %}}

[Train section](#Train-subsection)

{{% indent %}}

[Directory subsection](#Directory-subsection)

{{% /indent %}}

[Stops section](#Stops-section)

{{% indent %}}

[Decelerate subsection](#Decelerate-subsection)

[StopPosition subsection](#StopPosition-subsection)

[Doors subsection](#Doors-subsection)

[StopTime subsection](#StopTime-subsection)

[Accelerate subsection](#Accelerate-subsection)

[TargetSpeed subsection](#TargetSpeed-subsection)

[Direction subsection](#Direction-subsection)

[Rail subsection](#Rail-subsection)

{{% /indent %}}

[About RailX number restrictions](#RailX-number-restriction)

[The first Stop subsection](#first-Stop-subsection)

[Stop the TFO train](#Stop-TFO-train)

[To return from the departure station to the terminal station](#To-return-from-the-departure-station)

[How to move several stations and stops at each station](#How-to-stops-at-each-station)

[How to cross the oncoming train on a single rail at station](#How-to-cross-the-oncoming-train)

[How to change to the other rail](#How-to-change-to-the-other-rail)

[How to overtaking from behind by TFO train](#How-to-overtaking)

[How to overtaking the oncoming trains each other(to move multiple TFO objects)](#How-to-overtaking-oncoming-trains)

[How to moving on points](#moving-on-points)

[How to do more complex action across points](#How-to-do-more-complex-points)

[How to combine two or more TFO trains and how to move a combined TFO train](#combine-two-or-more-TFO-trains)

[How to separate a train and moving each TFO train](#separate-a-train)

[How to change the rail while moving & how to change the speed while moving](#change-rail-and-speed)

[Points subsection](#Points)

{{% indent %}}

[Point subsection](#Point)

[Decelerate subsection](#point-Decelerate)

[Position subsection](#point-Position)

[Accelerate subsection](#point-Accelerate)

[PassingSpeed subsection](#point-PassingSpeed)

[TargetSpeed subsection](#point-TargetSpeed)

[Rail subsection](#point-Rail)

{{% /indent %}}

[About relationship between PassingSpeed and TargetSpeed subsections](#point-PassingSpeed-TargetSpeed)

[Points to note when switching RailIndex and Direction at Point subsection](#point-RailIndex)

{{% /indent %}}

---
## <a name="overview"></a> ■ Overview

The TFO(TrackFollowingObject) is "The Animated Object thati can moving freely on rails".

By adding programmable movement to Animated Object, we can moving not only train but bus, car, airplane, elevator and more various objects.

TFO can move a train formation can move. To move a train formation, need a set of train that has at least with Extensions.cfg and Train.dat.

In this tutorial, we will use the built in public domain 103 series to learn how to move trains.

Also, this tutorial covers OpenBVE 1.6.0.0 to 1.7.1.3, but it also describes new features that will be implemented in the from 1.7.1.4 or newer version.
The new function explains how to change rails while driving and how to change speed.

###  <a name="sampleroute"></a>Example route and example car data of outer-view

{{% indent %}}

The samples are prepared that is for all of the route data explained in this tutorial.
Both the sample route and the outer view of the JNR 103 series train are in the public domain, and can be downloaded in the OpenBVE package format.

{{% indent %}}

Route data:

<http://openbve-project.net/files/TFODemo_Routes.zip>

The outer-view of JNR Series 103:

<http://openbve-project.net/files/TFODemo-Series103.zip>

{{% /indent %}}

### <a name="filelist"></a>The file list of sample routes

{{% table %}}

| route file name                     | Overview                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| TFODemo_basic.csv                   | It has two stations on a double track, with oncoming trains coming from the terminal station to the departure station. |
| TFODemo_basic2.csv                  | It has two stations on a double track, and oncoming trains return to and from the departure station. |
| TFODemo_three-stations.csv          | It has three stations on a double track, and oncoming trains stop at each station from the last station to the intermediate station and the departure station. |
| TFODemo_single-track-switching.csv  | If you are drive or jump to intermediate station, the driving train will cross the oncoming train on a single line at intermediate station. |
| TFODemo_single-track-switching2.csv | If you are drive or jump to intermediate station, the driving train will cross the oncoming train on a single line at intermediate station.<br />Oncoming trains change their tracks at the intermediate station and stop on downbound line of the departure station. |
| TFODemo_passings.csv                | If you are drive or jump to intermediate station, the another train has overtaking from behind. |
| TFODemo_passings2.csv               | If you are drive or jump to intermediate station, the another train has overtaking from behind.Similarly, the upbound line is overtaken when an oncoming train stopped on the waiting line. |
| TFODemo_passings3.csv               | If you are drive or jump to intermediate station, the another train has overtaking from behind.Similarly, the upbound line is overtaken when an oncoming train stopped on the waiting line.<br />Trains that have been overtaken will change their tracks at intermediate station and stop on the downbound line of the departure station. |
| TFODemo_N-style.csv                 | A train moves such as N-shape, change points, and come to the departure station from waiting line.<br />POI is set at the point to check easily. |
| TFODemo_Train-addition.csv          | A 2M2T train arrives from the terminal station, combine with the another 2M2T train from the side waiting line, becomes 4M4T, and heads for the terminal station. |
| TFODemo_Train-disconnection.csv     | A 4M4T train comes from the terminal station to the departure station's upbound line, and sepalated.<br/>A 2M2T train moving towards the tarminal station retreats to the side waiting line, and an another 2M2T train on the departure station goes to the terminal station. |
| TFODemo_points.csv                  | It will be used to explain the new Points section that will be added in the future version.<br />A TFO train is departure at departure station and move to terminal station. The point is changed from the departure station to the terminal station, but the track is switched while using only the actual rail, and deceleration and acceleration are performed before and after the point. |

{{% /table %}}

{{% /indent %}}

## <a name="howtousetfo"></a> ■ How to use TFO

To use the TFO, we need an object to move.
This time, to study how to move a train, use the built in public domain Series-103.

1.Prepare a TFO train object

In order to prepare TFO train yourself, you must compose it as a train, so you need Train.dat with minimum information and csv or Animated object and Extensions.cfg for each vehicle.
Train.dat must contain the Motor and Trailer ratio of the train and how many cars have a train, and the weight etc. 
Must be described with the default values generated by the editor at a minimum because it is simulated. If a cars waight is too light, there is a risk of derailment.
Since the operation is performed by TFO, the default value set in TrainEditor or TrainEditor2 is sufficient.

Set the Motor and Trailer cars ratio of a train, and how many cars have a train, at least with these tools and output with default values.

Excluding the cab panel (which is not used even if it is not included), TFO train with motor acceleration / deceleration sounds can be sounds according to the speed.

TFO caluclate to animated object and interprets all commands except safety system. TFO's animated object can interpret parameters such as LeftDoors and ReverserNotch.
For example, if the running direction of the train changes in TFO, ReverserNotch will also be linked. It also switch headlights and taillights.
Animation by train speed is also possible.

You can also open and close the door when the TFO train stop.

The included Series-103 can rotates wheels according to speed, opens and closes doors, and interlocks headlights and taillights.

2.Prepare a XML file

Prepare a XML file that describes the operation of TFO. For example, 'TFODemo_basic.XML'.

3.Read XML file into route data

At the route csv file,

Route.TfoXML(TFO_XML/TFOXML-basic.XML)

write function such as above, then OpenBVE read the XML file to the route data.

In OpenBVE 1.7.1.1 and later, the TFO train searches for and reads 'relative path from TFO.XML file folder under Route' or 'relative path from Train folder'.

For earlier versions from 1.6.0 to 1.7.0.4, search and load from the 'relative path from the folder of the TFO.XML file under Route'.

If you follow these steps, you can moving the TFO train.

## <a name="tfo-characteristic"></a> ■ The characteristic of TFO

Regardless of the acceleration / deceleration value, if specified, the TFO will automatically accelerate / decelerate to the target StopPosition and stop.
Even if the distance corresponding to the acceleration / deceleration value is not enough, the speed is automatically adjusted to stop at the appropriate StopPosition.
Even if the specified speed has not been reached, it will automatically decelerate according to the above rules and stop at the appropriate StopPosition.

If you want to more real moving animation from these rules, you need to take appropriate acceleration / deceleration and the required distance.



## <a name="Firsttheme"></a> ■ First theme

Let's create a TFO train that start with a double track, move from the terminal station to the departure station,  that will disappear when the time comes.
The sample route data using this TFO is 'TFODemo_basic.csv'.

This sample route is a simple double track with a departure station at a distance of 100m and a terminal station at 400m.

The driving rail is Rail0, and double track's rail is Rail4.

The corresponding TFO.XML is 'TFO_XML / TFOXML-basic.XML'.

Let's look at TFOXML-basic.XML.
{{% code %}} 
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>400</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--Set on the TFO rail and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--Stops to the departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}} 

## <a name="Individual-configuration-of-XML"></a> ■ Individual configuration of XML

XML file need describe the header

```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
```

Each sections are roughly divide and describe about...

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
	  Define part 
    </Definition>	
    <Train>	
      Define target TFO train's folder, csv object or animated file.
    </Train>	
    <Stops>	
      <Stop>
        command
      </Stop>	
      <Stop>	
		command
	  </Stop>	
      <Stop>	
		command
      </Stop>
      ・
      ・
      ・
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	

```
{{% /code %}}

The XML is such as above constitution.

Let's look at each section.

## <a name="Definition-section"></a> ■ Definition section

Definition section makes basic settings for TFO.
Definition section has some subsections, and at here, where you define the behavior.

{{% indent %}}

#### <a name="AppearanceTime-subsection"></a>AppearanceTime subsection

AppearanceTime determines when a TFO object will appear on a route.
If it is exactly 12:00, it will be 12.0000.
In the case of 18:30:46, it is 18.3046.

#### <a name="AppearanceStartPosition-subsection"></a>AppearanceStartPosition subsection

#### <a name="AppearanceEndPosition-subsection"></a>AppearanceEndPosition subsection

AppearanceStartPosition and AppearanceEndPosition determine what the area of the position of 'driving train'.
If a driving train distance area within from AppearanceStartPosition and to AppearanceEndPosition, the TFO object is appears in 3D space.

For example, if you are now driving distance is 1250m, and AppearanceStartPosition is set to 1000m and  AppearanceEndPosition is 2000m, the TFO train is appears.
If you are now driving distance at 2500m now, the TFO train is not appears.
You will be able to make as conditional expressions above.

Additionally, if you are set to AppearanceTime, conditional expressions can add to driving train's appearance distance area.

For example, if the time is 12:10:15(just in time arriving a station), and a driving train is at station, the distance is adjust to 1000, the TFO train is seems to overtaking from behind.
This case, you can set to...
AppearanceStartPosition:990(1000-10)
AppearanceEndPosition:1010(1000+10)
AppearanceTime:12.1015

#### <a name="LeaveTime-subsection"></a>LeaveTime subsection

Specify the appearing time at the 3D space.
From AppearanceTime, LeaveTime has passed, the TFO train remove from 3D space.
If you want the TFO object to leave the 3D space after 10 minutes, specify 00.1000.

{{% /indent %}}

## <a name="Train-subsection"></a> ■ Train section

In the Train section, specify the TFO object. Although there is set a TFO train at this tutorial, you can set to the other animated objects or csv objects are also specified here.

{{% indent %}}

#### <a name="Directory-subsection"></a>Directory subsection

In the Directory subsection, you can specify animated objects and TFO trains.
For objects such as animated objects and csv, up to the file name, for TFO trains, specify the folder where the train is located.

For example:

TFO train								<Directory>Series-103</Directory>
the Animated object	<Directory>animated/test.animated</Directory>
csv object	<Directory>a\b\test.csv</Directory>

The mentioned above, the TFO train is now defined.
Next, let's try to move the TFO train!

{{% /indent %}}

## <a name="Stops-section"></a> ■ Stops section

To move the TFO train, write some Stop subsections at Stops section.

The Stop subsection commands, there has two parts.
The 'Stopping part' commands are used to stop the TFO train, and 'Moving part' commands are used to move the TFO train. 

{{% table %}}
| Stopping part                          | Moving part                                    |
| -------------------------------------- | ---------------------------------------------- |
| Decelerate,StopPosition,Doors,StopTime | StopTime,Accelerate,TargetSpeed,Direction,Rail |
{{% /table %}}

This sample is an instruction to move from the last station to the first station.
We will first describe the instructions to put on the 3D space and move.
For future reference, all commands are listed below.

{{% indent %}}

#### <a name="Decelerate-subsection"></a>Decelerate subsection

Decelerate subsection specifies the deceleration [km / h / s] when decelerating from the speed inherited from the previous Stop subsection.

#### <a name="StopPosition-subsection"></a>StopPosition subsection

The StopPosition subsection specifies the in-game distance [m] at which the TFO object will stop.
It is also used when specifying the appearance position.

#### <a name="Doors-subsection"></a>Doors subsection

Doors subsection specifies whether to open and close doors when you stop. -1 or L: left, 0 or N: do not open, 1 or R: right, B: open both sides.

#### <a name="StopTime-subsection"></a>StopTime subsection

The StopTime subsection specifies the stop time [hh.mmss] what is after put in the 3D space or after stop.
If it is 00.0000, it will be departure immediately when put in the 3D space or after stop immediately moves to.

If you want to stops the TFO train one minute and 45 seconds, write 00.0145.

#### <a name="Accelerate-subsection"></a>Accelerate subsection

Accelerate subsection specifies the acceleration [km / h / s] of the train that started moving.

#### <a name="TargetSpeed-subsection"></a>TargetSpeed subsection

The TargetSpeed subsection specifies the target speed after acceleration / deceleration.

#### <a name="Direction-subsection"></a>Direction subsection

Direction subsection specifies the direction of the TFO object that you want to move. 1 is the forward direction, -1 is the backward direction.

#### <a name="Rail-subsection"></a>Rail subsection

The Rail subsection specifies the rail number you want to moving.

{{% notice %}}
#### <a name="RailX-number-restriction"></a>About RailX number restrictions

If we want to use from BVE2/4's route, the BVE2/4 is limit of RailX from 0 to 15.<br/>From OpenBVE1.4.3, the RailX is unlimited.

{{% /notice %}}

If you want to add a TFO to the route for BVE2 / 4, you can specify a number greater than 15 as the RailX number have been unlimited from OpenBVE 1.4.3 and later as described above.
Using this difference from BVE2/4 and OpenBVE, prepare a csv object that describes only Createmeshbuilder at the same coordinates, that is, a transparent object.
And make it a rail, set to a transparent rail.
If you do the actions it above, you can easily add it by overwriting the route you want to run You can.

Even if you create a new route, it would be more efficient to make points on the real rail and move on the transparent rail.

Now, let's look at the Definition section of the first assignment.

{{% code %}}
```
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>400</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>
```
{{% /code %}}

ApperaceTime, the appearance time of the TFO train, is 12.0000. Which is exactly 12:00pm.
The start time of the route data is also exactly 12:00, so it will be displayed immediately after the game starts.
AppearanceStartPosition and AppearanceEndPosition are the triggering conditions of where a driving train is places between with the two distances, but it is set so that it will appear when the driving train placing at the all route data's distance, from 0 to 400m.
LeaveTime is 00.0200, which means it will disappear from 3D space two minutes after being set to the 3D space.

{{% /indent %}}

## <a name="first-Stop-subsection"></a> ■ The first Stop subsection

Let's look again at the commands in the first Stop subsection of the first assignment.

{{% code %}}
```xml
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>
```
{{% /code %}}

These are the instruction sets to placement to 3D space and move the train first.

It starts from a stopped state at first. Also, since the speed is not inherited because of the first Stop section, and also Decelerate does not need to be set, so it is 0.00.
StopPosition is the distance to set a TFO train. Here, it is set at a distance of 400m.
If the TFO is a train, the first car will be placed at this distance, after the another car places and connect obey Extensions.cfg to the departure station direction.
Doors are specified when the TFO train is opening and closing at the appearance time. Here, 1 is specified to open and close that is the right side door to the direction of travel.
Despite that, the StopTime is 00.000 seconds, that means no time, so the TFO train's door is open and close immediately, and departure.
As soon as the TFO train is open and close the door and depart, accelerate at the acceleration 1.71 specified in Accelerate.
After acceleration, accelerate up to 60km / h specified by TargetSpeed.
The TFO trains follow Direction's -1 and move backward.
Rail number what the TFO train is trying to move is 4.

## <a name="Stop-TFO-train"></a> ■ Stop the TFO train

With the above instructions, the TFO train will start moving. The following command will stop the TFO train at the departure station.

{{% code %}}
```xml
      <!--Stop the TFO train at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Decelerate is "The deceleration when decelerating from the speed inherited from the previous Stop subsection".
So the previous Stop subsection's speed was 60 km / h, so deceleration starts at 1.65 from 60km/h.
StopPosition is about 100m, which is the target stop position of the departure station.
Doors is specified as R, open the right door to the direction of travel after stopping.
StopTime is specified as 00.0030, so stop for 30 seconds.
This time, if the TFO train come to the departure station, nothing will happen until it disappears, so does not need to accelerate. So the Accelerate value is set to 0.
TargetSpeed is also 0 as it does not need to be accelerated.
Direction is also does not need to be moved anymore, but it specifies -1 which is the same direction as when it came for the time being.
The moving rails will continue to be 4.

This concludes the first task.
Now, you can move the TFO train from the final station to the departure station.

Next, let's move back to the final station.

## <a name="To-return-from-the-departure-station"></a> ■ To return from the departure station to the terminal station

In this sample, the route data is 'TFODemo_basic2.csv' and TFO.XML is 'TFOXML-basic2.XML'.
Route data is the same, the only difference is that TFOXML-basic2.XML is specified in Route.TfoXML.

Let's take a look at TFOXML-basic2.XML.

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>400</AppearanceEndPosition>
        	<LeaveTime>00.0230</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--Stop at the departure station and back to the final station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--Stop at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

The Definition section, the Train section, and the first Stop section (set to the 3D space and getting started) are the same as the first task.
This time, the new task is to back at the final station and stop.

Let's take a look at the turnaround part at the departure station.

{{% code %}}
```xml
      <!--Stop at the departure station and back to the final station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

The TFO train that arrives at the departure station at 60 km / h from the terminal station will start to decelerate at deceleration of 1.65.
The stop position is distance of 100m, written at StopPosition.
After stopping, open the right side doors in the direction of travel obey the Doors value of  R.
StopTime is 0.0030, so stop for 30 seconds.

The process so far is exactly the same as the first task.
Let's enter the actions to start departure from departure station.

Specify the acceleration to move with Accelerate, and accelerate with acceleration rate of 1.71.
The TargetSpeed is 60, this is after acceleration speed, so it is 60km / h.
The direction of movement is Direction 1, so it is the direction of forward.
Since Rail specifies 4 for the rail to be moving, moving on Rail4.

Write to these commands, the TFO train will stop at the first train station, door open and close, turn it back and depart.
Next, enter the command to stop again at the terminal station.

{{% code %}}
```xml
      <!--Stop at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>400</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

The TFO train that arrives at the terminal station at 60 km / h from the departure station will start to decelerate at deceleration of 1.65.
StopPosition is 400, which is the stop position of the terminal station.
Doors should be R, and after stopping, open the right door.
The stop time is 30 seconds by StopTime, and the door is closed when the time has elapsed.
Accelerate is 0 because TFO train don't move any more.
The TargetSpeed is also 0km / h because TFO train doesn't move any more.
The direction does not need to be changed because TFO train does not move more, so Direction remains 1.
Rails to move are still 4 because TFO train do not move.

Finally, after LeaveTime has passed, it will disappear from 3D space.

## <a name="How-to-stops-at-each-station"></a> ■ How to move several stations and stops at each station

In this sample, the route data is 'TFODemo_three-stations.csv' and TFO.XML is 'TFODemo_three-stations.XML'.
Route data is a simple double track with an intermediate station at 600m and a terminal station at 1100m.

For these three stations, on double track will be moving at each station, from the terminal station via intermediate station to the first station.
Let's take a look at the XML file.

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>1100</AppearanceEndPosition>
        	<LeaveTime>00.0400</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>600</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

First, let's look at the Definition section.

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>1100</AppearanceEndPosition>
        	<LeaveTime>00.0500</LeaveTime>
    </Definition>	
```
{{% /code %}}

This route data starts at exactly 12:00.
AppearanceTime is exactly 12:00, so it will be set at the start of the route.
AppearanceStartPosition and AppearanceEndPosition are 0 and 1100, respectively, and they always appear by specifying all area of the route.
LeaveTime is 00.0500, so it disappears after 5 minutes.

The 'Train' section do not explanation at here ,because is only set folder.

{{% code %}}
```XML
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>1</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Since it is the first arrangement, it will not take over and decelerate with Decelerate, so specify 0.
StopPosition specifies the distance 1100 which is the stop position of the terminal station.
The door is treated immediately after appearance, so specify 1 and open the right door.
After the doors are opened, to stop the TFO train, set to 30 seconds with a StopTime of 0.0030
To move the TFO train, Accelerate with 1.71 acceleration.
After acceleration, specify 60 in TargetSpeed and accelerate up to 60km / h.
Because the TFO train moves from the terminal station to the departure station, Drirection specifies -1.
Specify Rail as 4 for the rail to move.
And TFO train is moveing on Rail4.

Next, stop at the intermediate station, doors are open and closed, and depart again toward the departure station.

{{% code %}}
```XML
      <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>600</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Since TFO train is accelerating up to 60km / h, we will take over the speed from previous Stop subsection, and decelerate at 1.65.
StopPosition is a stop position at 600m away from the intermediate station.
Operate to open and close doors, set Doors to R and open the right side.
The stop time is 00.0030 in StopTime and the vehicle is stopped for 30 seconds.
After departure, accelerate with Accelerate 1.71.
Set TargetSpeed to 60 and accelerate to 60km / h.
The direction to run is the direction of the departure station, so set Direction to -1.
The rail to move is set to Rail 4, and TFO train moves on Rail4.

After stopping and departure of the intermediate station, finally, stop at the departure station.

{{% code %}}
```XML
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Next, let's make a cross the oncoming train on a single rail at station.

## <a name="How-to-cross-the-oncoming-train"></a> ■ How to cross the oncoming train on a single rail at station

The sample route used this time is 'TFODemo_single-track-switching.csv'.
The corresponding TFO.XML is 'TFODemo_single-track-switching.XML'.
If you drive to the intermediate station on this line or make a selection by jumping at the station, the oncoming train of TFO will cross.

From here on, that will be complex example of situation.
As you know, if you make cross the oncoming train on a single track, you need to arrange a rail that branches off from your own track.

![TFODemo_single-track-switching_route-base](/images/TFOTutorial/EN/TFODemo_single-track-switching_route-base-EN.jpg)

This time, the sample route has shown as above layout.
The driving line runs as shown below.
![TFODemo_single-track-switching_route-base-rail0](/images/TFOTutorial/EN/TFODemo_single-track-switching_route-rail0-EN.jpg)

I used Rail1 and Rail4 to set the tracks as shown below.
![TFODemo_single-track-switching_route-base-rail1&4](/images/TFOTutorial/EN/TFODemo_single-track-switching_route-rail1&4-EN.jpg)

As mentioned earlier, since OpenBVE 1.4.3 has no restrictions on Rail numbers, use it to prepare a CSV object that describes only CreateMeshBuilder in Rail16 or higher numbers, and use to transparent rail. By placing transparent rail on the conventional rails, you will be able to run.
Of course, if all the paths to be moved are connected, as in the previous samples, you can also run the actual rail.
This time, the transparent rail is layouted as shown below.
![TFODemo_single-track-switching_route-base-rail20](/images/TFOTutorial/EN/TFODemo_single-track-switching_route-rail20-EN.jpg)

Keep in mind that TFOs are "moving animated objects" and "not trains sharing the same track."

Taking advantage of it, the TFO of a oncoming train runs "only while arriving at an intermediate station".
Now use the these functions for trigger.
AppearanceTime
AppearanceStartPosition
AppearanceEndPosition
Using these, you can specify the time and distance when you are stopped at the intermediate station, and activate TFO train only when the conditions are met.

After explain these points, look at the csv route data.
The timetable and distance of the intermediate station that moves the TFO are as follows.

{{% code %}}
```
850
	.Sta(intermidiate station; 12.0500; 12.0800; ; 1; 1; ; ; ; 5; ;)
1050	
	.Stop(1,1,1)
```
{{% /code %}}

The stop position is located at a distance of 1050.
If the driving train cannot stops at the stop position, it is possible that the driving train has exceeded the points before and after or the contact limit.
Therefore, the distance between AppearanceStartPosition and AppearanceEndPosition needs to be set to do not over the contact point.

Looking at the schedule, the train arrives at the intermediate station at 12:05 and departs at 12:08.
Within that time, a TFO train arrives and must stop at the station.
In this example, the command to move TFO was specified in consideration of them.

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.04</AppearanceTime>
        	<AppearanceStartPosition>1040</AppearanceStartPosition>
        	<AppearanceEndPosition>1060</AppearanceEndPosition>
        	<LeaveTime>00.0430</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
      <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

Let's talk about the Definition section, which will be more useful from this time.

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.04</AppearanceTime>
        	<AppearanceStartPosition>1040</AppearanceStartPosition>
        	<AppearanceEndPosition>1060</AppearanceEndPosition>
        	<LeaveTime>00.0430</LeaveTime>
    </Definition>	
```
{{% /code %}}

Again, the schedule and distance of the intermediate station that runs the TFO are as follows.

{{% code %}}
```
850
	.Sta(intermidiate station; 12.0500; 12.0800; ; 1; 1; ; ; ; 5; ;)
1050	
	.Stop(1,1,1)
```
{{% /code %}}

The stop position is located at a distance of 1050.
In other words, before your train arrives by 12.0500, you need to prepare the conditions for the TFO train to appearance.
For this reason, we set the ApperanceTime one minute before the train arrives at the station.

AppearanceStartPosition and AppearanceEndPosition have been specified for 1040 and 1060 to be activated, that is, when the vehicle is within 10m of the front and rear of the stop position of the intermediate station.

Leavetime will now disappear after 4 minutes and 30 seconds.

Next, let's look at the first command of the TFO train to be moved when the trigger is activated.

{{% code %}}
```XML
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

When first Stop section, Decelerate is 0 because it does not need to inherit the speed from the previous Stop.
This time StopPosition was in time what is coming from terminal station to intermediate station, so we decided to stop at the station and started from 2100m.
Doors are not open because TFO train wanted to take them to an arbitrary speed without open doors.
待機する時間がないため、StopTimeは0です。
Accelerate is a whopping 10. This is a technique that can be avoided by setting a large acceleration if you want to run at an arbitrary speed immediately after set to 3D space.

The Direction is -1, and the moving rail is on transparent Rail20.

The TFO train is now set to react when your train arrives at the station or jumps.

Next, let's take a look at the TFO train stopping at the intermediate station and departing.

{{% code %}}
```XML
      <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

Other than moving on a transparent track, you will notice that nothing has changed.

Decelerate to decelerate at deceleration 1.65, stop at 1050 by specifying StopPosition, open and close the left side with Doors set to L, StopTime is 00.0030, stop for 30 seconds, then accelerate with Accelerate1.71, accelerate at TargetSpeed 60 km / h, Direction is -1, running rail is transparent Rail20.

As shown above, stop at the intermediate station and depart.
Finally, stop at the departure train station and wait for the time to disappear.

{{% code %}}
```XML
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

Decelerate with decelerate rate 1.65
StopPosition is about 100m from the stop position of the departure station
Set Doors to L and open the left side
Stop time is 00.0030 in StopTime, so stops 30 seconds
Accelerate is 0 because there is no need to move again after stopping
TargetSpeed is 0 because there is no need to move
here is no need to move too, so set to Direction 1
The moving rail is set to 20 without changing the rail.
Specify the commands above.

## <a name="How-to-change-to-the-other-rail"></a> ■ How to change to the other rail

The sample route used this time is 'TFODemo_single-track-switching2.csv'.
The corresponding TFO.XML is 'TFODemo_single-track-switching2.XML'.

I didn't mention it earlier, but in fact, this sample route has another transparent rail, Rail21.
I did not explain it because it was confusing, but I set this transparent rail in this way.

![TFODemo_single-track-switching_route-base-rail21](/images/TFOTutorial/EN/TFODemo_single-track-switching_route-rail21-EN.jpg)

Track replacement is a function that allows you can change TFO objects moving rail between overlapping rails.
However, **be sure to stop TFO** when replacing tracks.

<u>※It does not cause an error if you change the line while driving, but be careful as the train stops once.</u>

Therefore, if the TFO's action want to looks more to real, I think it is more natural to stop it once.

The another thing,  if you are attempt to change rail, you must to **all TFO train cars must set the adjust coordinate on the before changing rail and after changing rail are overlapping, and have to on the both rails.**

Be careful not to change rail in an incomplete state, such as when the track is not on the way, as derailment, loss of vehicles, or runaway of OpenBVE may occur.

Now that we've talked about necessary topic, let's take a look at the XML that we will replace.

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.04</AppearanceTime>
        	<AppearanceStartPosition>1040</AppearanceStartPosition>
        	<AppearanceEndPosition>1060</AppearanceEndPosition>
        	<LeaveTime>00.0430</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
      <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

The Definition section is unchanged.

{{% code %}}
```XML
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

What we want you to pay attention to here is Accelerate.

Accelerate is a whopping 10.
The reason is that oncoming trains that arrive during a crossing stop do not always leave the station and start moving.
Therefore, it is possible that it is moving at a predetermined speed immediately after placing the TFO train.
To achieve that, they set the acceleration higher to make them look like they were driving from the beginning.

By the way, it is time to depart from the intermediate station when you say where the rails are changing.

{{% code %}}
```xml
      <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1050</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

The contents are almost the same here, but only the Rail command is changed.

Here we are changing from Rail20 to Rail21.
Just use this function, you can change the track easily.
Rail21 branches off the point and arrives at the departure station, so the TFO train can move like that.

The command to arrive at the departure station is not particularly changed, but the rails to be move have been switched to Rail21, so only that point has been changed.

{{% code %}}
```XML
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

Application this technique, it is also possible to move on Rail 21 first, stop at the intermediate station, and replace it with Rail 20.

## <a name="How-to-overtaking"></a> ■ How to overtaking from behind by TFO train

Overtaking is realized by applying the single-track cross oncoming train of the previous example, setting a triger of the conditional expression that is for the time and distance while stopping on the waiting line, and moving from back to front.

The sample route used this time is 'TFODemo_passings.csv'.
The corresponding TFO.XML is 'TFODemo_passings.XML'.
If you drive to the intermediate station on this route data or make a selection by jumping to the station, the TFO train will pass from behind during the stop time.

TFO is **'moving animated objects'**, not trains that share tracks of the 3D world.

Therefore, if the TFO train is intentionally drive after the TFO train has been activated, it will overlap with the object during overtaking.
So, It is necessary to have a complete signal system and keep the train from moving.

This time, the sample route is like showing below.

![TFODemo_passing_route-base](/images/TFOTutorial/EN/TFODemo_passing_route-base-EN.jpg)

The driving rail is Rail0.

![TFODemo_passing_route-rail0](/images/TFOTutorial/EN/TFODemo_passing_route-rail0-EN.jpg)Transparent rails to overtaking is placed as Rail20.

![TFODemo_passing_route-rail20](/images/TFOTutorial/EN/TFODemo_passing_route-rail20-EN.jpg)

Using above rails, the TFO train will overtaking.

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0230</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving with overtaking train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>100</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
	
     <!--stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

As I mentioned earlier, overtaking only reversed the direction of single-track cross the oncoming train, so XML is almost unchanged.
It is important to set a stop time just enough to be overtaken at the intermediate station so that you can overtake.

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0230</LeaveTime>
    </Definition>	
```
{{% /code %}}

As with single-track overtaking to cross the oncoming train, the trigger conditions are very important.

{{% code %}}
```
900					
	.Sta(Intermediate station; 12.1000; 12.1500; ; -1; 1; ; ; ; 100; ;)
1000					
	.Stop(1,1,1)				,;stop position
```
{{% /code %}}

The stop time and stop position of the intermediate station are as above.
Use this stop time to move the TFO of the overtaking train.
Therefore, the AppearanceTime of Definition is set to 12.1000 at the time when a driving train is arrived on time  at the intermediate station.
AppearanceStartPosition and AppearanceEndPosition are set to stop position + -25m, and distance of 975m and 1025m.
Leave Time is set to 2 minutes and 30 seconds to clear immediately after arriving because if you do not overtake and stop the car and delete the car at the terminal station, it will overlap with the TFO that is not the preceding train.

{{% code %}}
```XML
      <!--set to 3D space and start moving with overtaking train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>100</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

Let's take a look at the first Stop command.

Decelerate is 0 because it is starting to move, StopPosition is 100m at the stop position of the departure station, Doors is 0, Accelerate is 10 because we want to top speed from the beginning to overtake without opening and closing the door.

TargetSpeed is 100km / h because the overtaking train is fast.
Since Direction is the direction of travel, the moving rail is set to 20, which is a transparent rail for overtaking.
The overtaking train moves the passing station, but there is no concept of passing the station because the TFO is an animated object.

The overtaken TFO train will be stopped at the terminal station, open and close doors, and then extinguished.

{{% code %}}
```XML
     <!--stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>20</Rail>
      </Stop>	
```
{{% /code %}}

Decelerate is decelerate rate 1.65, StopPosition is 2100m, which is the stop position of the terminal station, and after stopping, set Doors to L and open and close the left door.

Accelerate required for acceleration rate is not necessary, so it is 0, TargetSpeed is also not need to accelerate, so it is 0, Direction is 1 as it has proceeded so far, and Rail is 20 because there is no need to change the rail.

## <a name="How-to-overtaking-oncoming-trains"></a> ■ How to overtaking the oncoming trains each other(to move multiple TFO objects)

Route.TfoXML can be multiple placed in the route data.
If this allows, you can put and move multiple cars, buses and TFO trains to be placed in the route data.
This time, we create several TFO trains that is "While stopping at the intermediate station and waiting for overtaking, the anothter waiting train arrives at the intermediate station on the opposite upbound line and stops and is overtaken by itself by another TFO train".

The sample route used this time is 'TFODemo_passings2.csv'.
The corresponding TFO.XML are 'TFODemo_passings.XML', 'TFODemo_passings2.XML' and 'TFODemo_passings3.XML'. Each XML is...

TFODemo_passings.XML: TFO train overtaking itself
TFODemo_passings2.XML: TFO train that stops at an intermediate station on the up line and arrives at the departure station after being overtaken
TFODemo_passings3.XML: A TFO train that overtakes the upbound train at an intermediate station on the upbound line

Showing above is the configuration of XML.

This sample route adds TfoXML that reads the above XML to TFODemo_passings.csv.
Therefore, the the route formation does not change.

![TFODemo_passing_route-base](/images/TFOTutorial/EN/TFODemo_passing_route-base-EN.jpg)

As the same of previous sample, when the driving train arrives at the intermediate station, the TFODemo_passings.XML for overtaking is activated and the waiting TFO train comes to the platform on the upbound line by TFODemo_passings2.XML.
In addition, TFODemo_passings3.XML is also activated, and it passes through the upbound line.

The passing train on the upbound line passes on Rail4.

![TFODemo_passing_route-rail4](/images/TFOTutorial/EN/TFODemo_passing_route-rail4-EN.jpg)

The waiting train on the upbound line passes Rail21 on the transparent rail.

![TFODemo_passing_route-rail4](/images/TFOTutorial/EN/TFODemo_passing_route-rail21-EN.jpg)

In the actual measured value, It took about 1 minute and 30 seconds for an overtaking train on the upbound line to arrive at the intermediate station from the terminal station.
Adjust the departure time of the passing train have to consideration of waiting TFO train arriving time.
First, let's look at the Definition section of TFODemo_passings2.XML.

{{% code %}}
```XML
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0500</LeaveTime>
    </Definition>	
```
{{% /code %}}

AppearanceTime is set to 12:10.
Active condition distance command of AppearanceStartPosition and AppearanceEndPosition are at the stop position + -25m, and the distance is about 975-1025m.
LeaveTime is 4 minutes and 30 seconds. This was adjusted by actual measured value of the time it takes for a TFO train was overtaked by passing train at the intermediate station, and arrive the departure station and disappear for more waiting 30 seconds after open and close  doors for 30 seconds.

The TFO train that will waiting at the intermediate station is accelerate rate is Accelerate10 after departure, arrives at the intermediate station in about 1 minute 30 seconds, confirms that it will be open and close door time by actual measurement.
And the overtaking TFO train's start to moving time is set by that actual measurement time.
 See the first Stop subsection of FODemo_passings2.XML for details.

Take a look at the Definition section of the passing train.
Passing trains are described in TFODemo_passings3.XML.

{{% code %}}
```
    <Definition>	
        	<AppearanceTime>12.1000</AppearanceTime>
        	<AppearanceStartPosition>975</AppearanceStartPosition>
        	<AppearanceEndPosition>1025</AppearanceEndPosition>
        	<LeaveTime>00.0400</LeaveTime>
    </Definition>	
```
{{% /code %}}

The AppearanceTime of the overtaking train on upbound line is set at 12:10, as also the waiting train on upbound line, that is in accordance with the time and distance at which the driving train arrives at the intermediate station.
AppearanceStartPosition and AppearanceEndPosition also have a stop position of + 25m and a distance of 975-1025m.

The overtaking train on upbound line can move add to1m30s later from 12:10pm.
That is, after 12:11:30, the overtaking train can pass at intermediate station.
We have to find the time that can pass just timing and adjust the departure time of the passing train.

Take a look at the first Stop subsection of the passing train on upbound line.
The first Stop subsection of TFODemo_passings3.XML is as follows:

{{% code %}}
```XML
    <Stops>	
      <!--set to 3D space and start moving with oncoming train-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>2100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0130</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>100</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
```
{{% /code %}}

Decelerate is 0 because it does not slow down on the first placement.
StopPosition is about 2100m, which is the stop position of the terminal station.
Doors is set to 0 and doors do not open.

This time, the important thing is StopTime.

Here, set it to 00.0130, stop for 1 minute and 30 seconds, waiting at the terminal station.
1 minute 30 seconds is the time when the preceding train of oncoming line arrives at the intermediate station and it open the doors.
The passing train's departure time from the terminal station is as above time, 12:10:00 + 1m30s, 12:11:30.
It was measured that the passing train accelerated with the acceleration of Accelerate10 and passed the confluence point at the departure station side at 12:12:25.
That is, waiting train that are overtaken can depart after this time without collision or rear-end collision, but change the waiting time to make it a realistic time.

Now take a look at the Stop subsection of the train, that commands are departure from terminal station and to wait at the intermediate station.
TFODemo_passings2.XML

{{% code %}}
```xml
     <!--Stops at the intermediate station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1000</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0130</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

Take over the previous Stop subsection, decelerate from 60km / h at Decelerate 1.65 and stop at a distance of StopPosition 1000m.
To open the right side door,  set Doors is R.

Here, we set the StopTime to 1 minute and 30 seconds to match the time when the train passing on the confluence point of the depature station side passes.
The time when the passing train passes the confluence point is actually measured at 12:12:25, so set the time after that.
Since this is a demonstration route, we will start the train immediately with a stop time of 1 minute and 30 seconds without considering the signal system time so that the result can be seen immediately.

Each train will be stopped at the starting station by the time of extinction, and will be on standby, if necessary, as door open/close is required.
If you do not let the overtaking train disappear at the departure station first, the TFOs will overlap and it will not look real, so it is good to consider that as well.
In this demonstration route, the disappear time of the preceding train is exactly 4 minutes, and the disappear time of the overtaken train is 5 minutes.

## <a name="moving-on-points"></a> ■ How to moving on points

To pass the points, either form all the rails with the physical rails, or spread only the points with the physical rails, and then form all the paths with the transparent rails.
On routes already made for BVE2 / 4, Rail is limited to 15, so assign a higher number with a transparent rail and let it be read from another file with include.
Or, add the new transparent rail data at a same routefile's end of file.
I recommend these methods.

Rail numbers have been unlimited since OpenBVE 1.4.3, so let's assign them to your favorite numbers.

This time, using the same track layout as TFODemo_passings.csv and TFODemo_passings2.csv, change only the XML, and change to a transparent rail crossing the point from the intermediate station, and move to the departure station.

TFODemo_passings.csv, TFODemo_passings2.csv and TFODemo_passings3.csv already have Rail22 of transparent rails set.
![TFODemo_passing_route-rail4](/images/TFOTutorial/EN/TFODemo_passing_route-rail22-EN.jpg)

Rail22 is a transparent rail that starts from the point where the straight line on the terminal station side of the intermediate station has started, and crosses the scissors crossing point on the departure station side to reach the platform on the downbound line.

When the waiting TFO train leave the intermediate station, by replacing it with Rail22, the TFO train can stop at the downbound platform of the departure station.

The sample route used this time is 'TFODemo_passings3.csv'.
The corresponding TFO.XML are 'TFODemo_passings.XML', 'TFODemo_passings3.XML' and 'TFODemo_passings4.XML'. Each XML mean...

TFODemo_passings.XML:A TFO train overtaking from driving train
TFODemo_passings3.XML:A TFO train overtaking an upbound train at an upline intermediate station
TFODemo_passings4.XML:A TFO train that stops at an intermediate station on the upbound line and arrives at the departure station after being overtaken. Change to Rail22 at the intermediate station

The TFO file list is showing above.

'TFODemo_passings.XML' and 'TFODemo_passings3.XML' are the same as those described in "How to overtaking the oncoming trains each other (to move multiple TFO objects)", so they are omitted here.

The newly prepared TFODemo_passings4.XML is basically the same as TFODemo_passings2.XML.
Let's take a look at the Stop subsection of TFODemo_passings4.XML, which stops at the intermediate station and departs.

{{% code %}}
```XML
     <!--Stops at the intermediate station, door open and close, and departure-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1000</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0130</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
```
{{% /code %}}

The only change here is Rail, which changed from Rail21 to Rail22.
As this above, you can do it that the upbound line's waiting train is moving on Rail22 and cross the point of the departure station to stop at the platform.

Below is the Stop subsection that stops at the departure station in TFODemo_passings4.XML

{{% code %}}
```xml
     <!--Stops at departure station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
```
{{% /code %}}

When arriving at the departure station, the platform is downbound side, so Doors is set to L.
You can do now that the TFO train can move across points.

## <a name="How-to-do-more-complex-points"></a> ■ How to do more complex action across points

If you want to make complicated turns, such as a locomotive move to the waiting line or into garages with moving on some points, attach a transparent rail on the path you want to move in advance.
Then you can move any transparent rail by changing Rail and Direction as you want.

This time, let's move the point while moving back and forth in N-shaped moving.
The arrangement of the sample route this time is as follows.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-base-EN.jpg)
The following numbers are used for each rail.
![TFODemo_passing_route-rail5](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail0-EN.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail1-EN.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail2-EN.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail3-EN.jpg)
![TFODemo_passing_route-rail5](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail4-EN.jpg)

I will talk about from Rail5 because of the explanation of the move.![TFODemo_passing_route-rail5](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail5-EN.jpg)

The TFO train departing from the departure station side of Rail5 will head forward.
Then, to cross the point and enter the departure station, change to Rail22.
When the TFO train arrive at the departure station, open the right door.

![TFODemo_passing_route-rail22](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail22-EN.jpg)

After closing the door, moving on Rail21 and toward the terminal station.

![TFODemo_passing_route-rail21](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail21-EN.jpg)

Let's take a look at the whole TFODemo_N-style.XML.

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.1000</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>275</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>5</Rail>
      </Stop>	
	
      <!--Stops ahead at the waiting line, change to Rail22, start to move towards the departure station of upbound line-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
	
     <!--Stops at the departure station of upbound line, change to Rail21, and moving towards to ahead-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
     <!--Stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

Let's look at the individual elements.

{{% code %}}
```xml
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.1000</LeaveTime>
    </Definition>	
```
{{% /code %}}

There is nothing special about the Definition section, and it will be activated as soon as the route data starts, and distance trigger is a driving trains departure station's stop position.
LeaveTime is set to the disappearance is 10 minutes, but there is no particular basis, so just to secure the time to finish all the movement and leave it as it is.

{{% code %}}
```xml
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>275</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>5</Rail>
      </Stop>	
```
{{% /code %}}

First place it on Rail5 and move it forward from a distance of 275m.
Now then, I think that you understand the meaning of the commands, from here, only the characteristic parts will be explained.

{{% code %}}
```xml
      <!--Stops ahead at the waiting line, change to Rail22, start to move towards the departure station of upbound line-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0010</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>25</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
```
{{% /code %}}

Decelerate at a deceleration of 1.65 and stop at a distance of 475m ahead of the side line. Here, set Direction to -1, change to Rail22 and accelerate to 25km/h.

{{% code %}}
```xml
     <!--Stops at the departure station of upbound line, change to Rail21, and moving towards to ahead-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>60</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

Travel on Rail22 and stop at a distance of 100m on the upbound line of the departure station, and open the right door.
Stop for 30 seconds, accelerate forward at an acceleration rate 1.71, and accelerate up to 60 km/h.
The moving rail is switched to Rail21.

{{% code %}}
```XML
     <!--Stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
```
{{% /code %}}

Finally, stop at the terminal station, open the left door, stop for 30 seconds, close the door, and wait until it disappears.

If you prepare a route across points like this, you can move it just by switching between Rail and Direction.

## <a name="combine-two-or-more-TFO-trains"></a> ■ How to combine two or more TFO trains and how to move a combined TFO train

In order to combine locomotives, passenger cars, and electric trains together and move them as one train, the method of "disappearing and replace" is used.
In other words, until combine, each train is moving each other. Immediately after finishing door close, they are deleted, replaced with new a combined train, and it moves.

Series-103 is 2M2T, so combine with the another 2M2T, then a train is 8 cars of 4M4T.
First, prepare 8 cars of combined 2M2T.
This time, we use the included 8-car train (Series-103-4M4T) because it is made in advance.

The arrangement of this sample route is the same as the wiring used for "How to do more complex action across points".
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-base-EN.jpg)
First, a TFO train that is before combine, moves to departure station on Rail4.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail4-EN.jpg)
The other TFO train will waiting on Rail22 that is the side rail in advance, stop in front of the combine train at the departure station, after then, it connect.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail22-EN.jpg)
Close the doors of both TFO trains, make them disappear, replace them with 4M4T trains, and moves on Rail21 toward the terminal station.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail21-EN.jpg)
The eight-car train arrived to the terminal station disappears after open / close doors.

First, let's look at the XML of the train moving on Rail4 towards the departure station.

TFODemo_Train-addition.XML

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>500</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>0</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--Stops at the departure station upbound line-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0115</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

From a distance of 500m, after the start of the route, accelerate at an acceleration rate of 10 and come to the departure station on Rail4.
Stop at 1 minute and 15 seconds with door open.
It took about 1 minute and 57 seconds for all operations to take place until the other train arrived, combine and finished close the door.
The door will be closed accordingly. StopTime was that time, 1 minute and 15 seconds.
The TFO train is extinguished exactly two minutes, and at that time it is replaced with 4M4T.

Let's look at the other hands TFO train.

TFODemo_Train-addition2.XML

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
	
      <!--To combine at the departure station upbound line, stop temporarily before combine, and move-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>180.25</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0005</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>3</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
      <!--Combine at the departure station upbound line-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0047</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

This TFO train is set to the same disappear time that is 2 minutes.
What is important here is that all the doors must be link with each TFO train and open / close at the same time, and disapper time is the same time.
And each TFO trains distance to stop must be exactly the same as the distance to be connected.

I try it many times and confirmed the position to link and display correctly.
Finally, I found that the distance when combined was 179.25m.
And the time to open and close the door was 47 seconds.
After moving to the departure station, it stopped temporarily, waiting for 5 seconds, then combined, door open and closed, and all operations took about 1 minute 57 seconds.

The disappear time was set to 2 minutes for both trains, so ready to change the two TFO trains.

Finally, let's take a look at the 4M4T that will be replaced and departed.

TFODemo_Train-addition3.XML

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0200</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0210</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103-4M4T</Directory>
    </Train>	
	
    <Stops>	
      <!--Replace and start to move-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
      <!--Stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

The time to replace and start is exactly 2 minutes after the game starts, so the AppearanceTime is exactly 12:02.

A 4M4T train's Extensions.cfg is must create exactly the same state as the each 2M2T state.
Otherwise, it will gap when you replace it.
StopPosition at the time of arrangement must match exactly with the distance of the combined train.
The distance when connected was 179.25m. In accordance with this, the StopPosition is set to 179.25m according to the distance of this arrangement.
After that, moving on Rail21, and arrive at the terminal station, open / close door and let it disappear.

The same procedure is used for merging three or more trains.

## <a name="separate-a-train"></a> ■ How to separate a train and moving each TFO train

After the train arrives, to split it into separate trains and move each one, do the opposite of combine.
It's easy to say, but it's hard to adjust distance.

The arrangement of this route will be the same as the route used for the combine.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-base-EN.jpg)
The 4M4T train to split comes first through on Rail4 to departure station.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail4-EN.jpg)
After arriving, open / close door, and the formation of the terminal station will enter the side line through Rail22.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail22-EN.jpg)
Finally, the departure station's side 2M2T is going through Rail21 and toward the terminal station.
When a TFO train arrive at the terminal station, open / close door and it will disappear.
![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail21-EN.jpg)

First, let's look at 4M4T's XML.

TFODemo_Train-disconnection3.XML

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0111</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103-4M4T</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>10</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--Stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0000</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>4</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

The LeaveTime of Definition is 00.0111, That is, 1 minute and 11 seconds.
This value is very important.

This is the actual measurement of the time to stop, that is the depart from the departure station, accelerate at a distance of 1100m from according to the first Stop subsection's the settings of each subsection, at accelerate rate 10.
And according to the next Stop subsection's each subsection, from 75m / h decelerate from deceleration rate 1.65, and stops at a distance of 179.25m.

A 4M4T TFO trains disappear immediately after arriving. At the same time, two trains of 2M2T appeared to match all eight cars, and the doors of each train were opened immediately afterwards.

Next, let's look at the XML of the train of the terminal station side.

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0111</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0200</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>179.25</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>22</Rail>
      </Stop>	
	
      <!--Move to the waiting line and stop-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>475</StopPosition>
        	<Doors>0</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>3</TargetSpeed>
        	<Direction>-1</Direction>
        	<Rail>22</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

Definition's AppearanceTime is 12.0111, that is, 12:01:11.
Please remind what I told that it took 1 minute and 11 seconds before the 4M4T train arrive at the departure station.

The departure time of 4M4T is exactly 12:00, it takes 1 minute and 11 seconds to arrive and stop.
The replacement time is changed after 1 minute and 11 seconds according to that, so the AppearanceTime is 12.0111, that is, 12:01:11.

The replacement distance is also very important, the first Stop subsection's StopPosition is 179.25.
This must be exactly as the distance as the 4M4T stops, and it adjust exactly the same.

After that, the TFO train on the terminal station side will be open / close door, moving on Rail22, and head for the waiting side line.

Finally, let's look at the train on the departure station side.

TFODemo_Train-disconnection.XML

{{% code %}}
```xml
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0111</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.0320</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0100</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>75</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
	
      <!--Stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1100</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>21</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

For the departure station side train, the AppearanceTime of Definition is also 12.0111, 12:01:11.
The StopPosition in the first Stop subsection is also very important, and you need to adjust the distance so that it does not gap.
That is 100m of StopPosition distance.
The swapping time and adjust the distance, these are very important.

With this, each 2M2T appeared the moment 4M4T arrived, and 4M4T  disappeared at the same time, and could be replaced.


## <a name="change-rail-and-speed"></a> ■ How to change the rail while moving & how to change the speed while moving

With the newly added function in the future, it will be possible to "change the speed while moving" which was not possible from 1.6.0.0 to 1.7.1.3.

In the past, transparent rails had to be installed on all routes in advance, but this command allows you to automatically switch rail number that are at the distance for each vehicle according to instructions without put the transparent rails when the TFO is on moving.

This time, the arrangement used for N-shaped moving is used, but there is no transparent rail, only the actual rail.

The sample route this time is 'TFODemo_points.csv' and the XML is 'TFODemo_points.XML'.

![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-base-EN.jpg)

A TFO train departing from the departure station of Rail4 accelerate up to 45km / h.
However, to cross the point, slow down to 25km / h in front of the point.

![TFODemo_N-style_route-base](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail4-EN.jpg)

Pass the Rail1 point, which goes from Rail4 to Rail0 over a distance of 250m to 375m.

![TFODemo_passing_route-rail21](/images/TFOTutorial/EN/TFODemo_N-style_route-Rail1-EN.jpg)

After passing the point, accelerate again and head for the terminal station at 75 km / h.

Here are the subsections that will be added in the future.


### <a name="Points"></a>Points subsection

In the Points subsection, it is possible to change the running rail and change the speed.
Same as the Stops subsection, just renamed to avoid misunderstanding. Therefore, you can write a Point subsection in the Stops subsection, and you can also write a Stop subsection in the Points subsection.
You can mix Stop and Point in the Stops subsection, and also you can mix Stop and Point in the Points subsection as well.

{{% indent %}}

#### <a name="Point"></a>Point subsection

The following subsections can be write in the Point subsection.
※The Direction subsection cannot change the direction unlike Stop. Therefore, the Direction subsection cannot be used.

#### <a name="point-Decelerate"></a>Decelerate subsection

In the Points subsection, you can specify the deceleration rate with Decelerate as same as Stop subsection.
Takes over the speed of the previous Stop or Point subsection and decelerates at the specified deceleration.

#### <a name="point-Position"></a>Position subsection

It is the same as StopPosition, but it may be confused if described as StopPosition even though it is a Point, so it can be also described as Position.

#### <a name="point-Accelerate"></a>Accelerate subsection

At the Points subsection, it can be specified acceleration rate in the Accelerate, as also in Stop subsection.

#### <a name="point-PassingSpeed"></a>PassingSpeed subsection

This is dedicated subsection used in the Point subsection. Describe the speed per hour at distance described by Position (StopPosition). From here, acceleration / deceleration is performed toward the TargetSpeed.

#### <a name="point-TargetSpeed"></a>TargetSpeed subsection

The TargetSpeed subsection changes meaning depending on acceleration / deceleration.
In the case of deceleration, the speed is reduced to the speed of PassingSpeed from the speed specified by TargetSpeed to the distance of the PassingSpeed subsection.
In the case of acceleration, it accelerates from the speed specified by PassingSpeed to the speed of TargetSpeed with the acceleration rate of the Accelerate subsection.

#### <a name="point-Rail"></a>Rail subsection

The Rail subsection specifies the rail number you want to move.

{{% /indent %}}

## <a name="point-PassingSpeed-TargetSpeed"></a> ■ About relationship between PassingSpeed and TargetSpeed subsections

If you set PassingSpeed and TargetSpeed to the same value, it will suddenly switch to that speed at that distance.
In order to move smoothly, it is necessary to specify the acceleration / deceleration and then take a distance to reach that speed.
If the speed specified by PassingSpeed and Position cannot be reached by the next Point before reaching TargetSpeed from the distance specified by PassingSpeed and Position, the speed suddenly changes to the speed specified later.
In order to move smoothly, it is necessary to keep a distance commensurate with acceleration / deceleration.

## <a name="point-RailIndex"></a> ■ Points to note when switching RailIndex and Direction at Point subsection

Stop subsection's distance is based on the leading edge of the tip vehicle in Extensions.cfg.
At Point subsection, the axle of each TFO vehicle is affected for each axle unit when it exceeds the distance specified by Point subsection.

Therefore, it is necessary to consider the length of the train when switching Rails especially when moving backward with Stop subsection.
Since Point is each axle unit for affect, suddenly moving may occur if you do not consider the train length when switching Rails at points. So, for example, you need to pay attention to RailEnd etc.

Let's take a look at TFODemo_points.XML.

{{% code %}}
```XML
<?xml version="1.0" encoding="utf-8"?>	
<openBVE xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">	
  <TrackFollowingObject>	
    <Definition>	
        	<AppearanceTime>12.0000</AppearanceTime>
        	<AppearanceStartPosition>0</AppearanceStartPosition>
        	<AppearanceEndPosition>300</AppearanceEndPosition>
        	<LeaveTime>00.1000</LeaveTime>
    </Definition>	
    <Train>	
        	<Directory>Series-103</Directory>
    </Train>	
	
    <Stops>	
      <!--set to 3D space and start moving-->	
      <Stop>	
        	<Decelerate>0.00</Decelerate>
        	<StopPosition>100</StopPosition>
        	<Doors>R</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>1.71</Accelerate>
        	<TargetSpeed>45</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>4</Rail>
      </Stop>	
	
      <!--slow down to 25km/h in front of the point-->	
      <Point>	
        	<Decelerate>1.65</Decelerate>
        	<Position>225</Position>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>45</TargetSpeed>
        	<PassingSpeed>25</PassingSpeed>
        	<Rail>4</Rail>
      </Point>	
	
      <!--change rail with moving at the point-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>250</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>1</Rail>
      </Point>	
	
      <!--change rail with moving at the point-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>375</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
	
      <!--accelerate to 75km/h after over the point-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>475</Position>
        	<Accelerate>1.71</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>75</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
	
     <!--Stops at the terminal station-->	
      <Stop>	
        	<Decelerate>1.65</Decelerate>
        	<StopPosition>1600</StopPosition>
        	<Doors>L</Doors>
        	<StopTime>00.0030</StopTime>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>0</TargetSpeed>
        	<Direction>1</Direction>
        	<Rail>0</Rail>
      </Stop>	
    </Stops>	
  </TrackFollowingObject>	
</openBVE>	
```
{{% /code %}}

There is nothing special about the Definition section.

The first Stop subsection also moves Rail4 in the direction of travel, but nothing has changed.
It accelerates to 45km/h.
Let's look at the first Point subsection that comes up next.

{{% code %}}
```XML
      <!--slow down to 25km/h in front of the point-->	
      <Point>	
        	<Decelerate>1.65</Decelerate>
        	<Position>225</Position>
        	<Accelerate>0</Accelerate>
        	<TargetSpeed>45</TargetSpeed>
        	<PassingSpeed>25</PassingSpeed>
        	<Rail>4</Rail>
      </Point>	
```
{{% /code %}}

Decelerate at 1.65 and slow down.

The Rail1's point rail is...

{{% code %}}
````
,;branch line right to left					
250					
	.Rail	1	;	4.3	;-0.02
	.Railtype 1;	1			
````
{{% /code %}}

Is specified as above. Therefore, the Position must slow down before this.
This time, set it to decelerate at until 225m.
Since it has already accelerated to 45km / h by the previous Stop command, and this case is in the case of deceleration of TargetSpeed, it is set to 45km/h.
From there, it will decelerate to PassingSpeed 25km/h by the distance of the current Point.

After decelerating to the point, the TFO train will enter the point, but you must switch at an appropriate distance due to the actual rail.
As before shown route data, Rail1 used for points starts at a distance of 250.

So the Point subsection that switches Rails looks like this:

{{% code %}}
```xml
      <!--change rail with moving at the point-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>250</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>1</Rail>
      </Point>	
```
{{% /code %}}

Deceleration and Accelerate are both 0 because deceleration has already been completed and acceleration / deceleration is not performed.
The important thing is Position, which needs to be adjusted to the distance of the point on the actual rail.
This time, Rail1 started with a distance of about 250m, so set it 250.
PassingSpeed and TargetSpeed specify the after deceleration value of 25km/h, to keep the speed from changing.
And Rail is Rail1 which is used as a point rail.

This allows you to change Rail while moving.

Then you need to switch to the destination rail again. Here the TFO train is move to Rail0.

The last distance of Rail1 is as follows.

{{% code %}}
```
375				
	.Rail	1	;	0
	.Railtype 1;	0		
	.RailEnd	1	;	
```
{{% /code %}}

After crossing the point, RailEnd 1 is set at a distance of 375m.

As mentioned earlier, the replacement of the rails is very important the distance, so we will replace them at 375m.

{{% code %}}
```xml
      <!--change rail with moving at the point-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>375</Position>
        	<Accelerate>0</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>25</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
```
{{% /code %}}

The distance 375m is specified as the position of RailEnd, and acceleration and deceleration are not performed, so both Decelerate and Accelerate are 0.
Since the speed is the same, PassingSpeed and TargetSpeed remain at 25km / h.

And set Rail to 0.

This will cause the TFO train to cross the point.

I think we will accelerate again when we finish crossing points or slow down, so we will accelerate the TFO train again when all vehicles have crossed the point.

{{% code %}}
```xml
      <!--accelerate to 75km/h after over the point-->	
      <Point>	
        	<Decelerate>0</Decelerate>
        	<Position>475</Position>
        	<Accelerate>1.71</Accelerate>
        	<PassingSpeed>25</PassingSpeed>
        	<TargetSpeed>75</TargetSpeed>
        	<Rail>0</Rail>
      </Point>	
```
{{% /code %}}

Since the public domain 103 series has 20m x 4 cars, this time, the Position was set to 475m.

To accelerate, Accelerate was set to 1.71.
At here, the important function is PassingSpeed and TargetSpeed.
These two functions is the difference meaning during acceleration and deceleration.
This time it is acceleration, so we will accelerate from PassingSpeed toward the TargetSpeed from the distance of PassingSpeed.

In other words, it accelerates from 25km/h to 75km/h at an acceleration of 1.71 from a distance of 475m.

The moving rail is continuing on Rail0.

Finally, arrive at the terminal station.



This tutorial is end.
We hope this tutorial will help you create your data.

This document is written at 2020 by midnight express ginga81 (ginga81)
This document is public domain.
