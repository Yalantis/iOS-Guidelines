# iOS Guidelines

## General
- Prefer c-functions / constants over #define.

## Project Structure
- Organized .xcodeproj: files are grouped by layers (Presentation, Domain/Model, Infrastructure, etc), responsibilities (UI, Model, Logic, Vendor, etc), roles, etc. No flat structure allowed.
- Organized project folder: desirable but not required to keep file structure in sync with .xcodeproj
- *One .m / .swift == One class*. No extra classes allowed.
- Localization is required even for case, when you have only one language. Therefore in future it worth nothing to add extra language.
- Images and other resources (.plist) should be grouped as well (i.e. SRCROOT/Resources/Images/Common/, SRCROOT/Resource/Assets/)

## Braces, Asterisk
- Opening curly braces on the same line. Closing - on new line. For short blocks / closures single line can be used.
- Asterisk placed near the name ```Type *var = ...;```
- Every code block  should be wrapped by ```{}```:
```objective-c
if (statement) {
  NSLog(@"%@", var);
}
```
The only exception is short statement and immediate return in the beggining: 
```objective-c
- (void)performSomeTask {
    if (!user.hasToken) return;
    ...
}
```

## Spaces, Formatting
#### General
- Tab consists of 4 spaces
- Max symbols in row: 120
- Use space after ```if```, ```while```, ```for```  and similar:
```objective-c
if (pointer != someOtherPointer)

CGFloat result = width * height * 2.f;

BOOL contentExists = self.content.length > 0;

for (int i = 0; i < x; i++) { ... }
```
- Alignment: do not align code as ASCII-art.
- Separate logical code blocks by *1* line wrap. Random number of \n not allowed.

#### Variable declaration
- Between Type & Protocol there are no spaces: ```id<NSObject> object = ...;```
- Between var and casting parentheses there are no spaces: ```ClassType *a = (ClassType *)b;```, ```ScalarType a = (ScalarType)b;```

#### Class Declaration
- In case when listing protocols goes beyond 120 characters: place each on separate line: 
```objective-c
@interface SCHCategoryViewController : UIViewController <UITableViewDelegate> 

@interface SCHCategoryViewController : UIViewController <
    UITableViewDelegate, 
    UITableViewDataSource, 
    SCHSyncronizationDelegate
>
```
- use [forward declaration](http://railsware.com/blog/2013/08/09/using-forward-declaration-in-your-objective-c-projects/) whenever possible

#### Method Declaration
- Use single space between +/- and returned type. Any additional spaces in arguments list are not allowed: ```- (void)doSomethingWithString:(NSString *)string flag:(BOOL)flag;```
- In case when method goes beyond 120+10 characters: place each argument on separate line, align by colon:
```objective-c
- (void)doSomethingWithString:(NSString *)string
                         rect:(CGRect)rectangle
                       length:(NSUInteger)length;
```
#### Method Invocation
- Method invocation should use same formatting as for declaration

#### Function Declaration
- No space symbol between name and arguments parentheses
- In case when method goes beyond 120+10 characters: parentheses should be placed in the same way, as opening/closing braces, place each argument on separate line, aligned by 1 tab: 
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
- Between ```@property``` and opening parentheses 1 space should be used
- Parameters ordering: atomic/nonatomic, memory policy, access specified (readonly/readwrite), setter declaration, getter declaration: ```@property (nonatomic, copy, readonly, getter=customGetter) NSString *value;```

#### Protocol Declaration
- ```@required``` and ```@optional``` aligned to the line beginning

#### Blocks / Closures Declaration
- Code inside block / closure aligned by 1 tab
- Openning and closing braces should be aligned by first symbol of declaration: 
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
