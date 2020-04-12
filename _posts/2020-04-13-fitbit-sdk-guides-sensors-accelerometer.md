---
title: "[Fitbit] Accelerometer Sensor, Fitbit SDK Guides"
excerpt: "dev.fitbit.com/build/guides/sensors/accelerometer/"

toc: true
toc_sticky: true
toc_label: "Accelerometer"

header:
  teaser: /assets/images/fitbit/fitbit-logo.jpeg

categories:
  - fitbit
tags:
  - fitbit

last_modified_at: 2020-04-13T01:00:00+09:00
---  

**Fidbit SDK Guides**  
These resources will help you understand how to use each element of the Fitbit SDK.  

**Sensors Guides**  
Each Fitbit device includes a variety of hardware sensors that have been exposed through Sonsor APIs.  

# Accelerometer Sensor Guide
Accelerometers can be used to measure device acceleration, and determine orientation.  
The sensor measures acceleration along 3 orthogonal axes: <big>X</big>, <big>Y</big> and <big>Z</big>.  

## A Brief Introduction to Axes

The <big>X</big> axis is parallel with the device's screen, aligned with the top and bottom edges, in the left-right direction. The <big>Y</big> axis is parallel with the device's screen, aligned with the left and right edges, in the top-bottom direction. The <big>Z</big> axis is perpendicular to the device's screen, pointing up.  

![](https://eliotjang.github.io/assets/images/fitbit/accelerometer-sensor-guide-1.png){: width="100%" height="100%"}  

Acceleration includes the acceleration of gravity. So it the device is at rest, lying flat on a table, the acceleration along the <big>Z</big> axis should equal the acceleration of gravity (<sup>~</sup>9.8 m/s<sup>2</sup>), and the acceleration along the <big>X</big> and <big>Y</big> axes should be <big>0</big>.  

The accelerometer axes readings are provided as floating point values, in units of m/s<sup>2</sup>.  

## Peeking the Current Accelerometer Reading  

If you need to check the latest reading from the accelerometer, you can access the properties directly from the accelerometer object using the `onreading` event.  

> By default, the accelerometer sample rate is **100Hz** (100 times per second).  

**app**  

```javascript  
if (Accelerometer) {
  // sampling at 1Hz (once per second)
  const accel = new Accelerometer({ frequency: 1 });
  accel.addEventListener("reading", () => {
    console.log(
      `ts: ${accel.timestamp}, \
       x: ${accel.x}, \
       y: ${accel.y}, \
       z: ${accel.z}`
    );
  });
  accel.start();
}
```  

## Batched Reading  

The Accelerometer API can also generate batches of data at a specified sample rate. This allows a developer to control the frequency that their application receives and processes sensor data.  

The `onreading` event is emitted when a batch of readings is available, and the `.readings` property contains the sensor readings, with each data channel (`x`, `y`, `z`, and `timestamp`) as its own array.  

By reducing the sample rate (`frequency`), or by increasing the batch size, developers will benefit from reduced CPU usage, and minimize their application's impact on battery life.  

In order to use batched readings, the `batch` property must be specified during the initailization of the sensor.  

**app**  

```javascript 
import { Accelerometer } from "accelerometer";

if (Accelerometer) {
  // 30 readings per second, 60 readings per batch
  // the callback will be called once every two seconds
  const accel = new Accelerometer({ frequency: 30, batch: 60 });
  accel.addEventListener("reading", () => {
    for (let index = 0; index < accel.readings.timestamp.length; index++) {
      console.log(
        `Accelerometer Reading: \
          timestamp=${accel.readings.timestamp[index]}, \
          [${accel.readings.x[index]}, \
          ${accel.readings.y[index]}, \
          ${accel.readings.z[index]}]`
      );
    }
  }
  accel.start();
}
```  

## Automatically Stopping and Starting  

One of the best ways to conserve battery life is to stop the sensor when the display is off. You can use the Display API to respond changes in the screen's power state.  

**app**  

```javascript  
import { Accelerometer } from "accelerometer";
import { display } from "display";

if (Accelerometer) {
  const accel = new Accelerometer({ frequency: 1 });
  accel.addEventListener("reading", () => {
    console.log(
      `ts: ${accel.timestamp}, \
       x: ${accel.x}, \
       y: ${accel.y}, \
       z: ${accel.z}`
    );
  });
  display.addEventListener("change", () => {
    // Automatically stop the sensor when the screen is off to conserve battery
    display.on ? accel.start() : accel.stop();
  });
  accel.start();
}
```  

## Accelerometer Best Practices  
Here's a simple list of best practices to follow when using the Accelerometer API:  

1. Always use the most optimal frequency for your specific needs.
2. Don't forget to call `accel.stop();` when you've finished using it.
3. Check if the sensor exists before using it.  

## The Accelerometer in Action  
If you're interesting in using the Accelerometer API within your application, review the Accelerometer API reference documentation.  




























