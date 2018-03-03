---
title: "Architektura tożsamości stosu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o architekturze tożsamości używanych w usłudze Azure stosu."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/28/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 7f2ec78da38f3c97fde810fb8fc965cfbb6fda08
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Architektura tożsamości Azure stosu
Przed wybraniem dostawcy tożsamości do użycia z stosu Azure należy poznać istotnych różnic między opcjami usługi Azure Active Directory (Azure AD) i usługi Active Directory federacyjnych (AD FS). 

## <a name="capabilities-and-limitations"></a>Możliwości i ograniczenia 
Wybranego dostawcy tożsamości, można ograniczyć opcje, w tym obsługi wielu dzierżawców. 

  

|Możliwość lub scenariusza        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Połączenie z Internetem     |Yes       |Optional (Opcjonalność)|
|Obsługa wielu dzierżawców     |Yes       |Nie      |
|Syndykacja Marketplace       |Yes       |Tak — wymagane jest użycie [w trybie offline zespolonego Marketplace](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) narzędzia.|
|Obsługa usługi Active Directory Authentication Library (ADAL) |Yes |Yes|
|Obsługa narzędzi, takich jak Azure interfejsu wiersza polecenia (CLI), Visual Studio (VS) i programu PowerShell  |Yes |Yes|
|Tworzenie nazwy główne usług za pośrednictwem portalu     |Yes |Nie|
|Tworzenie nazwy główne usług certyfikatów      |Yes |Yes|
|Utwórz jednostki usługi z kluczy tajnych (klucze)    |Yes |Nie|
|Aplikacje mogą używać usługi wykresu           |Yes |Nie|
|Aplikacje mogą używać dostawcy tożsamości dla logowania |Yes |Tak — wymaga aplikacji możliwości utworzenia federacji z lokalnej usługi AD FS. |

## <a name="topologies"></a>Topologie
Poniższe sekcje zawierają informacje o topologii tożsamości, których można użyć.

### <a name="azure-ad--single-tenant"></a>Usługi Azure AD — pojedynczej dzierżawy 
Domyślnie po zainstalowaniu stosu Azure i przy użyciu usługi Azure AD, Azure stosu używa topologii pojedynczej dzierżawy. 

Topologia pojedynczej dzierżawy jest przydatna, gdy:
- Wszyscy użytkownicy są częścią tej samej dzierżawy.
- Dostawcy usług znajduje się wystąpienie stosu Azure dla organizacji.  

![Azure topologii stosu przy użyciu topologii pojedynczej dzierżawy w usłudze Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Z tej topologii:
- Azure stosu rejestruje wszystkie aplikacje i usługi do tej samej usługi Azure AD dzierżawy katalogu. 
- Azure stosu uwierzytelnia tylko użytkownicy i aplikacje z tego katalogu, w tym tokenów. 
- Tożsamości dla dzierżawy użytkowników i administratorów (operatorom chmury) znajdują się w tej samej dzierżawy katalogu. 
- Aby umożliwić użytkownikowi dostęp do tego środowiska Azure stosu z innego katalogu, należy [zaprosić użytkowników jako gość](azure-stack-identity-overview.md#guest-users) do katalogu dzierżawcy.  

### <a name="azure-ad--multi-tenant"></a>Azure AD — wieloma dzierżawcami
Operatorzy chmury można skonfigurować stosu Azure, aby zezwolić na dostęp do aplikacji przez dzierżawców z jedną lub kilkoma organizacjami. Użytkownicy uzyskują dostęp do aplikacji za pośrednictwem portalu użytkownika. Portal administracyjny (wykorzystywane przez operatorowi chmury) w tej konfiguracji jest ograniczony do użytkowników z jednego katalogu. 

Topologia wielodostępne jest przydatna, gdy:
- Dostawcy usług chce, aby użytkownicy uzyskują dostęp stosu Azure z wielu organizacji.

![Azure topologii stosu z usługą Azure AD przy użyciu topologii z wieloma dzierżawcami](media/azure-stack-identity-architecture/multi-tenant.png)

Z tej topologii:
- Dostęp do zasobów powinna znajdować się na zasadzie dla organizacji. 
- Użytkownicy z organizacji, co nie może być może przyznać dostęp do zasobów dla użytkowników spoza organizacji.  
- Tożsamości dla administratorów (operatorom chmury) może być w oddzielnym katalogu dzierżawcy niż tożsamości użytkowników. Ta separacja zapewnia izolację konta na poziomie dostawcy tożsamości. 
 
### <a name="ad-fs"></a>AD FS  
Topologia usług AD FS, jest wymagany, gdy spełniony jest jeden z następujących warunków:
- Azure stosu nie połączyć się z Internetem.
- Stos Azure można połączyć się z Internetem, ale użytkownik chce używać usług AD FS dla dostawcy tożsamości.
  
![Azure topologię stosu za pomocą usług AD FS](media/azure-stack-identity-architecture/adfs.png)

Z tej topologii:
- Do obsługi użycia w środowisku produkcyjnym, wbudowane wystąpienia usług AD FS stosu Azure należy zintegrować z istniejącym wystąpieniem usług AD FS kopii przez Active Directory (AD), za pomocą zaufania federacyjnego. 
- Usługa wykresu w stosie Azure można zintegrować z istniejącej usługi AD.  Można również użyć OData na podstawie usługi interfejsu API programu Graph, który obsługuje interfejsy API, które są zgodne z interfejsu API programu Azure AD Graph.  

  Wchodzić w interakcje z usługi AD, interfejsu API programu Graph wymaga poświadczeń użytkownika z usługi AD, które ma uprawnienia tylko do odczytu usługi AD. 
  - Wbudowane usługi AD FS jest oparta na Server 2016. 
  - Z usług AD FS i AD musi być oparta na Server 2012 lub nowszym.  
  
  Między AD a wbudowanych usług AD FS interakcje nie są ograniczone do protokołu OpenID Connect i można użyć dowolnego wzajemnie obsługiwanych protokołów.  
  - Konta użytkowników są tworzone i zarządzane w lokalnej usługi AD.
  - Nazwy główne usług i rejestracji dla aplikacji są zarządzane w usłudze AD wbudowanych.



## <a name="next-steps"></a>Kolejne kroki
- [Omówienie tożsamości](azure-stack-identity-overview.md)   
- [Integracja Datacenter - tożsamości](azure-stack-integrate-identity.md)