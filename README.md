## Magnet Jumpstart App for iOS

The Magnet Jumpstart App for iOS is licensed under the terms of the [Magnet Software License Agreement](http://www.magnet.com/resources/tos.html).  Please see [LICENSE](LICENSE) file for full details.

In this tutorial, you will learn how to build a "Jumpstart" iOS app that interacts with a "Jumpstart" server running locally.

### Prerequisites
1. MAB tool
2. Xcode 5
3. CocoaPods

### Build the server

To build the Jumpstart server, you can run the following MAB command:

    jumpstart@local:mab> run jumpstart.mab

### Create Xcode project
Create a new Single View Application called "Jumpstart" using Xcode:
![Create Project](https://dl.dropboxusercontent.com/u/25131624/Xcode-Create-Project.png)

### Import dependencies using CocoaPods

#### Generate the mobile API
You can generate the mobile API by running the following MAB command:

    jumpstart@local:mab> api-generate ios
    
This command would generate the mobile API in the following directory: `~/MABProjects/jumpstart/mobile/apis/assets/ios`
    

If you haven't installed CocoaPods already, you can follow the installation instructions on their website: http://cocoapods.org

#### Create a Podfile
Create a Podfile in your Xcode project directory.    

    platform :ios, '7.0'

    pod 'MagnetMobileServer', :git => 'git@bitbucket.org:magneteng/magnet-sdk-ios-2.3.0.git'
    pod 'magnet-mobile-assets', :path => 'Source'

    target :AppTests, :exclusive => true do
        pod 'Kiwi/XCTest'
    end
