## Magnet Jumpstart App for iOS

The Magnet Jumpstart App for iOS is licensed under the terms of the [Magnet Software License Agreement](http://www.magnet.com/resources/tos.html).  Please see [LICENSE](LICENSE) file for full details.

In this tutorial, you will learn how to build a "Jumpstart" iOS app that interacts with a "Jumpstart" Mobile Backend running locally.

###1. Prerequisites
1. Mobile App Builder tool [provide link for MAB installation]
2. Xcode 5
3. CocoaPods [provide link for CocoaPods installation]

###2. Build the Mobile Backend

#### Use the Mobile App Builder tool to first build a Mobile Backend server.
The following command will automatically build a sample Mobile Backend server for the Jumpstart app that contains two controller APIs: HelloWorld and basic operations like create, read, update and delete on a sample Entity:

    jumpstart@local:mab> run jumpstart.mab

You can also find detailed instructions for building the above Mobile Backend server from scratch here: [INSERT LINK]

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

###5. Use the Mobile APIs

#### Call the HelloWorld controller API

The HelloWorld controller API concatenates the string `Hello ` with the input string argument and returns it. For example, given the input string argument `Magnet` it returns the string `Hello Magnet`.

To call the HelloWorld controller API, follow these steps:

###### Import the HelloWorldController header
    
    #import "HelloWorldController.h"
    
###### Initialize the HelloWorldController controller

    HelloWorldController *helloWorldController = [[HelloWorldController alloc] init];
    
###### Call the HelloWorldController controller

    [helloWorldController getHello:@"Magnet" options:nil success:^(NSString *response) {
        NSLog(@"response = %@", response); // executed if the Mobile Backend returns a valid response
    } failure:^(NSError *error) {
        NSLog(@"error = %@", error); // executed if the Mobile Backend returns an error
    }];

#### Call the SimpleEntity controller API

The SimpleEntity controller API provides basic operations like create, read, update and delete on a sample Entity.

To call the SimpleEntity controller API, follow these steps:

###### Import the SimpleEntityController header
    
    #import "SimpleEntityController.h"
    
###### Initialize the SimpleEntityController controller

    SimpleEntityController *simpleEntityController = [[SimpleEntityController alloc] init];
    
###### Call the SimpleEntityController controller

    // Initialize a SimpleEntityBean
    SimpleEntityBean *simpleEntityBean = [[SimpleEntityBean alloc] init];
    simpleEntityBean.name = @"John Appleseed";
    simpleEntityBean.customerId = arc4random() % 100000; // Generate a random customerId
    
    // Initialize a SimpleValueBean
    SimpleValueBean *simpleValueBean = [[SimpleValueBean alloc] init];
    simpleValueBean.boolean = NO;
    simpleValueBean.character = @"c";
    simpleValueBean.bigDecimal = [NSDecimalNumber decimalNumberWithString:@"1.0"];
    simpleEntityBean.value = simpleValueBean;
    
    // Call the controller to create the SimpleEntityBean
    [simpleEntityController create:simpleEntityBean options:nil success:^(int response) {
        NSLog(@"response = %d", response); // executed if the Mobile Backend returns a valid response
    } failure:^(NSError *error) {
        NSLog(@"error = %@", error); // executed if the Mobile Backend returns an error
    }];

#### Sample code
Add the above code snippets to ViewController.m as follows:

    #import "ViewController.h"
	// Import controller API headers
	#import "HelloWorldController.h"
	#import "SimpleEntityController.h"
	#import "SimpleEntityBean.h"
	#import "SimpleValueBean.h"

	@interface ViewController ()

	@end

	@implementation ViewController

	- (void)viewDidLoad
	{
	    [super viewDidLoad];
	    // Call HelloWorldController
	    [self callHelloWorldController];
	    // Call SimpleEntityController
	    [self callSimpleEntityController];
	}

	- (void)callHelloWorldController {
	    // Initialize controller
	    HelloWorldController *helloWorldController = [[HelloWorldController alloc] init];
    
	    // Call controller
	    [helloWorldController getHello:@"Magnet" options:nil success:^(NSString *response) {
	        NSLog(@"response = %@", response); // executed if the Mobile Backend returns a valid response
	    } failure:^(NSError *error) {
	        NSLog(@"error = %@", error); // executed if the Mobile Backend returns an error
	    }];
	}

	- (void)callSimpleEntityController {
	    // Initialize controller
	    SimpleEntityController *simpleEntityController = [[SimpleEntityController alloc] init];
    
	    // Initialize a SimpleEntityBean
	    SimpleEntityBean *simpleEntityBean = [[SimpleEntityBean alloc] init];
	    simpleEntityBean.name = @"John Appleseed";
	    simpleEntityBean.customerId = arc4random() % 100000; // Generate a random customerId
    
	    // Initialize a SimpleValueBean
	    SimpleValueBean *simpleValueBean = [[SimpleValueBean alloc] init];
	    simpleValueBean.boolean = NO;
	    simpleValueBean.character = @"c";
	    simpleValueBean.bigDecimal = [NSDecimalNumber decimalNumberWithString:@"1.0"];
	    simpleEntityBean.value = simpleValueBean;
    
	    // Call the controller to create the SimpleEntityBean
	    [simpleEntityController create:simpleEntityBean options:nil success:^(int response) {
	        NSLog(@"response = %d", response); // executed if the Mobile Backend returns a valid response
	    } failure:^(NSError *error) {
	        NSLog(@"error = %@", error); // executed if the Mobile Backend returns an error
	    }];
	}

    @end


###6. Deploy the Mobile Backend
You can deploy the Mobile Backend server for the Jumpstart app to your local machine by running the following command on the Mobile App Builder tool:

    jumpstart@local:mab> server-start
    
###7. Run the app
You are now ready to run the Jumpstart app on the iOS simulator using Xcode!

