# Week 2 - Intro Android

## Outline

- Mobile Applications
- Android History
- Androidx
- Android Studio
- Layouts
- Android Files
- AVD (Android Virtual Devices)

## Mobile Applications

1. Native apps
2. Web apps
3. Progressive Web Apps (PWAs)

### Native App

- Written in a native language like Android or Swift, so compiled into the machine code
- Distributed through native app stores (Apple App Store or Google Play) and requires installation on the device
- For updates, the user need to download the new version
- Specific to a certain mobile platform and not cross-platform
- Requires learning the native language
- High performance
- Provides full access to all the features and hardware of the device
- Offline operation capability
- Built-in security layers

### Web Apps

- Web Apps are usually built with HTML5, CSS and JavaScript
- No need to download and install the app
- Run in the web browser of the device like a web page
- Cross-platform
- Easier to modify and update web apps (automatic updates)
- The lack of access to hardware features (e.g. sensors or camera)
- Low performance (in terms of speed and responsiveness)
- **Hybrid apps**: combining the native apps and web apps
  - Flutter, React Native and Ionic

### Progressive Web App (PWA)

- Built using web technologies like HTML5 and JavaScript
- Run in a standalone window instead of a browser tab
- Supported by modern browsers
- Fast and simple installation (publishing on mobile app stores optional)
- Automatic updates
- Native-like capabilities and access to the device hardware level information, and features like file system access and push notifications
- Offline operation capability
- Good performance
- Progressive Web Apps must meet the criteria of being installable https://web.dev/pwa-checklist/

## Android history

Probably not on the exam

## Android Jetpack

- Jetpack includes a large group of libraries for developing high quality apps
- androidx.* contains the Android Jetpack libraries
  - The previous support library packages have been mapped into corresponding androidx.* packages
  - AndroidX provides backward compatibility across Android releases
- The libraries are created based on best practices (less crashes and memory leaks)
- They provide consistency across Android versions and devices
- They reduce boilerplate code and complexities

https://developer.android.com/jetpack

![Android language](./Images/AndroidLanguage.png)

