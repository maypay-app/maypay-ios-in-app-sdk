# Maypay

![Swift Version](https://img.shields.io/badge/swift-5.3.0-orange.svg)

An integration for Maypay app opening using deeplinks. It allows to check if the Maypay app in installed on device and open it with a paymentRequestId.
It also integrate default Maypay button for both SwiftUI and UIKit.

## Installation

### Swift Package Manager

For installation with [Swift Package Manager](https://github.com/apple/swift-package-manager), simply add the following to your `Package.swift`:

```swift
.package(url: "https://github.com/maypay-app/maypay-ios-in-app-sdk", from: "1.0.0")
```

## Prerequisites
To allow your app to open Maypay and let the user complete the payment process, you must add the Maypay URL Scheme into your app `plist.info`. 
As specified in the [Apple Documentation](https://developer.apple.com/documentation/uikit/uiapplication/1622952-canopenurl) you need to add the `LSApplicationQueriesSchemes` array key with `maypay` as value.

```xml
<dict>
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>maypay</string>
    </array>
</dict>
```



## Usage

### SwiftUI

You can display the default Maypay button by importing the MaypaySwiftUI library and adding the MaypayButton with your requestId.

```swift
import SwiftUI
import Maypay
import MaypaySwiftUI

public struct MaypayButton: View {

    @State var requestId: String = "" // You must retrieve your requestId from server.
    
    public var body: some View {
        VStack {
            MaypayButton(requestId: self.requestId)
        }
    }
    
}

```

### UIKit

You can display the default Maypay button by importing the MaypayUIKit library and adding the MaypayButton with your requestId.
To correctly import the Maypay button into your UIKit app, carry out the following steps:
1. Verify you imported the both `Maypay` and `MaypayUIKit`.
2. Insert a new UIButton in your storyboard.
3. In the Attribute Inspector give the button an empty name.
4. In the Identity Inspector assign the `MaypayButton` class from the `MaypayUIKit` module to the Button.
5. Link the button in your ViewController and assign MaypayButton as a subView.
 
```swift
import UIKit
import Maypay
import MaypayUIKit

class ViewController: UIViewController {

    @IBOutlet weak var maypayButton: MaypayButton!
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        maypayButton.addSubview(MaypayButton(requestId: "your_request_id")) 
        
    }
}
```

### Custom Integration

If you want to implement your custom button to handle the Maypay app opening you can use `canOpenMaypay` and `openMaypay` functions.

#### `canOpenMaypay()`
Returns a Boolean value that indicates whether your app is available to open Maypay or not.

#### `openMaypay(requestId: String)`
Redirect the user to Maypay handling the given requestId, or redirect to the Maypay AppStore page.

```swift
import Maypay

let requestId: String = "retrieve_this_from_server"
let canOpen = Maypay.canOpenMaypay()
if(canOpen){
    Maypay.openMaypay(requestId: requestId)
}
```