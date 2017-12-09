---
title: "Azure Active Directory Domain Services: Włącz ograniczone delegowanie protokołu kerberos | Dokumentacja firmy Microsoft"
description: "Włącz ograniczone delegowanie protokołu kerberos w domenach zarządzanych usług domenowych Azure Active Directory"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: maheshu
ms.openlocfilehash: f5ecb8b2afc0c05fb8b2cb3850adcd2510ca29d2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Skonfiguruj ograniczone delegowanie protokołu Kerberos (KCD) do domeny zarządzanej
Wiele aplikacji muszą uzyskać dostęp do zasobów w kontekście użytkownika. Usługa Active Directory obsługuje mechanizm delegowania protokołu Kerberos, co umożliwia przypadek użycia wywołuje. Ponadto można ograniczyć delegowania tak, aby tylko określonych zasobów można uzyskać w kontekście użytkownika. Azure domen zarządzanych w usługach domenowych AD różnią się od tradycyjnych domen usługi Active Directory, ponieważ są one bardziej bezpieczny sposób zablokowana.

W tym artykule przedstawiono sposób konfigurowania ograniczonego delegowania protokołu Kerberos w domenie zarządzanej usług domenowych Azure AD.

## <a name="kerberos-constrained-delegation-kcd"></a>Ograniczone delegowanie protokołu Kerberos (KCD)
Delegowanie protokołu Kerberos umożliwia konta personifikować innego podmiotu zabezpieczeń (na przykład użytkownik) dostęp do zasobów. Należy wziąć pod uwagę aplikacji sieci web, który uzyskuje dostęp do sieci web zaplecza interfejsu API w kontekście użytkownika. W tym przykładzie użytkownik personifikuje aplikacji sieci web (uruchomione w kontekście konta usługi lub konto komputera/urządzenia), podczas uzyskiwania dostępu do zasobów (zaplecza interfejsu API sieci web). Delegowanie protokołu Kerberos jest niebezpieczne, ponieważ nie ogranicza zasobów, których dostęp konta personifikacji w kontekście użytkownika.

Ograniczone delegowanie protokołu Kerberos (KCD) ogranicza usługi/zasoby, do których określony serwer może działać w imieniu użytkownika. Tradycyjny KCD wymaga uprawnień administratora domeny, aby skonfigurować konto domeny dla usługi i ogranicza konta do jednej domeny.

Tradycyjny KCD ma również kilka problemów związanych z nim. W starszych systemach operacyjnych Jeśli administrator domeny konfigurował KCD na podstawie konta usługi, administrator usług nie miał możliwości wiedzieć, które usługi frontonu delegowane do usług zasobów są jego własnością. A usługa frontonu, którą mógł delegować do usługi zasobów potencjalny punkt ataku. Jeśli takie naruszenie nastąpi naruszenie serwera udostępniającego usługę frontonu i był on skonfigurowany do delegowania usług zasobów, zasobów usługi również mogły zostać naruszone.

> [!NOTE]
> W domenie zarządzanej usług domenowych Azure AD nie masz uprawnienia administratora domeny. W związku z tym **KCD tradycyjnych nie można skonfigurować w domenie zarządzanej**. Użyj KCD oparte na zasobach, zgodnie z opisem w tym artykule. Ten mechanizm również jest bardziej bezpieczne.
>
>

## <a name="resource-based-kcd"></a>Oparte na zasobach KCD
Z systemu Windows Server 2012 lub nowszym administratorów usługi uzyskać możliwość konfigurowania ograniczonego delegowania dla usługi. W tym modelu administrator usług zaplecza można akceptować lub odrzucać określonych usług frontonu z za pomocą KCD. Ten model jest nazywany **oparte na zasobach KCD**.

Oparte na zasobach KCD została skonfigurowana przy użyciu programu PowerShell. Możesz użyć `Set-ADComputer` lub `Set-ADUser` poleceń cmdlet, w zależności od tego, czy konto personifikacji jest konto komputera lub konto serwisu konta użytkownika.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Skonfiguruj oparte na zasobach KCD dla konta komputera do domeny zarządzanej
Założono, że masz aplikację sieci web uruchomionej na komputerze "contoso100-webapp.contoso100.com". Wymaga dostępu do zasobu (interfejs API sieci web uruchomiony na "contoso100-api.contoso100.com") w kontekście użytkownicy domeny. Oto, jak należy skonfigurować oparte na zasobach KCD dla tego scenariusza.

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Skonfiguruj oparte na zasobach KCD dla konta użytkownika w domenie zarządzanej
Założono, że aplikacja sieci web uruchomiona jako konto usługi "appsvc" i musi on mieć dostęp do zasobu (interfejsu API sieci web uruchomiona jako konto usługi - "backendsvc") w kontekście użytkowników domeny. Oto, jak należy skonfigurować oparte na zasobach KCD dla tego scenariusza.

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Powiązana zawartość
* [Usługi domenowe AD Azure - Przewodnik wprowadzający](active-directory-ds-getting-started.md)
* [Omówienie delegowania ograniczonego protokołu Kerberos](https://technet.microsoft.com/library/jj553400.aspx)
