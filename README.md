## Magnet Jumpstart App for iOS

The Magnet Jumpstart App for iOS is licensed under the terms of the [Magnet Software License Agreement](http://www.magnet.com/resources/tos.html).  Please see [LICENSE](LICENSE) file for full details.

In this tutorial, you will learn how to build a "Jumpstart" iOS app that interacts with a "Jumpstart" Mobile Backend running locally.

###1. Prerequisites
1. Mobile App Builder tool
2. Xcode 5
3. CocoaPods

###2. Build the Mobile Backend

To build the Jumpstart server, you can run the following command on the Mobile App Builder tool:

    jumpstart@local:mab> run jumpstart.mab

###3. Create Xcode project
Create a new Single View Application called "Jumpstart" using Xcode:
![Create Project](https://dl.dropboxusercontent.com/u/25131624/Xcode-Create-Project-Wizard.png)

###4. Import Mobile SDK and Mobile APIs using CocoPods

#### Generate the Mobile APIs
You can generate the mobile API by running the following command on the Mobile App Builder tool:

    jumpstart@local:mab> api-generate ios
    
This command would generate the mobile API in the following directory: `~/MABProjects/jumpstart/mobile/apis/assets/ios`
    
#### Copy the Mobile APIs
You can copy the generated mobile API to your Xcode project directory by running the following command on the Mobile App Builder tool:
    
    jumpstart@local:mab> exec cp ~/MABProjects/jumpstart/mobile/apis/assets/ios/Source /path/to/MyProject

#### Create Podfile to contain Mobile APIs and Mobile SDK
Create a Podfile in your Xcode project directory.    

    platform :ios, '7.0'

    pod 'MagnetMobileServer', :git => 'git@bitbucket.org:magneteng/magnet-sdk-ios-2.3.0.git'
    pod 'magnet-mobile-assets', :path => 'Source'

This would install the Mobile SDK for iOS and your generated Mobile API in your project in the next step. At this stage, your Xcode project directory structure should look like this:
![Create Project](https://dl.dropboxusercontent.com/u/25131624/Xcode-Project-Directory-Structure.png)

A sample Podfile is also available in the generated Mobile API here: `~/MABProjects/jumpstart/mobile/apis/assets/ios/Podfile`

#### Install dependencies in your project
If you haven't installed CocoaPods already, you can follow the installation instructions on their website: http://cocoapods.org
In your Xcode project directory, install the dependencies by running the following command on the terminal:

    $ pod install
    
**Note**: Please note that if your installation fails, it may be because you are installing with a version of Git lower than CocoaPods is expecting. Please ensure that you are running Git &#62;&#61; 1.8.0 by executing `git --version`. You can get a full picture of the installation details by executing `pod install --verbose`.

Make sure to always open the Xcode workspace `Jumpstart.xcworkspace` instead of the project file when building your project:
    
    $ open Jumpstart.xcworkspace

###4. Use the Mobile APIs

#### Call the Helloworld controller API

Make the following changes to `ViewController.m`:

    // Import the header
    #import "HelloWorldController.h"
    
    // Initialize and call the controller
    - (void)viewDidLoad
    {
        [super viewDidLoad];
	    // Initialize controller
        HelloWorldController *helloWorldController = [[HelloWorldController alloc] init];
        // Call controller
        [helloWorldController getHello:@"Magnet" options:nil success:^(NSString *response) {
            NSLog(@"response = %@", response); // executed if the Mobile Backend returns a valid response
        } failure:^(NSError *error) {
            NSLog(@"error = %@", error); // executed if the Mobile Backend returns an error
        }];
    }

