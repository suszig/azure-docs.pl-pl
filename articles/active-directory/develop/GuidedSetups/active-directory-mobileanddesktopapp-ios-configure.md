---
title: "Wprowadzenie Azure AD w wersji 2 dla systemu iOS — Konfigurowanie | Dokumentacja firmy Microsoft"
description: "Jak aplikacje systemu iOS (Swift) można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 0ebca65585fc87bd4a85ba092cd423fce9540f58
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
## <a name="create-an-application-express"></a>Tworzenie aplikacji (Express)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Zarejestrować aplikację za pośrednictwem [portalu rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Wprowadź nazwę aplikacji i poczty e-mail
3.  Upewnij się, że zaznaczono opcję instrukcje konfiguracji
4.  Postępuj zgodnie z instrukcjami, aby uzyskać identyfikator aplikacji i wklej go w kodzie

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Dodaj swoje informacje rejestracyjne aplikacji do rozwiązania (zaawansowane)

1.  Przejdź do [portalu rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/portal/register-app)
2.  Wprowadź nazwę aplikacji i poczty e-mail
3.  Upewnij się, że jest zaznaczona opcja instrukcje konfiguracji
4.  Kliknij przycisk `Add Platform`, a następnie wybierz pozycję `Native Application` i kliknij przycisk`Save`
5.  Wróć do Xcode. W `ViewController.swift`, Zastąp wiersz rozpoczynający się "`let kClientID`" z Identyfikatorem aplikacji został zarejestrowany:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
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
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Zastąp <i> <code>[Your_Application_Id_Here]</code> </i> z identyfikatorem aplikacji został zarejestrowany
</li>
</ol>
