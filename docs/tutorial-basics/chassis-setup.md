---
sidebar_position: 2
---

# Chassis Setup

How to configure the mikLib chassis

## Drivetrain 

The first step is to configure your drivetrain motors, each motor takes in 4 values, the port, whether or not the motor is reversed, 
its cartridge, and name.

```cpp
// Motor on port 1 with a blue cartridge that is not reversed
mik::motor(PORT1, false, blue_6_1, "my motor")
```


:::important

Right and left are from facing behind the front of the robot,
or the robots point of view.

:::

To determine the port of your motor you can open the device menu on the brain
and unplug the motor and see which port disappears. Do this for 
the left and right side of the drivetrain.

```cpp
// Left drivetrain motors (left/right is looking from behind the robot)
mik::motor_group({
    mik::motor(PORT1, false, blue_6_1, "left_front_motor"),
    mik::motor(PORT2, false, blue_6_1, "left_middle_motor"),
    mik::motor(PORT3, false, blue_6_1, "left_back_motor")
}),
// Right drivetrain motors
mik::motor_group({
    mik::motor(PORT4, false, blue_6_1, "right_front_motor"),
    mik::motor(PORT5, false, blue_6_1, "right_middle_motor"),
    mik::motor(PORT6, false, blue_6_1, "right_back_motor")
}),
```

When applying a positive voltage, all the drivetrain motors need to spin 
forward, to find which motors are reversed you can run the program, press
the **Config** tab at the top left, then press
**Spin Motors** at the bottom left. You should see each motor spin individually. Set reversed to 
**true** if motor does not spin forward.

## Odometry

### Inertial Sensor

mikLib requires an inertial sensor. Make sure your inertial sensor is lying down flat. To reduce vibrations
that cause the sensor to drift it is recommended to mount it on foam or rubber
links. 

// add image of that here

Place the port of your inertial sensor in the chassis.

```cpp
PORT10, // Inertial sensor port
```


### Inertial Scale

:::note
This is for extra precision, in most cases 360 will be fine to use
for inertial scale
:::

:::important
If the robot is not spun clockwise to find inertial scale, heading will be incorrect
:::

Sometimes what can happen is your inertial sensor will 
not read a full 360° after a full rotation. To fix this
you can apply a scale factor to your heading. To find this value run the program,
press **Config** then **Odom Data**. You should see **Rotation: 0.00000** with 
the small decimal places flickering. Turn your robot clockwise in a full 360 until it ends up
in the exact same spot it started. Use the value from **Rotation: 360.15**.

```cpp
360.15, // Inertial scale (reading after a full 360° turn)
```

### Tracking Wheels

Tracking wheels are non motorized wheels with encoders attached to them.

// attach image here

:::important
The forward tracking wheel is parallel to the wheels on the drivetrain, with 
sideways tracking wheel being perpendicular
:::

### Forward Tracker

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

The forward tracker measures the lateral distance of the robot. You can use
either a dedicated forward tracking wheel or your drivetrain motor encoders.

<Tabs>
  <TabItem value="motor-encoder" label="Motor Encoders" default>

If you don't have a dedicated forward tracking wheel, you can use the built-in
motor encoders from your drivetrain.

```cpp
motor_encoder, // Use "motor_encoder" if no forward tracker
```

  </TabItem>
  <TabItem value="tracking-wheel" label="Forward Tracking Wheel">

If you have a dedicated forward tracking wheel, plug in the port and wheel
diameter.

```cpp
// Forward tracker on port 'A' with a 2.75" wheel
mik::tracker(PORT_A, 2.75, "forward_tracker"),
```

  </TabItem>
</Tabs>