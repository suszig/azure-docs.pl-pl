---
title: "Usług domenowych Azure Active Directory: Funkcje | Dokumentacja firmy Microsoft"
description: "Funkcje usług domenowych w usłudze Azure Active Directory"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 8005be7ded6ea005af086aeaf594963a5f2d4ac2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Funkcje
Następujące funkcje są dostępne w domenach zarządzanych usług domenowych Azure AD.

* **Środowisko proste wdrożenie:** można włączyć usługi domenowe Azure AD dla dzierżawy usługi Azure AD za pomocą kilku kliknięć. Niezależnie od tego, czy dzierżawy usługi Azure AD jest dzierżawą chmury lub synchronizacji z katalogiem lokalnym można szybko za domeny zarządzanej.
* **Obsługa przyłączenie do domeny:** możesz z łatwością przyłączenie do domeny komputerów w Twojej domeny zarządzanej, jest dostępny w sieci wirtualnej platformy Azure. Środowisko przyłączenie do domeny, na kliencie systemu Windows i działa systemy operacyjne serwera bezproblemowo względem domeny obsługiwanych przez usługi domenowe Azure AD. Można również użyć zautomatyzowane sprzężenie narzędzi względem tych domen.
* **Wystąpienia jednej domeny na katalog usługi Azure AD:** można utworzyć pojedynczej domeny usługi Active Directory dla każdego katalogu usługi Azure AD.
* **Tworzenia domen z niestandardowych nazw:** można utworzyć domeny niestandardowej nazwy (na przykład "contoso100.com") przy użyciu usług domenowych Azure AD. Można używać nazw albo zweryfikowane i niezweryfikowane domeny. Opcjonalnie można również utworzyć domenę z sufiksem domeny wbudowanych (to znaczy "*. onmicrosoft.com") oferowane przez katalog usługi Azure AD.
* **Zintegrowane z usługą Azure AD:** jest konieczne konfigurowanie lub zarządzać replikacją usług domenowych Azure AD. Konta użytkowników, członkostwa w grupach i poświadczeń użytkownika (hasła) z katalogiem Azure AD są automatycznie dostępne w usługach domenowych Azure AD. Nowi użytkownicy, grupy lub zmiany atrybutów z dzierżawy usługi Azure AD lub lokalnego katalogu są synchronizowane automatycznie do usług domenowych Azure AD.
* **Uwierzytelnianie NTLM i Kerberos:** o obsługę uwierzytelniania NTLM i Kerberos, można wdrożyć aplikacji wykorzystujących zintegrowane uwierzytelnianie systemu Windows.
* **Użyj poświadczeń firmowych/hasła:** haseł dla użytkowników usługi Azure AD dzierżawcy korzystają z usług domenowych Azure AD. Użytkownicy mogą Użyj swoich poświadczeń firmowych do przyłączania do domeny komputerów, logowania interakcyjnego lub za pośrednictwem pulpitu zdalnego i uwierzytelniania względem domeny zarządzanej.
* **Wiązanie LDAP & LDAP odczytu pomocy technicznej:** mogą używać aplikacji, które opierają się na wiązania LDAP do uwierzytelniania użytkowników w domenach obsługiwanych przez usługi domenowe Azure AD. Ponadto aplikacje, które umożliwia atrybuty użytkownika i komputera kwerendy LDAP operacji odczytu z katalogu może również współpracować z usługami domenowymi Azure AD.
* **Bezpieczny protokół LDAP (LDAPS):** można włączyć dostęp do katalogu za pośrednictwem bezpiecznego protokołu LDAP (LDAPS). Bezpieczny dostęp LDAP jest dostępny w sieci wirtualnej domyślnie. Można jednak również włączyć bezpieczny dostęp LDAP, za pośrednictwem Internetu.
* **Zasady grupy:** pojedynczy wbudowany GPO można użyć dla użytkowników i komputerów kontenerów, aby wymuszał zgodność z wymaganych zasad zabezpieczeń dla kont użytkowników i komputerów przyłączonych do domeny. Można także tworzenie własnych niestandardowych obiektów zasad grupy i przypisać je do niestandardowych jednostek organizacyjnych [Zarządzanie zasadami grupy](active-directory-ds-admin-guide-administer-group-policy.md).
* **Zarządzanie DNS:** członków grupy "Administratorzy kontrolera domeny usługi AAD" można zarządzać DNS dla domeny zarządzanej przy użyciu znanych narzędzi administracyjnych DNS, takie jak przystawka programu MMC administracji DNS.
* **Tworzenie niestandardowych jednostek organizacyjnych (OU):** członków grupy "Administratorzy usługi AAD kontrolera domeny" można tworzyć niestandardowe jednostek organizacyjnych w domenie zarządzanej. Te użytkownicy mają prawo pełne uprawnienia administracyjne za pośrednictwem niestandardowych jednostek organizacyjnych, więc ich można usunąć konta usług, komputerów, grup itp. w ramach tych niestandardowych jednostek organizacyjnych.
* **Dostępne w wielu regionach platformy Azure:** zobacz [usług Azure według regionu](https://azure.microsoft.com/regions/#services/) stronę, aby wiedzieć, regiony platformy Azure, w których są dostępne usługi domenowe Azure AD.
* **Wysoka dostępność:** usługi domenowe Azure AD oferują wysoką dostępność domeny. Ta funkcja oferuje gwarancji wyższej czas działania usługi i odporność na awarie. Monitorowania oferuje wbudowane kondycji automatycznego korygowania z błędami przez Obracająca się nowych wystąpień, aby zastąpić wystąpień nie powiodło się, a także aby zapewnić ciągłość usługi dla domeny.
* **Użyj narzędzi zarządzania znanych:** znanych narzędzi zarządzania usługi Active Directory systemu Windows Server, takich jak Centrum administracyjne usługi Active Directory lub środowiska PowerShell usługi Active Directory służy do administrowania domen zarządzanych.
