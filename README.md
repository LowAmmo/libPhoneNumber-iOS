# Internal Oracle Cerner Repo

This is an internal copy/fork of the public repo from github.com - https://github.com/iziz/libPhoneNumber-iOS

The goal/hope is that this is a temporary fork while waiting for the main repo to make necessary/required updates to continue building with the latest/newest build tools.

## Process

To update/contribute to this repo, the process is shortened - and relying on true testing taking place via the consuming components / applications.

### Contributing

To contribute / update the repo, submit Pull Requests onto the `cerner-main` branch.

### Branching

##### Security

The official branches are locked down just to prevent accidental modifications. Any changes done to cerner-release will require modifying the Settings for this repo to remove restrictions on the branch. Please make sure to reapply the restrictions when done.

#### `cerner-main`

Our "custom" code that goes ontop of the official code from the primary code repository.

#### `cerner-release`

When this branch is pushed / updated, a new "release" is published to the internal cocoapod spec repo.

**Make sure to tag each published version**

_ONLY PUSH/MODIFY THIS BRANCH WHEN RELEASING A NEW VERSION OF THIS COMPONENT_

### Versioning

Each 'custom' version should append `-cerner` to indicate it's a "cerner only" release in our internal cocoapod spec repo.

Version the base version with the correlating version from the original public repo.

#### Example

If the base public release is `1.2.3`, the internal release that addresses the necessary issue(s) would be `1.2.3-cerner`.

Subsequent Cerner releases on the same version would append a version number after the `-cerner` postfix: `1.2.3-cerner3`, etc.

### Tagging

When releasing a new version, make sure to do a github Release describing the changes in the release, etc.


#### New Public Version Released

If the main repo publishes a new release, that does not address the necessary changes to stop using this internal version, an updated internal release should be done.

1. Do a hard reset of the `cerner-main` branch to the new base release of the original repo (could be to `main` or `master` or to a version tagged commit)
2. Cherry-pick applicable commits from the old `cerner-main`
3. If any changes to the old commits are necessary, do a new Pull Request onto `cerner-main` INSTEAD OF cherry-picking

In general, every internal release should contain at least 3 commits:

1. Initial commit preparing the internal version of the repo:
  * These updates to the README.md
  * Pull Request Template
  * Jenkinsfile
  * Podspec updates to point to this repo (if necessary)
2. Necessary changes to fix issues or enable building, etc.
3. Setting the vew version to publish

Keeping each commit separate increases the odds that just cherry picking will work for future updates (except for the new version setting).

&nbsp;


# -- Original README.md From Public Repo --

&nbsp;

&nbsp;


[![CocoaPods](https://img.shields.io/cocoapods/p/libPhoneNumber-iOS.svg?style=flat)](http://cocoapods.org/?q=libPhoneNumber-iOS)
[![CocoaPods](https://img.shields.io/cocoapods/v/libPhoneNumber-iOS.svg?style=flat)](http://cocoapods.org/?q=libPhoneNumber-iOS)
[![Travis](https://travis-ci.org/iziz/libPhoneNumber-iOS.svg?branch=master)](https://travis-ci.org/iziz/libPhoneNumber-iOS)
[![Coveralls](https://coveralls.io/repos/iziz/libPhoneNumber-iOS/badge.svg?branch=master&service=github)](https://coveralls.io/github/iziz/libPhoneNumber-iOS?branch=master)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)

# **libPhoneNumber for iOS**

 - NBPhoneNumberUtil
 - NBAsYouTypeFormatter

> ARC only

## Update Log
[https://github.com/iziz/libPhoneNumber-iOS/wiki/Update-Log](https://github.com/iziz/libPhoneNumber-iOS/wiki/Update-Log)


## Issue
You can check phone number validation using below link.
https://rawgit.com/googlei18n/libphonenumber/master/javascript/i18n/phonenumbers/demo-compiled.html

Please report, if the above results are different from this iOS library.
Otherwise, please create issue to following link below to request additional telephone numbers formatting rule.
https://github.com/google/libphonenumber/issues

Metadata in this library was generated from that. so, you should change it first. :)

## Install

#### Using [CocoaPods](http://cocoapods.org/?q=libPhoneNumber-iOS)
```
source 'https://github.com/CocoaPods/Specs.git'
pod 'libPhoneNumber-iOS', '~> 0.8'
```
##### Installing libPhoneNumber Geocoding Features
```
pod 'libPhoneNumberGeocoding', :git => 'https://github.com/CocoaPods/Specs.git'
```

#### Using [Carthage](https://github.com/Carthage/Carthage)

 Carthage is a decentralized dependency manager that automates the process of adding frameworks to your Cocoa application.

 You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate libPhoneNumber into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "iziz/libPhoneNumber-iOS"
```

And set the **Embedded Content Contains Swift** to "Yes" in your build settings.

#### Setting up manually
 Add source files to your projects from libPhoneNumber
    - Add "Contacts.framework"

See sample test code from
> [libPhoneNumber-iOS/libPhoneNumberTests/ ... Test.m] (https://github.com/iziz/libPhoneNumber-iOS/tree/master/libPhoneNumberTests)

## Usage - **NBPhoneNumberUtil**
```obj-c
 NBPhoneNumberUtil *phoneUtil = [NBPhoneNumberUtil sharedInstance];
 NSError *anError = nil;
 NBPhoneNumber *myNumber = [phoneUtil parse:@"6766077303"
                              defaultRegion:@"AT" error:&anError];
 if (anError == nil) {
     NSLog(@"isValidPhoneNumber ? [%@]", [phoneUtil isValidNumber:myNumber] ? @"YES":@"NO");

     // E164          : +436766077303
     NSLog(@"E164          : %@", [phoneUtil format:myNumber
                                       numberFormat:NBEPhoneNumberFormatE164
                                              error:&anError]);
     // INTERNATIONAL : +43 676 6077303
     NSLog(@"INTERNATIONAL : %@", [phoneUtil format:myNumber
                                       numberFormat:NBEPhoneNumberFormatINTERNATIONAL
                                              error:&anError]);
     // NATIONAL      : 0676 6077303
     NSLog(@"NATIONAL      : %@", [phoneUtil format:myNumber
                                       numberFormat:NBEPhoneNumberFormatNATIONAL
                                              error:&anError]);
     // RFC3966       : tel:+43-676-6077303
     NSLog(@"RFC3966       : %@", [phoneUtil format:myNumber
                                       numberFormat:NBEPhoneNumberFormatRFC3966
                                              error:&anError]);
 } else {
     NSLog(@"Error : %@", [anError localizedDescription]);
 }

 NSLog (@"extractCountryCode [%@]", [phoneUtil extractCountryCode:@"823213123123" nationalNumber:nil]);

 NSString *nationalNumber = nil;
 NSNumber *countryCode = [phoneUtil extractCountryCode:@"823213123123" nationalNumber:&nationalNumber];

 NSLog (@"extractCountryCode [%@] [%@]", countryCode, nationalNumber);
```
##### Output
```
2014-07-06 12:39:37.240 libPhoneNumberTest[1581:60b] isValidPhoneNumber ? [YES]
2014-07-06 12:39:37.242 libPhoneNumberTest[1581:60b] E164          : +436766077303
2014-07-06 12:39:37.243 libPhoneNumberTest[1581:60b] INTERNATIONAL : +43 676 6077303
2014-07-06 12:39:37.243 libPhoneNumberTest[1581:60b] NATIONAL      : 0676 6077303
2014-07-06 12:39:37.244 libPhoneNumberTest[1581:60b] RFC3966       : tel:+43-676-6077303
2014-07-06 12:39:37.244 libPhoneNumberTest[1581:60b] extractCountryCode [82]
2014-07-06 12:39:37.245 libPhoneNumberTest[1581:60b] extractCountryCode [82] [3213123123]
```

#### with Swift
##### Case (1) with Framework
```
import libPhoneNumberiOS
```

##### Case (2) with Bridging-Header
```obj-c
// Manually added
#import "NBPhoneNumberUtil.h"
#import "NBPhoneNumber.h"

// CocoaPods (check your library path)
#import "libPhoneNumber_iOS/NBPhoneNumberUtil.h"
#import "libPhoneNumber_iOS/NBPhoneNumber.h"

// add more if you want...
```

##### Case (3) with CocoaPods
import libPhoneNumber_iOS


##### - in swift class file
###### 2.x
```swift
override func viewDidLoad() {
    super.viewDidLoad()

    guard let phoneUtil = NBPhoneNumberUtil.sharedInstance() else {
        return
    }

    do {
        let phoneNumber: NBPhoneNumber = try phoneUtil.parse("01065431234", defaultRegion: "KR")
        let formattedString: String = try phoneUtil.format(phoneNumber, numberFormat: .E164)

        NSLog("[%@]", formattedString)
    }
    catch let error as NSError {
        print(error.localizedDescription)
    }
}
```

## Usage - **NBAsYouTypeFormatter**
```obj-c
 NBAsYouTypeFormatter *f = [[NBAsYouTypeFormatter alloc] initWithRegionCode:@"US"];
    NSLog(@"%@", [f inputDigit:@"6"]); // "6"
    NSLog(@"%@", [f inputDigit:@"5"]); // "65"
    NSLog(@"%@", [f inputDigit:@"0"]); // "650"
    NSLog(@"%@", [f inputDigit:@"2"]); // "650 2"
    NSLog(@"%@", [f inputDigit:@"5"]); // "650 25"
    NSLog(@"%@", [f inputDigit:@"3"]); // "650 253"

    // Note this is how a US local number (without area code) should be formatted.
    NSLog(@"%@", [f inputDigit:@"2"]); // "650 2532"
    NSLog(@"%@", [f inputDigit:@"2"]); // "650 253 22"
    NSLog(@"%@", [f inputDigit:@"2"]); // "650 253 222"
    NSLog(@"%@", [f inputDigit:@"2"]); // "650 253 2222"
    // Can remove last digit
    NSLog(@"%@", [f removeLastDigit]); // "650 253 222"

    NSLog(@"%@", [f inputString:@"16502532222"]); // 1 650 253 2222
```

## libPhoneNumberGeocoding

For more information on libPhoneNumberGeocoding and its usage, please visit [libPhoneNumberGeocoding](https://github.com/iziz/libPhoneNumber-iOS/blob/master/libPhoneNumberGeocoding/README.md) for more information.

## libPhoneNumberShortNumber

For more information on libPhoneNumberShortNumber and its usage, please visit [libPhoneNumberShortNumber](https://github.com/iziz/libPhoneNumber-iOS/blob/master/libPhoneNumberShortNumber/README.md) for more information.

##### Visit [libphonenumber](https://github.com/google/libphonenumber) for more information or mail (zen.isis@gmail.com)
