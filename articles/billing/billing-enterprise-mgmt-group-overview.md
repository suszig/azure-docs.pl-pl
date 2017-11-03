---
title: "Organizowanie zasobów z grupami zarządzania Azure - Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat grupy zarządzania i sposobu ich używania."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: 18541c68b02ae1b59ae4a6a85122dff614c9978c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizowanie zasobów z grupami zarządzania Azure 

Jeśli masz wiele subskrypcji, można je zorganizować w o nazwie "grupy zarządzania" kontenery ułatwiające zarządzanie dostępu, zasad, kosztów i zgodności wszystkich subskrypcji. Na przykład możesz zastosować zasady z grupą zarządzania tego limitu typów zasobów, które mogą być tworzone.

> [!Note]
> Ta funkcja jest obecnie w prywatnej wersji zapoznawczej. [Zarejestruj się tutaj](https://aka.ms/MGPreviewSignup) ma informacji o rejestracji join w wersji zapoznawczej.   
 


Grupy zarządzania można organizować w hierarchii. Struktura wyświetlany jest przykładowe reprezentację hierarchii grupy zarządzania, który istnieje:


![Drzewo hierarchia](media/billing-enterprise-mgmt-groups/tree.png)



## <a name="how-management-groups-are-related-to-your-azure-enterprise-enrollment"></a>Jak grupy zarządzania dotyczące rejestracji w przedsiębiorstwie Azure

Wprowadzenie grupy zarządzania jest etapem większych drogi przejście [Enterprise portal](https://ea.azure.com) funkcje [portalu Azure](https://portal.azure.com).

Struktura grupy zarządzania jest tworzony zdefiniowanej w witrynie Enterprise portal. Całej hierarchii, składające się z konta, działów i rejestracji są mapowane do odpowiednich grup zarządzania. 

Oto, jak mapuje bieżącej struktury EA do hierarchii grupy zarządzania. 

![Drzewo hierarchia](media/billing-enterprise-mgmt-groups/tree2.png)

W poniższej tabeli przedstawiono, jak użytkownicy w witrynie Enterprise portal są mapowane do hierarchii grupy zarządzania.

|    Rola Administrator przedsiębiorstwa                                       |    Rola w węźle grupy zarządzania mapowanych    |    Uprawnienia w węźle grupy zarządzania                                                          |
|--------------------------------------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
|    Umowa EA administratora                              |    Współautor zasad zasobów                   |    Można przeglądać kosztów, zasobu zasad i Widok hierarchii poziomie i poniżej węzła rejestracji    |
|    Administrator EA w trybie tylko do odczytu            |    Czytnik rozliczeń                                |    Można odczytać kosztów i wyświetlanie hierarchii w poziomie i poniżej węzła rejestracji                              |
|    Administratorem działu                      |    Czytnik rozliczeń                                |    Można odczytać kosztów i wyświetlanie hierarchii i poniżej węzła działu                                 |
|    Administratorem działu w trybie tylko do odczytu    |    Czytnik rozliczeń                                |    Można odczytać kosztów i wyświetlanie hierarchii i poniżej węzła działu                                 |
|    Właściciel konta                                 |    Współautor zasad zasobów                   |    Można przeglądać kosztów, zasobu zasad i Widok hierarchii poziomie i poniżej węzła konta       |




## <a name="view-management-groups-in-the-azure-portal"></a>Wyświetlanie grup zarządzania w portalu Azure

Aby wyświetlić rejestracji, działu lub konto w wersji zapoznawczej, zaloguj się w portalu Azure z łączem w powitalnej wiadomości e-mail.   

![Hierarchii](media/billing-enterprise-mgmt-groups/hierarchy.png)

### <a name="viewing-costs"></a>Wyświetlanie kosztów 
Na ekranach szczegółów grup zarządzania zobacz temat bieżące koszty miesięcy do daty. Koszty te są oparte na użycia i bez uwzględnienia przedpłacone kwoty, nadwyżki ilości dołączone, dostosowania i podatki. Dla grupy zarządzania odpowiadający informacji o rejestracji w sekcji kosztów zawiera zobowiązań pozostałych.  

![Szczegóły rejestracji](media/billing-enterprise-mgmt-groups/enrollment.png)

 "Koszty rachunki" są miesięczne akumulacji oddzielne zmian, takie jak witryny marketplace, nadwyżki i innych kosztów, które nie przechodzą przed zobowiązań Twojej rejestracji.  Aby uzyskać więcej informacji dotyczących podziału kosztów, zobacz [Enterprise portal](https://ea.azure.com). 

### <a name="enabling-access-to-costs"></a>Włączanie dostępu do kosztów
Jeśli nie widzisz kosztów, zobacz nasze [enterprise rozwiązywanie koszt widoków](https://aka.ms/enableazurecosts) dokumentu, aby uzyskać pomoc.  

### <a name="delays-between-the-enterprise-portal-and-azure-portal"></a>Opóźnienia między Enterprise portal i portalu Azure 
* W wersji zapoznawczej kwoty wyświetlone w portalu Azure mogą być opóźnione w porównaniu do wartości w witrynie Enterprise portal. Ten problem jest tymczasowy i Trwa praca.
* Zaktualizowano ustawienia w witrynie Enterprise portal mają opóźnienia kilka minut, zanim aktualizacje zostaną odzwierciedlone w portalu Azure. 

## <a name="management-groups-have-a-relationship-with-your-directory"></a>Grupy zarządzania mają relację z katalogu   
Podobnie jak subskrypcji grupy zarządzania mają relację zaufania z usługą Azure AD. Hierarchia grup zarządzania ufa jeden katalog do uwierzytelniania użytkowników. Wszystkich administratorów skojarzonych z hierarchii grup zarządzania muszą należeć do tego samego katalogu. 

Ponieważ hierarchii rejestracji jest zamapowany do grup zarządzania, z jednego katalogu jest ustanowić relacji zaufania. Jeśli to możliwe, wybrano istniejący katalog skojarzonego z kontami użytkownika rejestracji. W niektórych przypadkach nowy katalog jest tworzony, i wszystkich istniejących rejestracji użytkowników, zachęca się do tego katalogu. Nie dotyczy skojarzone z subskrypcją rejestracji. W związku z tym hierarchii może być tworzone w katalogu innym niż subskrypcji. [Dowiedz się więcej](billing-enterprise-mgmt-grp-find.md) o jak proces ten ma wpływ na środowisko nawigowanie między hierarchii i jego subskrypcji.

## <a name="administering-your-management-groups"></a>Administrowanie grup zarządzania
Grupy zarządzania w portalu Azure w wersji zapoznawczej i są tylko do odczytu w początkowej wersji. Aby zapewnić wszelkie aktualizacje, przejdź do Enterprise portal. Aktualizacje są automatycznie odzwierciedlane w portalu Azure. W dokumentacji w witrynie enterprise portal w celu uzyskania pomocy dotyczące wprowadzania zmian i dodatków.   

## <a name="policy-management"></a>Zarządzanie zasadami
Usługa Resource Manager umożliwia tworzenie zasad niestandardowych na potrzeby zarządzania zasobami. Z grupy zarządzania, zasady można przypisać na dowolnym poziomie w hierarchii i zasoby dziedziczy tych zasad.  [Dowiedz się więcej](https://go.microsoft.com/fwlink/?linkid=858942)

> [!Note]
> Zasady nie jest wymuszany dla katalogów. 


