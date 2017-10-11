---
title: "Azure AD Connect: Wystąpienie usługi synchronizacji | Dokumentacja firmy Microsoft"
description: "Ta strona dokumentów uwagi dotyczące wystąpienia usługi Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Uwagi dotyczące wystąpienia
Azure AD Connect jest najczęściej używana z wystąpieniem na całym świecie usługi Azure AD i Office 365. Istnieją także inne wystąpienia, ale te mają różne wymagania dotyczące adresów URL i inne uwagi.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud (Niemcy)
[Niemcy Microsoft Cloud](http://www.microsoft.de/cloud-deutschland) to chmura suwerennych przez osobę zaufaną niemieckim danych.

| Adresy URL, aby otworzyć w serwera proxy |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Listy odwołania certyfikatów |

Po zalogowaniu się do dzierżawy usługi Azure AD, należy użyć konta w domenie onmicrosoft.de.

Aktualnie nie znajduje się w Niemczech Microsoft Cloud funkcje:

* **Azure AD Connect Health** nie jest dostępna.
* **Aktualizacje automatyczne** nie jest dostępna.
* **Zapisywanie zwrotne haseł** jest dostępna w wersji zapoznawczej wersji Azure AD Connect 1.1.570.0 i po.
* Inne usługi Azure AD Premium nie są dostępne.

## <a name="microsoft-azure-government-cloud"></a>Chmury Microsoft Azure dla instytucji rządowych
[Chmury Microsoft Azure dla instytucji rządowych](https://azure.microsoft.com/features/gov/) to chmura dla instytucji rządowych Stanów Zjednoczonych.

Ta chmura obsługiwanego przez wcześniejszych wersjach narzędzia DirSync. Z 1.1.180 kompilacji programu Azure AD Connect generacji chmury jest obsługiwana. Tej generacji używa USA — tylko do oparte na punktach końcowych i mieć różne listy adresów URL, aby otworzyć w serwera proxy.

| Adresy URL, aby otworzyć w serwera proxy |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Listy odwołania certyfikatów |

Azure AD Connect nie będzie mógł automatycznie wykrywa, że dzierżawy usługi Azure AD znajduje się w chmurze dla instytucji rządowych. Zamiast tego należy wykonać następujące czynności, po zainstalowaniu usługi Azure AD Connect.

1. Uruchom instalację Azure AD Connect.
2. Gdy pojawi się pierwszej strony, gdzie są powinien zaakceptować umowę licencyjną, nie należy kontynuować, ale pozostawić uruchomiony Kreator instalacji.
3. Uruchom regedit i zmienić wartość klucza rejestru `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` wartość `2`.
4. Przejdź do Kreatora instalacji usługi Azure AD Connect, należy zaakceptować umowę licencyjną i kontynuować. Podczas instalacji, upewnij się użyć **konfiguracji niestandardowej** ścieżki instalacji (i nie Instalacja ekspresowa). Następnie kontynuować instalację w zwykły sposób.

Aktualnie nie znajduje się w chmurze Microsoft Azure dla instytucji rządowych funkcje:

* **Azure AD Connect Health** nie jest dostępna.
* **Aktualizacje automatyczne** nie jest dostępna.
* **Zapisywanie zwrotne haseł** jest dostępna w wersji zapoznawczej wersji Azure AD Connect 1.1.570.0 i po.
* Inne usługi Azure AD Premium nie są dostępne.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat [integrowania tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).
