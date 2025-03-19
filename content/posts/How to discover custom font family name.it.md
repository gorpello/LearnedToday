---
title: "Come scoprire il family name di un font custom in Swift"
date: 2025-03-18
tags:
- Swift
- Foundation
---

Spesso, nelle proprie app, si vuole usare un font custom diverso da San Francisco, quando questo succede, dopo aver scaricato i file necessari in Xcode, proviamo ad utilizzarlo in SwiftUI.
E poi...  
Non funziona... ☹️

Molto probabilmente, a meno che non abbiamo sbagliato qualcosa prima, abbiamo sbagliato a selezionare il font family name nell'inizializzazione del Font.  

```swift
    Font.custom("Family-Name", size: size)
```

Può succedere che il nome del file non corrisponda al font family name effettivo. Quando mi succede, per risolvere velocemente il problema in Swift, cerco tra i font installati per capire, se prima di tutto il font sia stato installato correttamente, e successivamente con quale nome devo fare riferimento per utilizzarlo.

```swift 
for family in UIFont.familyNames.sorted() {
    let names = UIFont.fontNames(forFamilyName: family)
    dump("Family: \(family) Font names: \(names)")
}
```

Questo problema l'ho riscontrato durante lo sviluppo di [Outsoon](https://outsoon.unicorndonkeys.com), il mio team e io abbiamo deciso di usare il font [DM Sans](https://fonts.google.com/specimen/DM+Sans), dopo aver scaricato il font non riuscivo ad applicarlo. Velocemente grazie a questo metodo sono riuscito a trovare i family name corretti, creando un'estensione per gestire facilmente le varie versioni del font.

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

Questo mi rende molto facile l'utilizzo del font custom, senza dover manualmente scrivere una String ogni volta, rischiando di commettere errori.

```swift
.font(.dmSans(.regular, size: 16))
```

Grazie a questa soluzione, posso integrare il font personalizzato nella mia app senza il rischio di errori e senza dover ricordare manualmente ogni nome di famiglia. Questo metodo è particolarmente utile quando si lavora con più font o più pesi del font e si vuole mantenere il codice più leggibile e manutenibile.