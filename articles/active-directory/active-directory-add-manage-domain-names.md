---
title: "Zarządzanie niestandardowych nazw domen w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Pojęcia dotyczące zarządzania i kwestie dotyczące zarządzania domeny niestandardowej w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 5ae19bb370064de96cf466ca09b13d02563d65a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Zarządzanie niestandardowych nazw domen w usłudze Azure Active Directory
Nazwa domeny może być identyfikator ważne w przypadku wielu zasobów katalogu w ramach:

* Użytkownik nazwy lub adresu e-mail użytkownika
* Adres grupy
* Identyfikator URI aplikacji dla aplikacji

Zasób w usłudze Azure Active Directory (Azure AD) może zawierać nazwy domeny, która została już zweryfikowana należeć do katalogu, który zawiera zasób. Tylko administrator globalny może wykonywać zadania zarządzania domeny w usłudze Azure AD.

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule. Jak zarządzać nazwy domeny w Centrum administracyjnym usługi Azure AD, zobacz [Zarządzanie niestandardowych nazw domen w usłudze Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Ustaw nazwę domeny głównej dla katalogu usługi Azure AD
Podczas tworzenia katalogu z początkowej nazwy domeny, takie jak "contoso.onmicrosoft.com", jest również nazwę domeny głównej dla katalogu. Domena podstawowa jest domyślna nazwa domeny dla nowego użytkownika, podczas tworzenia nowego użytkownika w [klasycznego portalu Azure](https://manage.windowsazure.com/), lub innych portalach, takich jak portalu administracyjnego usługi Office 365. Usprawnia to proces dla administratora utworzyć nowych użytkowników w portalu.

Aby zmienić nazwę domeny głównej dla katalogu:

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) przy użyciu konta użytkownika będącego administratorem globalnym katalogu usługi Azure AD.
2. Wybierz **usługi Active Directory** na pasku nawigacyjnym po lewej stronie.
3. Otwórz katalog.
4. Wybierz **domen** kartę.
5. Wybierz **zmiany podstawowego** przycisk paska poleceń.
6. Wybierz domenę, w której mają być nowej domeny głównej dla katalogu.

Można zmienić nazwę domeny głównej dla katalogu jako zweryfikowanej domeny niestandardowej, która nie jest zintegrowany. Zmiana domeny podstawowej dla katalogu nie spowoduje zmiany nazwy użytkownika dla wszystkich istniejących użytkowników.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Dodawanie niestandardowych nazw domen do usługi Azure AD
Maksymalnie 900 nazw domen niestandardowych można dodać do każdego katalogu usługi Azure AD. Proces [Dodawanie dodatkowej niestandardowej nazwy domeny](active-directory-add-domain.md) jest taki sam dla pierwszej nazwy domeny niestandardowej.

## <a name="add-subdomains-of-a-custom-domain"></a>Dodawanie poddomen domeny niestandardowej
Jeśli chcesz dodać nazwy domen trzeciego poziomu, takie jak "europe.contoso.com" do katalogu, należy najpierw dodać i zweryfikować domeny drugiego poziomu, np. contoso.com. Poddomeny zostanie automatycznie zweryfikowane przez usługę Azure AD. Aby wyświetlić zweryfikowaniu poddomeny, który właśnie został dodany, Odśwież stronę w przeglądarce, która zawiera listę domen w katalogu.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Co robić w przypadku zmiany nazwy domeny niestandardowej rejestratora DNS.
Jeśli zmienisz rejestratora DNS dla nazwy domeny niestandardowej, można korzystać z nazwy domeny niestandardowej z usługą Azure AD, sam bez przerw i dodatkowe zadania konfiguracyjne. Jeśli korzystasz z niestandardowej nazwy domeny z usługą Office 365, Intune lub innych usług, które opierają się na niestandardowych nazw domen w usłudze Azure AD, można znaleźć w dokumentacji tych usług.

## <a name="delete-a-custom-domain-name"></a>Usuń niestandardową nazwę domeny
Czy organizacja już używa tej nazwy domeny, czy należy użyć tej nazwy domeny z inną usługą Azure AD, możesz usunąć niestandardową nazwę domeny z usługi Azure AD.

Aby usunąć niestandardową nazwę domeny, musi najpierw upewnić, że żaden zasób w katalogu zależą od nazwy domeny. Nie można usunąć nazwy domeny z katalogu, jeśli:

* Każdy użytkownik ma nazwę użytkownika, adres e-mail lub adres serwera proxy, który obejmuje nazwę domeny.
* Każda grupa ma adres e-mail lub adres serwera proxy, która zawiera nazwę domeny.
* Aplikacje w usługi Azure AD ma identyfikator URI, który obejmuje nazwę domeny aplikacji.

Należy zmienić lub usunąć tych zasobów w katalogu usługi Azure AD, przed usunięciem nazwy domeny niestandardowej.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Użyj programu PowerShell lub interfejsu API programu Graph, zarządzanie nazwami domen
Większość zadań zarządzania dla nazwy domeny w usłudze Azure Active Directory można również przeprowadzić przy użyciu PowerShell firmy Microsoft lub programowo przy użyciu interfejsu API Azure AD Graph.

* [Zarządzanie nazwami domen w usłudze Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Zarządzanie nazwami domen w usłudze Azure AD przy użyciu interfejsu API programu Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Następne kroki
* [Więcej informacji na temat nazw domen w usłudze Azure AD](active-directory-add-domain-concepts.md)
* [Zarządzanie niestandardowymi nazwami domen](active-directory-add-manage-domain-names.md)

