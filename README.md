# Buzzz framework for iOS

## Table of Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Installation](#installation)
- [How to set up your project](#how-to-set-up-your-project)
  - [Add to Info plist file](#add-to-info-plist-file)
  - [Capabilities tab of the project](#capabilities-tab-of-the-project)
  - [How to create push notification certificate of your project](#how-to-create-push-notification-certificate-of-your-project)
- [Methods of the framework](#methods-of-the-framework)
- [How to set up your code](#how-to-set-up-your-code)
  - [Usage example](#usage-example)
- [Using of AppDelegate class](#using-of-appdelegate-class)
  - [Usage example](#usage-example)

## Introduction
A Swift based iOS framework that communicates to our server via RESTFul API.
### Version: 0.3.0
## Requirements:

- iOS 11.0+
- Swift 4.0.1

## Installation:

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

Or you can download latest [release](https://github.com/Carthage/Carthage/releases) file `Carthage.pkg` and install it.

To integrate Buzzz with necessary linked frameworks into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "BuzzzProx/buzzz"
github "Alamofire/Alamofire"
github "aws/aws-sdk-ios"
```

Run `carthage update` to build the framework and drag the built `buzzz.framework` and others into your Xcode project at `Embedded binaries` section.

These linked frameworks should be added into your project:

1. Alamofire.framework
2. AWSCore.framework
3. AWSIoT.framework
4. AWSS3.framework
5. buzzz.framework


## How to set up your project

Drop buzzz.framework file to a root directory of your project.
Add the framework in "Embedded Binaries" on "General" tab of project's target. 
Input "import buzzz" in the project to start use framework.

### Add to Info plist file:
1. App Transport Security Settings -> Allow Arbitrary Loads => YES

2. Privacy - Location Always Usage Description (NSLocationAlwaysUsageDescription) => "A string value explaining to the user how the app uses this data"
3. Privacy - Location When In Use Usage Description (NSLocationWhenInUseUsageDescription) => "A string value explaining to the user how the app uses this data When In Use"
4. Privacy - Location Always and When In Use Usage Description (NSLocationAlwaysAndWhenInUseUsageDescription) => "A string value explaining to the user how the app uses this data Always and When In Use"

### Capabilities tab of the project:

1. Background Modes: Location updates, Uses Bluetooth LE accessories, Remote notifications, Background fetch.

2. Push notifications*.

* You should create a certificate of push notifications for your App Id and provide file with ".pem" extension to our server.

### How to create push notification certificate of your project:

1. Follow to "https://developer.apple.com/account/ios/identifier/bundle".
2. Click "Edit" button on your App Id.
3. In "Push Notification" section choose right section "Development" or "Production SSL Certificate" and click button "Create Certificate...".
4. Follow instructions on the page and create ".certSigningRequest" file.
5. Download file with ".cer" extension.
6. Open ".cer" file with Keychain Access program and add it in "login" section.
7. Select "login" keychain and "All items" category on the left side.
8. Find your certificate named like "Apple Development IOS Push Services/ ...".
9. Right mouse click on it and click "Export ...".
10. Select ".p12" file format and click "Save".
11. Open "Terminal" application. Find file with ".p12" format location in console. Edit "openssl pkcs12 -in YourCertificateName.p12 -out YourCertificateNameForServer.pem -nodes -clcerts" line and press "Enter".
11. New file "YourCertificateNameForServer.pem" was created near your certificate "YourCertificateName.p12". Provide ".pem" file to our server.

## Methods of the framework:
- BuzzzInit (clientid, secret)  - Get token and Setup api connections to server, initiate beacon regions.
Parameters:
> "clientId" - Client id (String type)

> "secret" -     Secret code (String type)

- BuzzzStart() - Start beacon scanning and update resuts to servers.
- BuzzzStop() - Stop beacon scanning and update resuts to servers.

## How to set up your code

1. Add "import buzzz" line at top of file where framework methods will be used.
1. Replace AppDelegate class inheritance from "UIResponder, UIApplicationDelegate" to "BuzzzAppDelegate".
2. Call BuzzzInit (clientid, secret) inside of 'application: didFinishLaunchingWithOptions' delegate method of AppDelegate class.
3. Use BuzzzStart() or BuzzzStop() anywhere in your project.

### Usage example
```
import UIKit
import buzzz

@UIApplicationMain
class AppDelegate: BuzzzAppDelegate
{
var window: UIWindow?

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]?) -> Bool
{
BuzzzInit(clientId: "xxxx", secret: "yyyy")

return true
}
}
```

## Using of AppDelegate class 
If you want to write delegate methods to AppDelegate class and you see a need to add "override" keyword before this method, then you need call superclass function of this method on the first line.
Otherwise, some framework features will not work correctly.

### Usage example
```
override func yourAppDelegateMethod () 
{
super.yourAppDelegateMethod()
// .. your implementation
}
```
