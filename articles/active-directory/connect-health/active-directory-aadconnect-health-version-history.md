---
title: Historia wersji programu Azure AD Connect Health
description: "Ten dokument zawiera opis wydanie dla usługi Azure AD Connect Health i dołączył w tych wersjach."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6c990a184d44771c78330f54f518bb4c35a36a35
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: historia wersji
Zespół usługi Azure Active Directory regularnie aktualizuje Azure AD Connect Health z nowych funkcji. W tym artykule wymieniono wersje i funkcje, które zostały wydane.

## <a name="october-2016"></a>Października 2016
**Aktualizacja agenta:**

* Agent Azure AD Connect Health dla usług AD FS \(wersji 2.6.408.0\)
  1. Ulepszenia w wykrywaniu adresów IP klientów w żądania uwierzytelniania
  2. Poprawki błędów związane z alertami
* Agent Azure AD Connect Health dla usług AD DS (wersja 2.6.408.0)
  1. Poprawki błędów związane z alertami.
* Agent Azure AD Connect Health zwolnione z programem Azure AD Connect wersji 1.1.281.0 synchronizacji (wersja 2.6.353.0)
  1. Podaj wymagane dane raportów o błędach synchronizacji
  2. Poprawki błędów związane z alertami

**Nowe funkcje w wersji zapoznawczej:**

* Raporty o błędach synchronizacji dla usługi Azure AD Connect

**Nowe funkcje:**

* Azure AD Connect Health dla usług AD FS — pole adresu IP jest dostępna w raporcie o najwyższym 50 użytkowników z zły nazwy użytkownika i hasła.

## <a name="july-2016"></a>Lipca 2016 r.
**Nowe funkcje w wersji zapoznawczej:**

* [Azure AD Connect Health dla usług AD DS](active-directory-aadconnect-health-adds.md).

## <a name="january-2016"></a>Styczeń 2016
**Aktualizacja agenta:**

* Agent Azure AD Connect Health dla usług AD FS (wersja 2.6.91.1512)

**Nowe funkcje:**

* [Narzędzie Test łączności dla usługi Azure AD Connect kondycji agentów](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Listopad 2015
**Nowe funkcje:**

* Obsługa [kontrola dostępu oparta na rolach](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)

**Nowe funkcje w wersji zapoznawczej:**

* [Azure AD Connect Health dla synchronizacji](active-directory-aadconnect-health-sync.md).

**Rozwiązane problemy:**

* Wprowadzono poprawki błędów podczas rejestracji agenta.

## <a name="september-2015"></a>Wrzesień 2015
**Nowe funkcje:**

* Niewłaściwa nazwy użytkownika hasło raportów dla usług AD FS
* Pomocy technicznej, aby skonfigurować serwer Proxy HTTP nieuwierzytelniony
* Pomocy technicznej w celu skonfigurowania agenta w instalacji Server core
* Ulepszenia alerty dla usług AD FS
* Przekaż ulepszenia Azure AD Connect Health Agent dla usług AD FS dla danych i łączności.

**Rozwiązane problemy:**

* Poprawki błędów w szczegółowe informacje dotyczące użycia dla typów AD FS błędów.

## <a name="june-2015"></a>Czerwiec 2015
**Początkowa wersja programu Azure AD Connect Health dla usług AD FS i serwera Proxy usług AD FS.**

**Nowe funkcje:**

* Alerty dotyczące monitorowania serwerów usług AD FS i serwera Proxy usług AD FS z powiadomienia e-mail.
* Łatwy dostęp do topologii usług AD FS i wzorce w liczniki wydajności usługi AD FS.
* Trend w pomyślnych żądań tokenów na serwerach usług AD FS pogrupowane według aplikacje, metody uwierzytelniania, żądania sieci lokalizacji itp.
* Trendy w żądaniu nie powiodło się na serwerach usług AD FS pogrupowane według aplikacji, błąd typy itp.
* Łatwiejsze wdrażanie agenta przy użyciu poświadczeń administratora globalnego usługi Azure AD.  

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [monitorowanie lokalnej tożsamości infrastruktury i synchronizacji usług w chmurze](active-directory-aadconnect-health.md).

