---
title: "Znajdowanie aplikacji w chmurze niezarządzane z usługi Cloud App Discovery w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Zawiera informacje o znajdowanie aplikacji i zarządzanie nimi z Cloud App Discovery, jakie są zalety i jak działa."
services: active-directory
keywords: "Usługa cloud app discovery, zarządzanie aplikacjami"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1d0ad06fc7eec07f8e1e0ba47121b6eec01c87df
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Znajdowanie aplikacji w chmurze niezarządzane z usługi Cloud App Discovery
## <a name="summary"></a>Podsumowanie

Usługa cloud App Discovery w usłudze Azure Active Directory teraz zapewnia rozszerzoną odnajdywania bez wykorzystania agentów obsługiwane przez usługę Microsoft Cloud App Security. Aby użyć Cloud App Discovery, po prostu Zaloguj się przy użyciu poświadczeń usługi Azure AD Premium P1. Ta aktualizacja jest dostępna bez ponoszenia dodatkowych kosztów dla wszystkich klientów usługi Azure AD Premium P1. Rozpoczynanie pracy z artykułem [Konfigurowanie Cloud App Discovery w usłudze Azure AD](https://docs.microsoft.com/azure/active-directory/cloudappdiscovery-get-started), następnie wypróbowanie [Microsoft Cloud App Security](https://portal.cloudappsecurity.com/).

> [!IMPORTANT] 
> Usunąć bieżący Azure AD Cloud App Discovery środowisko z odnajdowania opartego na agenta jest można wyłączyć na 5 marca 2018, po którym agentów zostanie wyłączone, a dane. Wykonaj akcję przed marca 5. Aby rozpocząć pracę na nowe środowisko w celu uniknięcia przerw w świadczeniu usługi.  
 
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


## <a name="next-steps"></a>Kolejne kroki
* [Cloud App Discovery zabezpieczeń i zagadnienia dotyczące ochrony prywatności](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Ustawienia funkcji cloud Discovery aplikacji rejestru dotyczące serwerów Proxy z portami niestandardowe](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Wykaz cloud App Discovery agenta zmian](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

