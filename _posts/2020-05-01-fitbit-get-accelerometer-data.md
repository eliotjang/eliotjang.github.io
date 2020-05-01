---
title: "[Fitbit] #5 Accelerometer Sensor Application Demo"
excerpt: "https://studio.fitbit.com/"

toc: true
toc_sticky: true
toc_label: "Accelerometer Application"

header:
  teaser: /assets/images/fitbit/fitbit-logo.jpeg

categories:
  - fitbit
tags:
  - fitbit

last_modified_at: 2020-05-01T23:00:00+09:00
---  

<span style="color:blue"><b>I made Project that demos reading data from hardware sensors</b></span>.

## Application Architecture

### /app/

- index.js

```javascript
import { Accelerometer } from "accelerometer";
import { Barometer } from "barometer";
import { BodyPresenceSensor } from "body-presence";
import { display } from "display";
import document from "document";
import { Gyroscope } from "gyroscope";
import { HeartRateSensor } from "heart-rate";
import { OrientationSensor } from "orientation";

const accelLabel = document.getElementById("accel-label");
const accelData = document.getElementById("accel-data");

const barLabel = document.getElementById("bar-label");
const barData = document.getElementById("bar-data");

const bpsLabel = document.getElementById("bps-label");
const bpsData = document.getElementById("bps-data");

const gyroLabel = document.getElementById("gyro-label");
const gyroData = document.getElementById("gyro-data");

const hrmLabel = document.getElementById("hrm-label");
const hrmData = document.getElementById("hrm-data");

const orientationLabel = document.getElementById("orientation-label");
const orientationData = document.getElementById("orientation-data");

const sensors = [];

if (Accelerometer) {
  const accel = new Accelerometer({ frequency: 1 });
  accel.addEventListener("reading", () => {
    accelData.text = JSON.stringify({
      x: accel.x ? accel.x.toFixed(1) : 0,
      y: accel.y ? accel.y.toFixed(1) : 0,
      z: accel.z ? accel.z.toFixed(1) : 0
    });
  });
  sensors.push(accel);
  accel.start();
} else {
  accelLabel.style.display = "none";
  accelData.style.display = "none";
}

if (Barometer) {
  const barometer = new Barometer({ frequency: 1 });
  barometer.addEventListener("reading", () => {
    barData.text = JSON.stringify({
      pressure: barometer.pressure ? parseInt(barometer.pressure) : 0
    });
  });
  sensors.push(barometer);
  barometer.start();
} else {
  barLabel.style.display = "none";
  barData.style.display = "none";
}

if (BodyPresenceSensor) {
  const bps = new BodyPresenceSensor();
  bps.addEventListener("reading", () => {
    bpsData.text = JSON.stringify({
      presence: bps.present
    })
  });
  sensors.push(bps);
  bps.start();
} else {
  bpsLabel.style.display = "none";
  bpsData.style.display = "none";
}

if (Gyroscope) {
  const gyro = new Gyroscope({ frequency: 1 });
  gyro.addEventListener("reading", () => {
    gyroData.text = JSON.stringify({
      x: gyro.x ? gyro.x.toFixed(1) : 0,
      y: gyro.y ? gyro.y.toFixed(1) : 0,
      z: gyro.z ? gyro.z.toFixed(1) : 0,
    });
  });
  sensors.push(gyro);
  gyro.start();
} else {
  gyroLabel.style.display = "none";
  gyroData.style.display = "none";
}

if (HeartRateSensor) {
  const hrm = new HeartRateSensor({ frequency: 1 });
  hrm.addEventListener("reading", () => {
    hrmData.text = JSON.stringify({
      heartRate: hrm.heartRate ? hrm.heartRate : 0
    });
  });
  sensors.push(hrm);
  hrm.start();
} else {
  hrmLabel.style.display = "none";
  hrmData.style.display = "none";
}

if (OrientationSensor) {
  const orientation = new OrientationSensor({ frequency: 60 });
  orientation.addEventListener("reading", () => {
    orientationData.text = JSON.stringify({
      quaternion: orientation.quaternion ? orientation.quaternion.map(n => n.toFixed(1)) : null
    });
  });
  sensors.push(orientation);
  orientation.start();
} else {
  orientationLabel.style.display = "none";
  orientationData.style.display = "none";
}

display.addEventListener("change", () => {
  // Automatically stop all sensors when the screen is off to conserve battery
  display.on ? sensors.map(sensor => sensor.start()) : sensors.map(sensor => sensor.stop());
});
```

### /resources/

- icon.png

![](https://eliotjang.github.io/assets/images/fitbit/icon.png){: width="30%"}

- index.gui

```javascript
<svg>
  <rect width="100%" height="20" x="0" y="0" />
  <text id="accel-label" class="sensor-label">accelerometer</text>
  <text id="accel-data" class="sensor-data">{ ... }</text>
  <text id="bar-label" class="sensor-label">barometer</text>
  <text id="bar-data" class="sensor-data">{ ... }</text>
  <text id="bps-label" class="sensor-label">body-presence</text>
  <text id="bps-data" class="sensor-data">{ ... }</text>
  <text id="gyro-label" class="sensor-label">gyroscope</text>
  <text id="gyro-data" class="sensor-data">{ ... }</text>
  <text id="hrm-label" class="sensor-label">heart rate</text>
  <text id="hrm-data" class="sensor-data">{ ... }</text>
  <text id="orientation-label" class="sensor-label">orientation</text>
  <text id="orientation-data" class="sensor-data">{ ... }</text>
</svg>

```
- styles.css

```javascript
.sensor-label {
  font-family: System-Regular;
  fill: white;
  text-anchor: middle;
  text-length: 32;
  font-size: 20;
  x: 50%;
  y: $+5;
}

.sensor-data {
  font-family: System-Light;
  fill: yellow;
  text-anchor: middle;
  text-length: 64;
  font-size: 20;
  x: 50%;
  y: $;
}
```

- widgets.gui

```javascript
<svg>
  <defs>
    <link rel="stylesheet" href="styles.css" />
    <link rel="import" href="/mnt/sysassets/widgets_common.gui" />
  </defs>
</svg>

```

## What I did

I published to Fitbit application and executed on my Fitbit Versa Lite.  

![](https://eliotjang.github.io/assets/images/fitbit/application-name.png){: width="80%"}

![](https://eliotjang.github.io/assets/images/fitbit/get-accelerometer-data.jpeg){: width="70%"}  

## What I have to do next?

<span style="color:green">I made the projects with sensor examples.  
I must to restore acceleromter data to Database.  
First that come in to my head is using Firebase.</span>























