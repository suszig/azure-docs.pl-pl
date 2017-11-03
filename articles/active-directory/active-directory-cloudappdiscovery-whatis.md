---
title: "Znajdowanie aplikacji w chmurze niezarządzane z usługi Cloud App Discovery w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Zawiera informacje o znajdowanie aplikacji i zarządzanie nimi z Cloud App Discovery, jakie są zalety i jak działa."
services: active-directory
keywords: "Usługa cloud app discovery, zarządzanie aplikacjami"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 35b898aa3c03aeef914a7df574ac65a22a6c7bec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Znajdowanie aplikacji w chmurze niezarządzane z usługi Cloud App Discovery
## <a name="summary"></a>Podsumowanie

Usługa cloud App Discovery to funkcja usługi Azure Active Directory Premium, która umożliwia odnalezienie aplikacji w chmurze niezarządzane używane przez osoby z Twojej organizacji. W przedsiębiorstwach nowoczesnego działu IT często nie są znane wszystkich aplikacji korzystających z członków organizacji ich w pracy w chmurze. To proste zobaczyć, dlaczego Administratorzy byłyby problemów dotyczących nieautoryzowanego dostępu do danych firmowych, wycieku danych i inne zagrożenia dla bezpieczeństwa. To Brak świadomości ułatwia tworzenie planu zajmowanie się te zagrożenia bezpieczeństwa wydawać się czasochłonnym zadaniem.

> [!TIP] 
> Zapoznaj się z ulepszenia Cloud App Discovery w usłudze Azure Active Directory (Azure AD), która rozszerzono przez [Integracja z usługą Microsoft Cloud App Security](https://portal.cloudappsecurity.com).

**Usługa Cloud App Discovery można:**

* Znajdź używane aplikacje w chmurze i pomiar wykorzystanie przez liczbę użytkowników, ilości ruchu lub liczbę żądań sieci web do aplikacji.
* Określenie użytkowników, którzy korzystają z aplikacji.
* Eksportowanie danych do analizy w trybie offline.
* Przenoszenie tych aplikacji pod kontrolą IT i Włącz logowanie jednokrotne do zarządzania użytkownikami.

## <a name="how-it-works"></a>Jak to działa
1. Aplikacja użycia agenci są zainstalowani na komputerach użytkowników.
2. Informacje o użyciu aplikacji przechwycone przez agentów są wysyłane za pośrednictwem kanału bezpieczne, zaszyfrowane do usługi odnajdywania aplikacji w chmurze.
3. Usługa Cloud App Discovery ocenia dane i generuje raporty.

![Diagram odnajdywania aplikacji w chmurze](./media/active-directory-cloudappdiscovery/cad01.png)


## <a name="next-steps"></a>Następne kroki
* [Cloud App Discovery zabezpieczeń i zagadnienia dotyczące ochrony prywatności](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Ustawienia funkcji cloud Discovery aplikacji rejestru dotyczące serwerów Proxy z portami niestandardowe](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Wykaz cloud App Discovery agenta zmian](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

