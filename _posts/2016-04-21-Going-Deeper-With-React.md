---
layout: post
title: Going Deeper With React Native and iOS Native UI
---

React-Native is sparse on how to reach deeply into iOS.

[They're great on the basics like sending callbacks and promises from the JS side to the native iOS side, and creating events and properties on the native side that you can make available or subscribe to on the JS side.](https://facebook.github.io/react-native/docs/native-modules-ios.html#content)

But they're really sparse on two deeper topics:

1. How to approach triggering native UI.
2. How to pass values from native UI interactions back to the JS side.

Ultimately, triggering native iOS UI and passing values from native UI interactions back to RN requires two additional steps that aren't covered in the RN documentation. These were the missing steps in building this iOS9 CNContactPicker API I put together(https://github.com/nsipplswezey/react-native-iOS9-contact-picker).

And a demo gif of the outcome:

![Demo Gif](https://nsipplswezey.github.io/assets/anim.gif)

**Step 1: Put your JS callbacks/promises (RCTSenderResponseBlocks) in a mutable array that can be passed between your Objective-C methods**

```objective-c
  //RCTNativeUIViewManager.m
  @implementation RCTNativeUIViewManager.m
  {
     NSMutableArray<RCTResponseSenderBlock> *_callbacks;
  };

  RCT_EXPORT_METHOD(openNativeUIView:(NSDictionary *) args
                    callback:(RCTResponseSenderBlock)callback)
  {
    _callbacks = [NSMutableArray new];
    [_callbacks addObject:callback];
```

**Step 2: Walk to the current top view controller before presenting your new native UI view controller.**

```objective-c
  UIViewController *presentingController = RCTKeyWindow().rootViewController;
    if(presentingController == nil) {
       RCTLogError(@"Tried to display contact picker, but there is no application window. args:%@",args);
    }

    // Walk the chain to the topmost model view controller.
    while(presentingController.presentedViewController) {
      presentingController = presentingController.presentedViewController;
    }

    CNContactPickerViewController *contactPickerController = [CNContactPickerViewController new];

    NSArray *displayedItems = @[CNContactPhoneNumbersKey,CNContactEmailAddressesKey,CNContactBirthdayKey];
    contactPickerController.delegate = self;
    contactPickerController.displayedPropertyKeys = displayedItems;

    [presentingController presentViewController:contactPickerController animated:YES completion:nil];
```

These two steps give you everything you need to implement your ContactPickerViewControllerDelegate, using the callbacks in your array to return the result of the contact selection to the JS side.

```objective-c
  - (void)contactPicker:(CNContactPickerViewController *)contactPickerController didSelectContactProperty:(CNContactProperty*)contactProperty
  {
    //Get the selected phone number and contact name
    CNContact* contact = contactProperty.contact;
    CNPhoneNumber* contactPhone = contactProperty.value;
    NSString* phoneNumber = contactPhone.stringValue;
    NSString* contactName = [CNContactFormatter stringFromContact:contact style:CNContactFormatterStyleFullName];

    //Get your callback out of your array
    RCTResponseSenderBlock callback = _callbacks[0];

    //Pass your contactName and phoneNumber to the callback
    callback(@[contactName,phoneNumber]);

    //Remove the callback
    [_callbacks removeObjectAtIndex:0];

  }
```

Of course there are multiple potential reasons for the sparseness of documentation.

* React-Native wants to replace all native UI with equivalent or better React-Native views, so why provide extensive examples of creating native UI views?
* Highlighting Objective-C could become a red-herring and turn-off for developers who want to write iOS apps purely in react and javascript.
* React-Native is young and in active development. Developers haven't had a lot of time to dig in deep, write documentation, examples and blog posts.

[Here's the source in the form of a launchable a demo react-native project that uses the contact picker component.](https://github.com/nsipplswezey/react-native-iOS9-contact-picker)
