# iOS Guidelines

## Project Structure
- Organized .xcodeproj: files are grouped by layers (Presentation, Domain/Model, Infrastructure, etc), responsibilities (UI, Model, Logic, Vendor, etc), roles, etc. No flat structure allowed
- Organized project folder: desirable but not required to keep file structure in sync with .xcodeproj
- *One .m / .swift == One class*. No extra classes allowed
- Localization is required even for the case, when you have only one language. Therefore adding extra language won't cause any difficulty in future
- Images and other resources (.plist) should be grouped as well (i.e. $SRCROOT/Resources/Images/Common/, $SRCROOT/Resource/Assets/)

## File Structure (.m)
- Structure
```
imports

related constants

class extension

@implementation
- dealloc
- init methods
- other
@end
```
- Method sections should be separated by #pragma mark - $sectionName

## Braces, Asterisk
- Curly braces are opened on the same line with code, closed - on a new line. For short blocks / closures a single line can be used
- Asterisk placed near the name ```Type *var = ...;```
- Every code block  should be wrapped by ```{}```:
```objective-c
if (statement) {
    NSLog(@"%@", var);
}
```
The only exception is a short statement and an immediate return in the beginning: 
```objective-c
- (void)performSomeTask {
    if (!user.hasToken) return;
    ...
}
```

## Spaces, Formatting
#### General
- Tab consists of 4 spaces
- Max symbols in a row: *120*
- Use space after ```if```, ```while```, ```for```  and similar:
```objective-c
if (pointer != someOtherPointer)

CGFloat result = width * height * 2.f;

BOOL contentExists = self.content.length > 0;

for (int i = 0; i < x; i++) { ... }
```
- Alignment: do not align code as ASCII-art.
- Separate logical code blocks with 1 line wrap. Random number of \n is not allowed

#### Variable declaration
- Between Type & Protocol there are no spaces: ```id<NSObject> object = ...;```
- Between var and casting parentheses there are no spaces: ```ClassType *a = (ClassType *)b;```, ```ScalarType a = (ScalarType)b;```

#### Class Declaration
- If listing protocols goes beyond 120 characters, place each on a separate line: 
```objective-c
@interface SCHCategoryViewController : UIViewController <UITableViewDelegate> 

@interface SCHCategoryViewController : UIViewController <
    UITableViewDelegate, 
    UITableViewDataSource, 
    SCHSyncronizationDelegate
>
```

#### Method Declaration
- Use a single space between +/- and a returned type. Any additional spaces in arguments list are not allowed:
```- (void)doSomethingWithString:(NSString *)string flag:(BOOL)flag;```
- If the method goes beyond 120+10 characters, place each argument on a separate line, align by colon:
```objective-c
- (void)doSomethingWithString:(NSString *)string
                         rect:(CGRect)rectangle
                       length:(NSUInteger)length;
```
#### Method Invocation
- Method invocation should use the same formatting as for declaration

#### Function Declaration
- No space symbol between name and arguments parentheses
- If the method goes beyond 120+10 characters, parentheses should be placed the same way, as opening/closing braces. Place each argument on a separate line aligned by 1 tab: 
```objective-c
void * LongLongLongFunction(
    id firstArgument, 
    id secondArgument,
    id *outArgument,
    int other
)

NSAssert(
    [controller conformsToProtocol:@protocol(EParticipantSelection)],
    @"Controller %@ should confrom to protocol",
    NSStringFromProtocol(@protocol(EParticipantSelection))
);
```

#### Access specifiers & ivars 
- ```@public```, ```@private```, ```@protected``` should be aligned by 1 tab: 
```objective-c
@interface MyClass : NSObject {
    @private
    id _privateIvar;

    @protected
    id _protectedIvar;
}
```
- Access specifiers should be declared explicitly

#### Property Declaration
- Between ```@property``` and opening parentheses 1 space symbol should be used
- Parameters ordering: atomic/nonatomic, memory policy, access specified (readonly/readwrite), setter declaration, getter declaration: ```@property (nonatomic, copy, readonly, getter=customGetter) NSString *value;```

#### Protocol Declaration
- ```@required``` and ```@optional``` aligned to the line beginning

#### Blocks / Closures Declaration
- Code inside block / closure aligned by 1 tab
- Opening and closing braces should be aligned by the first symbol of declaration: 
```objective-c
dipatch_async(dispatch_get_main_queue(), ^{
    // your code here
});

NSArray *contentArray = nil;
[contentArray enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop){
     // your code here
}];

[UIView animateWithDuration:0.3 animations:^{
    // your code here
} completion:^(BOOL finished){
     // your code here
}];
```
- There is no space between ```^``` and return type / arguments: ```^(id response){}``` ```^NSUInteger * (id response){}```

## Naming
In general we're using [Apple Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingBasics.html#//apple_ref/doc/uid/20001281-BBCHBFAH) with several highlights:
- Three-letters prexif is REQUIRED
- Methods for the non-project class categories should be prefixed by ```lowercasePrefix_```: 
```- (void)sch_performTask:(id)arg```

## Constants
- Magical numbers are not allowed - constants should be used instead. Exception: constant value, used in local one place with descriptive name
- No "raw" strings. Exception: the same as for the magical numbers
- Desirable to use ```extern``` keyword:
```objective-c
// SCHNotifier.h
OBJC_EXTERN NSString * const SCHNotifierDidChangeStateNotification;
OBJC_EXTERN const CGFloat SCHDefaultAnimationDuration;

// SCHNotifier.m

NSString * const SCHNotifierDidChangeStateNotification = @"com.project.notifier.stateDidChange";
const CGFloat SCHDefaultAnimationDuration = 0.33;
```

## Required / recommended best practices
#### Types Declaration / Usage
- Use typedef for blocks declaration and custom enums / scalar types
- Use NSInteger, NSTimeInterval, CGFloat, etc over plain types. Therefore it is easier to migrate to the 64-bits

#### Forward Declaration
Use [forward declaration](http://railsware.com/blog/2013/08/09/using-forward-declaration-in-your-objective-c-projects/) whenever possible

#### Ivars
```@public``` ivars not allowed 

#### Properties
- Use ```copy``` specifier in case mutability leads to unexpected behaviour (i.e. when NSMutableString passed as NSString and mutated outside of the class)
- Use ```readonly``` for public properties whenever possible to prevent unexpected class usage (i.e. outlet assigned from client code, etc)
- (iOS only) Use ```nonatimic``` whenever possible to speedup code execution
- Desirable to access underlying property's ivar only in init/setter/getter
- Use of @property solely to create _ivar is not recommended and should be prohibited

#### Exceptions handling
Use exceptions only where it is required. Desirable to use NSError ** and / or return result status (i.e. Success, Fail)

#### Protocols
- Delegate methods should always pass ```sender``` argument: 
```objective-c
@protocol CustomClassDelegate <NSObject>

- (NSInteger)someDelegateMethod:(CustomClass *)customClass;
- (void)customClass:(CustomClass *)customClass didSelectRowAtIndexPath:(NSIndexPath *)indexPath;

@end
```
- Protocol support for properties / vars should be declared explicitly: ```id<Protocol> instance = ...;```, ```@property (nonatomic, weak) id<Protocol> delegate;```

#### Boolean statements
- Desirable to use implicit equality check: ```if (bar && baz && quux)```

#### Preprocessor usage
- Prefer c-functions / constants / methods over #define

#### AppDelegate
Keep your AppDelegate as clean as possible. Any logic, not related to AppDelegate (database seeding, networking, etc) is not allowed 

#### Clean .pch
Group required #defines, constants to a separate header (SCHConstants.h, SCHDefines.h). Garbage in .pch is not allowed
