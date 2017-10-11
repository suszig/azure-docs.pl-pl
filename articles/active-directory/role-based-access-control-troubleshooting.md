---
title: "Rozwiązywanie problemów z Azure RBAC | Dokumentacja firmy Microsoft"
description: "Uzyskaj pomoc dotyczącą problemy lub pytania dotyczące zasobów kontroli dostępu opartej na rolach."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Oparta na rolach kontrola dostępu do rozwiązywania problemów

W tym artykule dokumentu odpowiedzi na często zadawane pytania dotyczące określone prawa dostępu przyznane z rolami, aby wiedzieli, czego można oczekiwać, korzystając z ról w portalu Azure i może rozwiązać problemy z dostępem do. Te trzy role opisano wszystkie typy zasobów:

* Właściciel  
* Współautor  
* Czytelnik  

Właściciele i współautorzy mają pełny dostęp do możliwości zarządzania, ale współautora nie może udzielić dostępu do innych użytkowników lub grup. Elementy poznasz nieco bardziej interesującego z rolą czytnika, dzięki czemu to, gdzie będzie poświęcić trochę czasu. Zobacz [opartej na rolach kontrola dostępu get-started artykułu](role-based-access-control-configure.md) szczegółowe informacje na temat sposobu udzielić dostępu.

## <a name="app-service-workloads"></a>Obciążeń usługi aplikacji
### <a name="write-access-capabilities"></a>Możliwości zapisu
Przyznanie dostępu użytkowników tylko do odczytu w aplikacji sieci web jednej, niektóre funkcje zostały wyłączone, że nie może spodziewać się. Następujące funkcje zarządzania wymagają **zapisu** dostęp do aplikacji sieci web (współautora lub właściciela), a nie są dostępne w jakimkolwiek scenariuszu tylko do odczytu.

* Polecenia (na przykład Uruchom, Zatrzymaj, itp.)
* Zmiana ustawień, takich jak konfiguracja ogólna, ustawienia skalowania ustawienia kopii zapasowej i ustawienia monitorowania
* Uzyskiwanie dostępu do poświadczeń publikowania i innych informacji poufnych, takich jak ustawienia aplikacji i parametry połączenia
* Dzienniki przesyłania strumieniowego
* Dzienniki diagnostyczne konfiguracji
* W konsoli (wiersza polecenia)
* Ostatnie i Active wdrożenia (ciągłe wdrażanie lokalnej git)
* Szacowane wydatki
* Testy sieci Web
* Sieć wirtualna (widoczne tylko dla czytnika, jeśli wcześniej skonfigurowano sieci wirtualnej przez użytkownika z uprawnieniami do zapisu).

Jeśli nie masz dostępu do żadnego z tych kafelków, należy skontaktować się z administratorem współautora dostępu do aplikacji sieci web.

### <a name="dealing-with-related-resources"></a>Zajmujących się zasoby pokrewne
Aplikacje sieci Web są skomplikowane obecności kilka różnych zasobów, które wzajemne oddziaływanie. W tym miejscu jest grupą typowych zasobów z kilku witryn sieci Web:

![Grupa zasobów aplikacji sieci Web](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Dzięki temu, jeśli ktoś dostępu tylko w aplikacji sieci web, wiele funkcji w bloku witryny sieci Web w portalu Azure jest wyłączona.

Te elementy wymagają **zapisu** dostęp do **planu usługi aplikacji** odnosi się do witryny sieci Web:  

* Wyświetlanie aplikacji sieci web firmy cenowym (wolne lub standardowy)  
* Skala konfiguracji (liczba wystąpień, rozmiar maszyny wirtualnej, ustawienia skalowania automatycznego)  
* Przydziały (magazynu, przepustowości, procesor CPU)  

Te elementy wymagają **zapisu** dostęp do całego **grupy zasobów** zawierający witryny sieci Web:  

* Certyfikaty SSL i powiązań (certyfikaty SSL można udostępniać między lokacjami w tej samej grupie zasobów i lokalizacja geograficzna)  
* Reguły alertów  
* Ustawienia skalowania automatycznego  
* Application insights składników  
* Testy sieci Web  

## <a name="virtual-machine-workloads"></a>Obciążenia maszyny wirtualnej
Wiele takich jak z aplikacjami sieci web, niektóre funkcje w bloku maszyny wirtualnej wymagają dostęp do zapisu do maszyny wirtualnej lub do innych zasobów w grupie zasobów.

Maszyny wirtualne są powiązane z nazwy domeny, sieci wirtualne, konta magazynu i reguł alertów.

Te elementy wymagają **zapisu** dostęp do **maszyny wirtualnej**:

* Punkty końcowe  
* Adresy IP  
* Dyski  
* Rozszerzenia  

Wymagają one wykonywania **zapisu** dostęp do **maszyny wirtualnej**i **grupy zasobów** (wraz z nazwą domeny) jest it:  

* Zestaw dostępności  
* Zestawu o zrównoważonym obciążeniu  
* Reguły alertów  

Jeśli nie masz dostępu do żadnego z tych kafelków, skontaktuj się z administratorem współautora dostępu do grupy zasobów.

## <a name="see-more"></a>Zobacz więcej
* [Kontroli dostępu opartej na rolach](role-based-access-control-configure.md): rozpoczynanie pracy z RBAC w portalu Azure.
* [Wbudowane role](role-based-access-built-in-roles.md): uzyskiwanie szczegółowych informacji dotyczących ról, które standardowo w RBAC.
* [Role niestandardowe w Azure RBAC](role-based-access-control-custom-roles.md): Dowiedz się, jak tworzyć role niestandardowe, aby spełniały Twoje potrzeby dostępu.
* [Tworzenie raportu historii zmian dostępu](role-based-access-control-access-change-history-report.md): informacje o zmieniania przypisań ról w RBAC.

