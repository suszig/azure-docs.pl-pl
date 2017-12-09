
## <a name="add-the-applications-registration-information-to-your-app"></a>Dodawanie informacji o rejestracji aplikacji do aplikacji

W tym kroku musisz dodać identyfikator aplikacji do projektu:

1.  W `ViewController.swift`, Zastąp wiersz rozpoczynający się "`let kClientID`" z:
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Kontrolowanie i kliknięcia, <code>Info.plist</code> wyświetlić menu kontekstowe, a następnie kliknij przycisk: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
W obszarze <code>dict</code> węzła głównego, należy dodać następujące:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Enter the application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
