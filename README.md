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
- Asterisk placed near the name ```Type *var = ...```
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
