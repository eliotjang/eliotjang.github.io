---
title: "[Fitbit] #4 Accelerometer API, Device API Reference"
excerpt: "dev.fitbit.com/build/reference/device-api/accelerometer/"

toc: true
toc_sticky: true
toc_label: "Accelerometer API"

header:
  teaser: /assets/images/fitbit/fitbit-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - Fitbit

last_modified_at: 2020-04-13T04:00:00+09:00
---  

**Reference**  
Fitbit offers three types of APIs for building Fitbit device apps and clock faces.  

**Device API Reference**  
The Device APIs are accessbile by applications which run on Fitbit devies only.  

# Accelerometer API

## Class: Accelerometer  
An Accelerometer sensor measures a device's acceleration along 3 orthogonal axes (`x`, `y` and `z`).  

- The `x` axis is parallel with the device's screen, aligned with the top and bottom edges, in the left-to-right direction.
- The `y` axis is parallel with the device's screen, aligned with the left and right edges, in the bottom-to-top direction.
- The `z` axis is perpendicular to the device's screen, pointing up.  

Accelertaion readings include the acceleration of gravity. For example: If the device is at rest, lying flat on a table on the surface of the earth, the acceleration along the `z` axis should equal the acceleration of grivity (<sup>~</sup>9.8 m/s<sup>2</sup>), and the accleration along the `x` and `y` axees should be `0`.  

Read the [Accelerometer Sensor Guide](https://eliotjang.github.io/fitbit/fitbit-sdk-guides-sensors-accelerometer/) for further information. The Accelerometer API provides access to the acceleration data measured by the hardware sensor.  

**app**  

```javascript
import { Acceleromter } from "acceleromter";

if (Accelerometer) {
  console.log("This device has an Accelerometer!");
  const accelorometer = new Accelerometer({ frequency: 1 });
  accelerometer.addEventListener("reading", () => {
    console.log(`${accelerometer.x},${accelerometer.y},${accelerometer.z}`);
  });
} else {
  console.log("This device does NOT have an Accelerometer!");
}
```  

### Properties

#### readonly activated  
`boolean`  

Flag that indicates if the sensor is activate or not. When a sensor is created, the sensor is not activated, thus the initial value of this property equals `false`.  

#### onactivate  
`((this: Sensor, event: Event) => any) or undefined`  

Event handler that is called when the sensor is activated.  

#### onerror  
`((this: Sensor, event: SensorErrorEvent) => any) or undefined`  

Event handler that is called when an error occurs. When an error occurs, the sensor is automatically stopped, and the activated property equals `false`.  

#### onreading  
`((this: Sensor, event: Event) => any) or undefined`  

Event handler that is called whenever a new reading is available.  

#### readonly readings  
`BatchedAccelerometerReading or undefined`  

*New in SDK 2.0*  

## Interface: BatchedAccelerometerReading  
*New in SDK 2.0*  

### Properties

#### readonly timestamp  
`Float32Array`  

#### readonly x  
`Float32Array`  

#### readonly y  
`Float32Array`  

#### readonly z  
`Float32Array`  

## Interface: AccelerometerReading  
Acceleration data measured by the accelerometer sensor.  

### Properties  

#### readonly timestamp  
`number or null`  

Timestamp of the reading in milliseconds.  

> NOTE: this is relative to an unspecified arbitrary `0` time, or `null` if no reading is avilable (when the sensor is not yet activated and there are no valid cached values that can be used).  

#### readonly x  
`number or null`  

#### readonly y  
`number or null`  

#### readonly z  
`number or null`  

Acceleration along the `z` axis in m/s<sup>2</sup>  





