---
title: "Usługi Azure AD v2 Windows Desktop pobieranie rozpoczęte — Config | Dokumentacja firmy Microsoft"
description: "Jak aplikacji .NET pulpitu systemu Windows (XAML) można uzyskać tokenu dostępu i wywołania funkcji API chronione przez punkt końcowy w wersji 2 usługi Azure Active Directory."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 5e83171846517496e221f0a84565cdf7b77514df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Dodawanie informacji o rejestracji aplikacji do aplikacji
W tym kroku musisz dodać identyfikator aplikacji do projektu.

1.  Otwórz `App.xaml.cs` i Zastąp wiersz zawierający `ClientId` z:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Co to jest dalej

[Testowanie i weryfikowanie](active-directory-mobileanddesktopapp-windowsdesktop-test.md)
