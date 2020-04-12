---
title: "[Fitbit] #2 Glossary of Terms, Fitbit SDK Guides"
excerpt: "dev.fitbit.com/build/guides/glossary/"

toc: true
toc_sticky: true
toc_label: "Glossary"

header:
  teaser: /assets/images/fitbit/fitbit-logo.jpeg

categories:
  - fitbit
tags:
  - fitbit

last_modified_at: 2020-04-12T21:20:00+09:00
---  
# Fitbit SDK Guides  
These resources will help you understand how to use each element of the Fitbit SDK.  

## Glossary  
If you want to understand some of the terminology used in the guides.

### App / Application
An application which executes on a Fitbit device. The application is comprised of JavaScript (.js) or TypeScript (.ts) files for the <span style="color:blue">application logic</span>, SVG (.gui) files for the <span style="color:blue">structure</span>, and CSS (.css) files for the <span style="color:blue">presentation</span> layer.  

### Clock face
Clock faces are a special type of app that are primarily used to display the current date and time. Each device has a single clock face installed at any time, and interaction is limited to touch events only.  

### Device API
A set of JavaScript APIs which allow developers access to data from hardware sensors, send and receive communications, or interact with the presentaion layer of the device. These APIs are only accessible within application code.  

### SVG(Scalable Vector Graphics)
SVG defines the structure of your application's presentation layer.  

### Component
Prebuilt, easy to use SVG components which can be included in your application. Some examples include: Clock, Menu Bar, Button etc.  

### CSS (Cascading Style Sheets)
CSS is used to apply styling to your application's presentation layer. It allows you to change colors, fonts, positioning and sizes of elements. This is based up standard web CSS, but it does have a few subtle implementation differences.  

### Companion
A secondary JavaScript application, which executes within the Fitbit mobile application. The companion can act as a bridge between the Fitbit device and the Internet. It allows you to send and receive data or files, or perform data processing without the constraints of the Fitbit device hardware.  

### Companion API
A set of JavaScript APIs which allow developers to send and receive communications, or utilize the processing capabilities of the mobile device. These APIs are only accessible within companion code.  

### Settings
Settings allow users to change the behavior or appearance of an application by changing values from within the Fitbit mobile application. Settings are comprised of components such as text boxes, toggles, select lists and other form controls.  

### Settings API
The <span style="color:blue">Settings API</span> is used to generate an application's settings page. Each setting can be easily defined using <span style="color:blue">React JSX</span>, and the Fitbit mobile application will render this as a form which matches the existing appearance of the mobile application.  

### Fitbit Studio
<span style="color:blue">Fitbit Studio</span> is the web browser based environment used for Fitbit application development. Developers can create, edit and compile multiple applications using this online tool.  

### Developer Bridge
The Developer Bridge provides a websocket connection between Fitbit Studio, your Fitbit device, and the Fitbit mobile application. This connection is used to install apps and companions, and provides console logs to Fitbit Studio.  


