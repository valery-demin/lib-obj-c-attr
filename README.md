[![Build Status](https://api.travis-ci.org/epam/lib-obj-c-attr.png)](https://travis-ci.org/epam/lib-obj-c-attr) [![License](https://go-shields.herokuapp.com/license-BSD%203-blue.png)](http://opensource.org/licenses/BSD-3-Clause) [![Platform](https://cocoapod-badges.herokuapp.com/p/ROADFramework/badge.png)](https://github.com/epam/road-ios-framework/)


#libObjCAttr - attributes for your code

libObjCAttr is attribute-oriented programming for Objective-C language. It is compile-time based with support of Xcode auto-completion and errors tracing features. We nailed every aspect of implementation including performance so attributes will never be a bottleneck.

Our solution helps to reduce complexity of code, makes it compact and self-documented. There are a lot of languages with attributes support, we added Objective C into that list.

##Installation

[CocoaPods](http://cocoapods.org) is the only recommended way of libObjCAttr integration. Besides standard configuration of pod dependencies *pod_install* hook definition required as shown below. Typical **Podfile** will looks like following:

```ruby
pod 'libObjCAttr'

post_install do |installer|
    require File.expand_path('ROADConfigurator.rb', './Pods/libObjCAttr/libObjCAttr/Resources/')
    ROADConfigurator::post_install(installer)
end
```


##Samples

It super easy to mark your code with attribute. We currently supports **4 types** of attributes:

* *Class attributes*
* *IVar attributes*
* *Method attributes*
* *Property attributes*

Check out this example to get know how to declare them:

```objective-c
RF_ATTRIBUTE(MyAttribute, stringProperty = @"Hello world")
@interface AnnotatedClass : NSObject {

    RF_ATTRIBUTE(MyAttribute, numericProperty = @9000)
    NSObject * _someField;
}

RF_ATTRIBUTE(MyAttribute, blockProperty = ^(int value) { return value; })
- (void)foo;

RF_ATTRIBUTE(MyAttribute, objectProperty = [NSObject new])
@property NSString *name;

@end
```

Attribute can be any class inherited from NSObject or its subclasses. It may or may not have properties.

```objective-c
#import <ROAD/ROADAttribute.h>

@interface MyAttribute : NSObject

@property NSString *stringProperty;
@property NSNumber *numericProperty;
@property id blockProperty;
@property id objectProperty;

@end
```

Now you can get them in runtime with code like this:

```objective-c
...

NSArray *classAttributes = [AnnotatedClass RF_attributesForClass];
NSArray *ivarAttributes = [AnnotatedClass RF_attributesForIvar:@"_someField"];

// Let's filter it, in case there are many of them
MyAttribute *methodAttributes = [AnnotatedClass RF_attributeForMethod:@"foo" withAttributeType:[MyAttribute class]];
MyAttribute *classAttributes = [AnnotatedClass RF_attributeForProperty:@"name" withAttributeType:[MyAttribute class]];

...
```

##Requirements
libObjCAttr requires **iOS 5.0 and above**. Compatibility with 4.3 and older is not tested.

libObjCAttr was initially designed to use **ARC**.

##License
libObjCAttr is made available under the terms of the [BSD-3](http://opensource.org/licenses/BSD-3-Clause). See the LICENSE file that accompanies this distribution for the full text of the license.
