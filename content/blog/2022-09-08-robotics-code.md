---
title: Robotics Code
date: 2022-09-08T21:42:29.983Z
description: Figure dis out
---
<!--StartFragment-->

/\*----------------------------------------------------------------------------\*/

/\* \*/

/\* Module: main.cpp \*/

/\* Author: VEX \*/

/\* Created: Wed Sep 25 2019 \*/

/\* Description: Tank Drive \*/

/\* This sample allows you to control the V5 Clawbot using the both \*/

/\* joystick. Adjust the deadband value for more accurate movements. \*/

/\*----------------------------------------------------------------------------\*/



// ---- START VEXCODE CONFIGURED DEVICES ----

// Robot Configuration:

// \[Name] \[Type] \[Port(s)]

// Controller1 controller

// LeftMotor motor 1

// RightMotor motor 10

// ClawMotor motor 3

// ArmMotor motor 8

// ---- END VEXCODE CONFIGURED DEVICES ----



\#include "vex.h"



using namespace vex;



void whenControllerL1Pressed() {

IntakeTop.spin(forward);

IntakeMiddle.spin(forward);

waitUntil(!Controller.ButtonR1.pressing());

IntakeTop.stop();

IntakeMiddle.stop();

}



void whenControllerL2Pressed() {

IntakeTop.spin(reverse);

IntakeMiddle.spin(reverse);

waitUntil(!Controller.ButtonR2.pressing());

IntakeTop.stop();

IntakeMiddle.stop();

}



int dist = 0;



int main() {

// Initializing Robot Configuration. DO NOT REMOVE!

vexcodeInit();



Inertial5.calibrate();



IntakeTop.setStopping(hold);

IntakeMiddle.setStopping(hold);

IntakeTop.setVelocity(100, percent);

IntakeMiddle.setVelocity(100, percent);



Controller.ButtonR1.pressed(whenControllerL1Pressed);

Controller.ButtonR2.pressed(whenControllerL2Pressed);



// Deadband stops the motors when Axis values are close to zero.

int deadband = 5;



while (true) {

// Get the velocity percentage of the left motor. (Axis3)

int leftMotorSpeed = Controller.Axis3.position() + Controller.Axis1.position();

// Get the velocity percentage of the right motor. (Axis3 - Axis1)

int rightMotorSpeed = Controller.Axis3.position() - Controller.Axis1.position();



// Set the speed of the left motor. If the value is less than the deadband,

// set it to zero.

if (abs(leftMotorSpeed) < deadband) {

// Set the speed to zero.

FrontLeft.setVelocity(0, percent);

BackLeft.setVelocity(0, percent);

} else {

// Set the speed to leftMotorSpeed

FrontLeft.setVelocity(leftMotorSpeed, percent);

BackLeft.setVelocity(leftMotorSpeed, percent);

dist += FrontLeft.rotation(rotationUnits::rev);

FrontLeft.resetRotation();

}



// Set the speed of the right motor. If the value is less than the deadband,

// set it to zero.

if (abs(rightMotorSpeed) < deadband) {

// Set the speed to zero

FrontRight.setVelocity(0, percent);

BackRight.setVelocity(0, percent);

} else {

// Set the speed to rightMotorSpeed

FrontRight.setVelocity(rightMotorSpeed, percent);

BackRight.setVelocity(rightMotorSpeed, percent);

dist += FrontRight.rotati/on(rotationUnits::rev);

FrontRight.resetRotation();

}



// Spin both motors in the forward direction.

FrontLeft.spin(forward);

FrontRight.spin(forward);

BackLeft.spin(forward);

BackRight.spin(forward);



wait(25, msec);



}

}



<!--EndFragment-->