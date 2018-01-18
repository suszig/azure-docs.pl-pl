---
title: "Synchronizacja programu Azure AD Connect: rozszerzenia katalogów | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano funkcję rozszerzeń katalogu w programie Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 31758568f7ce916a4c242aad743bb4b0cb9b2d6e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Synchronizacja programu Azure AD Connect: rozszerzenia katalogów
Rozszerzenia katalogów umożliwia rozszerzanie schematu w usłudze Azure AD przy użyciu własnego atrybutów z lokalnej usługi Active Directory. Ta funkcja umożliwia tworzenie aplikacji biznesowych, korzystanie z atrybutów, można nadal zarządzać lokalnymi. Te atrybuty mogą być używane za pośrednictwem [rozszerzeń katalogu Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) lub [Microsoft Graph](https://graph.microsoft.io/). Widać atrybuty dostępne przy użyciu [explorer Azure AD Graph](https://graphexplorer.azurewebsites.net/) i [Eksplorator Microsoft Graph](https://developer.microsoft.com/en-us/graph/graph-explorer) odpowiednio.

Obecnie nie obciążenie usługi Office 365 wykorzystuje te atrybuty.

Możesz skonfigurować dodatkowych atrybutów, które chcesz synchronizować w ścieżce ustawienia niestandardowe w Kreatorze instalacji.
![Kreator rozszerzenia schematu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
Instalacja zawiera następujące atrybuty, które są kandydatami prawidłowy:

* Typy obiektów użytkowników i grup
* Atrybuty jednowartościowe: String, Boolean, Integer, dane binarne
* Atrybuty wielowartościowe: ciąg, dane binarne


>[!NOTE]
> Azure AD Connect obsługuje synchronizowanie wielowartościowe atrybutów AD do usługi Azure AD jako rozszerzenia katalogów wielowartościowe, obecnie nie są żadne funkcje w usłudze Azure AD, które obsługują rozszerzenia katalogów wielowartościowe.

Lista atrybutów jest do odczytu z pamięci podręcznej schematu, utworzony podczas instalacji programu Azure AD Connect. Jeśli rozszerzono schemat usługi Active Directory przy użyciu dodatkowych atrybutów, a następnie [schematu musi zostać odświeżona](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) przed te nowe atrybuty są widoczne.

Obiektu w usłudze Azure AD mogą mieć maksymalnie 100 atrybuty rozszerzenia katalogu. Maksymalna długość wynosi 250 znaków. Jeśli wartość atrybutu jest dłuższy, następnie zostanie obcięta przez aparat synchronizacji.

Podczas instalacji programu Azure AD Connect aplikacja jest zarejestrowany, których te atrybuty są dostępne. Można wyświetlić tę aplikację w portalu Azure.  
![Aplikacja rozszerzenia schematu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Te atrybuty są teraz dostępne za pośrednictwem wykresu:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Atrybuty są poprzedzane prefiksem rozszerzenia\_{AppClientId}\_. AppClientId ma taką samą wartość dla wszystkich atrybutów w dzierżawie usługi Azure AD.

## <a name="next-steps"></a>Kolejne kroki
Dowiedz się więcej o [synchronizacja programu Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej na temat [integrowania tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).
