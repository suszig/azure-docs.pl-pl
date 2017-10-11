---
title: "Poglądowe omówienie niestandardowych nazw domen w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Wyjaśniono koncepcyjnej ramach przy użyciu niestandardowych nazw domen w usłudze Azure Active directory, w tym federacyjnych dla rejestracji jednokrotnej"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fd0c5def-0da2-43af-81bc-76f4cfe86afd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 3c591680160101a91174868714392674c9aa7178
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Poglądowe omówienie niestandardowych nazw domen w usłudze Azure Active Directory
Nazwa domeny może być identyfikator ważne w przypadku wielu zasobów katalogu w ramach:

* Użytkownik nazwy lub adresu e-mail użytkownika
* Adres grupy
* Identyfikator URI aplikacji dla aplikacji

Zasób w usłudze Azure Active Directory (Azure AD) może zawierać nazwy domeny, która została już zweryfikowana należeć do katalogu, który zawiera zasób. Tylko administrator globalny może wykonywać zadania zarządzania domeny w usłudze Azure AD.

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule. Jak zarządzać nazwy domeny w Centrum administracyjnym usługi Azure AD, zobacz [Zarządzanie niestandardowych nazw domen w usłudze Azure Active Directory](active-directory-domains-manage-azure-portal.md).

Nazwy domen w usłudze Azure AD są globalnie unikatowe. Niestandardowej nazwy domeny mogą posłużyć tylko jeden dzierżawy usługi Azure AD w czasie. Jeśli jednego katalogu usługi Azure AD ma zweryfikować nazwę domeny, nie katalogu usługi Azure AD można sprawdzić lub użyć tej samej nazwy domeny.

## <a name="initial-and-custom-domain-names"></a>Nazwy domen początkowej i niestandardowych
Nazwa domeny, co w usłudze Azure AD jest początkową nazwę domeny lub niestandardowej nazwy domeny.

Co usługi Azure AD jest dostarczany z początkową nazwę domeny w postaci contoso.onmicrosoft.com. Ta nazwa trzeciego poziomu domeny, w tym przykładzie "contoso.onmicrosoft.com", została włączona podczas tworzenia katalogu, zazwyczaj przez administratora, który utworzył katalog. Z początkowej nazwy domeny dla katalogu nie można zmienić ani usunąć. Z początkowej nazwy domeny, podczas funkcjonalnej, jest przeznaczony głównie w celu można użyć jako mechanizm bootstrapping, dopóki nie zostanie zweryfikowana niestandardowej nazwy domeny.

W większości środowisk produkcyjnych katalog ma co najmniej jeden zweryfikowanej domeny niestandardowe, takie jak "contoso.com" i jest tej domeny niestandardowej, która jest widoczna dla użytkowników końcowych. Niestandardowej nazwy domeny jest nazwą domeny, jest własnością i używane przez tę organizację, takich jak "contoso.com", do celów, takich jak hostingu swojej witryny sieci web. Ta nazwa domeny jest znanych do pracowników, ponieważ jest ona częścią nazwy użytkownika używanego do logowania do sieci firmowej, lub aby wysyłanie i pobieranie wiadomości e-mail.

Przed użyciem przez usługę Azure AD, niestandardowa nazwa domeny musi dodane do katalogu i weryfikacji.

## <a name="verified-and-unverified-domain-names"></a>Nazwy domeny zweryfikowane i niezweryfikowane
Z początkowej nazwy domeny dla katalogu niejawnie jest oceniana jako zweryfikowane przez usługę Azure AD. Gdy administrator dodaje niestandardową nazwę domeny do usługi Azure AD, jest początkowo w stanie niezweryfikowane. Usługi Azure AD nie zezwala na wszystkie zasoby katalogu użyć nazwy niezweryfikowanej domeny. Dzięki temu tylko jeden katalog można użyć nazwy określonej domeny, czy rzeczywiście czy organizacja używa nazwy domeny jest właścicielem tej nazwy domeny.

Usługi Azure AD sprawdza własność nazwy domeny, wyszukując określonego wpisu w pliku strefy domeny name service (DNS) dla nazwy domeny. Aby potwierdzić własność nazwy domeny, administrator pobiera wpis DNS z usługi Azure AD, Azure AD będzie szukać i dodaje ten wpis do pliku strefy DNS dla nazwy domeny. Plik strefy DNS obsługiwanego przez rejestratora nazw domen dla tej domeny. Kroki, aby zweryfikować domeny są wyświetlane w artykule, aby [Dodawanie niestandardową domenę do katalogu usługi Azure AD](active-directory-add-domain.md).

Dodanie wpisu DNS do pliku strefy dla nazwy domeny nie ma wpływu na innych usług domenowych, takich jak adres e-mail lub Usługa hostingu sieci web.

## <a name="federated-and-managed-domain-names"></a>Nazwy domen federacyjnych i zarządzanie nimi
Umożliwić użytkownikom logowania federacyjnego środowisko między lokalnymi usługi Active Directory i Azure AD można skonfigurować niestandardową nazwę domeny w usłudze Azure AD. Konfigurowanie domeny dla Federacji wymaga aktualizacji zasobów uprzywilejowanych w usłudze Azure AD, a także usługi systemu Windows Server Active Directory. Konfigurowanie domeny federacyjnej musi zostać ukończone z usługi Azure AD Connect lub przy użyciu programu PowerShell. Nie można zainicjować federowania domeny niestandardowej z klasycznego portalu Azure. [Obejrzyj ten film, aby dowiedzieć się więcej na temat konfigurowania usług AD FS dla użytkownika logują się przy użyciu usługi Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domeny, które nie są federacyjnych są nazywane domen zarządzanych. Domena początkowa dla katalogu usługi Azure AD niejawnie jest szacowana jako domeny zarządzanej.

## <a name="primary-domain-names"></a>Nazwy domeny głównej
Nazwa domeny głównej dla katalogu jest nazwa domeny, który jest wstępnie wybrane jako wartość domyślną dla części "domeny" Nazwa użytkownika, gdy administrator tworzy nowego użytkownika w [portalu Azure](https://portal.azure.com/), lub innego portalu, takich jak portalu administracyjnego usługi Office 365 lub portalu Microsoft Intune. Katalog może mieć tylko jedną nazwę domeny głównej. Administrator może zmienić nazwy domeny podstawowej jako żadnego weryfikacji domeny niestandardowej, która nie jest zintegrowany lub do domeny początkowej.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Nazwy domen w usłudze Azure AD i innych usług Online firmy Microsoft
Należy zweryfikować nazwę domeny w usłudze Azure AD, zanim będzie można użyć innej usługi Online firmy Microsoft, takich jak Exchange Online, SharePoint Online i Intune. Inne usługi zwykle wymagają administratorem, aby dodać jeden lub więcej wpisów DNS dla usługi.

Aplikacji sieci web platformy Azure używa mechanizm, aby zweryfikować prawo własności do domeny. Należy zweryfikować domeny do użycia z usługą Azure AD, nawet wtedy, gdy wcześniej weryfikacji do użycia przez aplikację sieci web platformy Azure w ramach subskrypcji, która zależy od tej usługi Azure AD. Aplikacja sieci web platformy Azure można użyć nazwy domeny, która została zweryfikowana w innym katalogu z katalogu, który zabezpiecza aplikacji sieci web.

## <a name="managing-domain-names"></a>Zarządzanie nazwami domen
Można wykonać zadania zarządzania w domenie z klasycznego portalu Azure i programu PowerShell. Wiele zadań można wykonać przy użyciu interfejsu API Azure AD Graph.

* [Dodawanie i weryfikowanie niestandardowej nazwy domeny](active-directory-add-domain.md)
* [Zarządzanie domenami w klasycznym portalu Azure](active-directory-add-manage-domain-names.md)
* [Zarządzanie nazwami domen w usłudze Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Zarządzanie nazwami domen w usłudze Azure AD przy użyciu interfejsu API programu Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

