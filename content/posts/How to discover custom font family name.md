---
title: "How to Discover the Family Name of a Custom Font in Swift"
date: 2025-03-18
tags:
- Swift
- Foundation
---

It often happens that, in your app, you want to use a custom font that is not San Francisco. When this happens, after downloading the necessary files in Xcode, you try to use it in SwiftUI.
And then...
It doesn't work... ☹️

Most likely, unless you made a mistake earlier, you selected the wrong font family name when initializing the font.

```swift
    Font.custom("Family-Name", size: size)
```

It may happen that the file name does not match the actual font family name. When this happens to me, to quickly solve the problem in Swift, I search among the fonts installed at that moment to check, first of all, if the font was installed correctly, and then to see what name I need to use to reference it.

```swift 
for family in UIFont.familyNames.sorted() {
    let names = UIFont.fontNames(forFamilyName: family)
    dump("Family: \(family) Font names: \(names)")
}
```

I encountered this problem while developing [Outsoon](https://outsoon.unicorndonkeys.com). My team and I decided to use the [DM Sans](https://fonts.google.com/specimen/DM+Sans) font, but after downloading it, I couldn't apply it. Thanks to this method, I quickly found the correct family names and created an extension to easily manage the different font versions.

```swift
extension Font {
    
    /// This fuction is used in order to fint the right custom font family
    /// every time that is needed. This app use a custom font family with different
    /// weight.
    /// - Parameters:
    ///   - weight: The forn weight needed.
    ///   - size: The size of the font needed.
    /// - Returns: The custom font requested.
    static func dmSans(_ weight: Font.Weight, size: CGFloat) -> Font {
        return switch weight {
            case .black:
                Font.custom("DMSans-9ptRegular_Black", size: size)
            case .bold:
                Font.custom("DMSans-9ptRegular_Bold", size: size)
            case .heavy:
                Font.custom("DMSans-9ptRegular_ExtraBold", size: size)
            case .light:
                Font.custom("DMSans-9ptRegular_Light", size: size)
            case .medium:
                Font.custom("DMSans-9ptRegular_Medium", size: size)
            case .regular:
                Font.custom("DMSans-9ptRegular_Regular", size: size)
            case .semibold:
                Font.custom("DMSans-9ptRegular_SemiBold", size: size)
            case .thin:
                Font.custom("DMSans-9ptRegular_Thin", size: size)
            case .ultraLight:
                Font.custom("DMSans-9ptRegular_ExtraLight", size: size)
            default:
                Font.custom("DMSans-9ptRegular", size: size)
        }
    }
    
}
```

This makes it much easier to use the custom font without having to manually write a string each time, reducing the risk of errors.

```swift
.font(.dmSans(.regular, size: 16))
```

Thanks to this solution, I can integrate the custom font into my app without the risk of errors and without having to manually remember each family name. This method is especially useful when working with multiple font weights and wanting to keep the code more readable and maintainable.