---
title: "Azure AD w wersji 2 dla systemu iOS Getting Started — Konfigurowanie (ARP) | Dokumentacja firmy Microsoft"
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
ms.openlocfilehash: 50cb4a2803b6aebe8b39ec9fb02da2293c1065fa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
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
