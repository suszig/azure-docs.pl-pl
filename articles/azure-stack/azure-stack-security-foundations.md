---
title: "Opis formantów zabezpieczeń stosu Azure | Dokumentacja firmy Microsoft"
description: "Administrator usługi więcej informacji na temat opcji zabezpieczeń stosowane do stosu Azure"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2018
ms.author: mabrigg
ms.openlocfilehash: c1d92f8f2ed9e8ab504afc65bab861e1f7bb3689
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="azure-stack-infrastructure-security-posture"></a>Stan zabezpieczeń infrastruktury w usłudze Azure stosu

*Dotyczy: Azure stosu zintegrowane systemy*

Zagadnienia dotyczące zabezpieczeń i przepisy dotyczące zgodności są sterowniki głównego przy użyciu chmury hybrydowej. Stos Azure została zaprojektowana dla tych scenariuszy i ważne jest, aby zrozumieć formantów już w miejscu, przyjmując stosu Azure.

W stosie Azure istnieją dwie warstwy stan zabezpieczeń, które współistnieć. Pierwsza warstwa składa się z infrastrukturą stosu Azure, która przechodzi z składniki sprzętowe aż do usługi Azure Resource Manager i zawiera portale dzierżawy i administratora. Druga warstwa składa się z obciążeń, które dzierżawcom tworzenie, wdrażanie i zarządzanie nimi i obejmować takie rzeczy jak maszyny wirtualne lub witryny sieci web usługi aplikacji.  

## <a name="security-approach"></a>Metoda zabezpieczeń
Stos Azure została zaprojektowana z stan zabezpieczeń obrony pod kątem współczesnych zagrożeń i został utworzony w celu spełnienia wymagań z standardów zgodności głównych. W związku z tym stan zabezpieczeń w infrastrukturze Azure stosu jest oparty na dwóch filarach:

 - **Przykładowa naruszenia.**  
Począwszy od założenia, że nastąpiło już naruszenie systemu, skupić się na *wykrywanie i ograniczanie wpływu naruszenia* i tylko w trakcie przed atakami. 
 - **Wzmocnione zabezpieczenia domyślne.**  
Ponieważ infrastruktura działa w dobrze zdefiniowanej sprzętu i oprogramowania, firma Microsoft *włączenia, skonfigurowania i zweryfikować wszystkie funkcje zabezpieczeń* domyślnie.



Ponieważ stos Azure jest dostarczane jako zintegrowany system, stan zabezpieczeń w infrastrukturze Azure stosu jest zdefiniowana przez firmę Microsoft. Podobnie jak na platformie Azure dzierżaw są zobowiązani do definiowania stan zabezpieczeń ich obciążeń dzierżawców. Ten dokument zawiera on podstawowych wiedzy na stan zabezpieczeń w infrastrukturze Azure stosu.

## <a name="data-at-rest-encryption"></a>Dane na rest szyfrowania
Wszystkie dane stosu Azure infrastruktury i dzierżawcy jest szyfrowane, gdy za pomocą funkcji Bitlocker. Szyfrowanie chroni przed fizycznych utrata lub kradzież składników magazynu Azure stosu. 

## <a name="data-in-transit-encryption"></a>Szyfrowanie przesyłanych danych
Składniki infrastruktury Azure stosu komunikują się za pomocą kanałów zaszyfrowane za pomocą protokołu TLS 1.2. Certyfikaty szyfrowania zarządza własnym infrastruktury. 

Wszystkie punkty końcowe zewnętrznych infrastruktury, takich jak punkty końcowe REST lub w portalu Azure stos obsługi protokołu TLS 1.2 dla bezpiecznej komunikacji. Certyfikaty szyfrowania, z innej firmy lub urząd certyfikacji przedsiębiorstwa, należy określić dla tych punktów końcowych. 

Chociaż mogą być używane certyfikaty z podpisem własnym dla tych zewnętrzne punkty końcowe, Microsoft zdecydowanie z informacją o tym przed ich użyciem. 

## <a name="secret-management"></a>Tajny zarządzania
Azure infrastruktura stosu używa wieloma tajemnice, takie jak hasła, do funkcji. Większość z nich są automatycznie obracane często, ponieważ są one konta usług zarządzanych grupy, które Obróć co 24 godziny.

Pozostałe hasła, które nie są kont usług zarządzanych grupy można obracać ręcznie przy użyciu skryptu w uprzywilejowanych punktu końcowego.

## <a name="code-integrity"></a>Integralność kodu
Stos Azure korzysta z najnowszych systemu Windows Server 2016 funkcje zabezpieczeń. Jeden z nich jest systemu Windows Defender ochrony urządzeń, zawiera listę dozwolonych aplikacji podobnej i zapewnia, że tylko autoryzowani uruchamia kod w infrastrukturze Azure stosu. 

Autoryzowanego kodu jest podpisany przez firmę Microsoft lub partnerem OEM i znajduje się na liście dozwolonych oprogramowania, które jest określone w zasadach zdefiniowana przez firmę Microsoft. Innymi słowy mogą być wykonywane tylko oprogramowania, które zostały zatwierdzone do uruchamiania w infrastrukturze Azure stosu. Każda próba wykonania nieautoryzowanego kodu jest zablokowana i wygenerowaniu inspekcji.

Zasady ochrony urządzeń zapobiega także innych agentów lub oprogramowania uruchomionych w infrastrukturze Azure stosu.

## <a name="credential-guard"></a>Ochrona poświadczeń
Innej funkcji zabezpieczeń systemu Windows Server 2016 w stosie Azure jest Windows Defender poświadczeń Guard, który służy do ochrony Azure stosu infrastruktury poświadczeń przed atakami Pass--Ticket i Pass--Hash.

## <a name="antimalware"></a>Oprogramowanie chroniące przed złośliwym kodem
Każdy składnik Azure stosu (hosty funkcji Hyper-V i maszyn wirtualnych) jest chroniony za pomocą programu Windows Defender wirusami.

## <a name="constrained-administration-model"></a>Model administracyjny ograniczone
Administracja w stosie Azure jest kontrolowany przy użyciu trzech punktów wejścia, każdy z określonym przeznaczeniem: 
1. [Portalu administratora](azure-stack-manage-portals.md) zapewnia obsługę punktu i kliknij codziennych operacji zarządzania.
2. Usługa Azure Resource Manager udostępnia wszystkie operacje zarządzania portalu administratora za pomocą interfejsu API REST, używany przez programu PowerShell i interfejsu wiersza polecenia Azure. 
3. Z określonymi operacjami niskiego poziomu, na przykład dane Centrum integracji lub obsługi scenariuszy, stos Azure udostępnia punkt końcowy programu PowerShell o nazwie [uprzywilejowanych punktu końcowego](azure-stack-privileged-endpoint.md). Ten punkt końcowy przedstawia tylko białej zestaw poleceń cmdlet i silnie podlega inspekcji.

## <a name="network-controls"></a>Formanty sieci
Azure infrastruktury stosu jest dostarczany z wielu warstw List(ACL) kontroli dostępu do sieci. Listy ACL uniemożliwić nieautoryzowany dostęp do składników infrastruktury i ograniczyć infrastruktury komunikacji do ścieżek, które są wymagane do jego działania. 

Listy kontroli dostępu w sieci są wymuszane w trzech warstw:
1.  Zmienia początku stojak
2.  Sieć definiowana przez oprogramowanie
3.  Zapory systemu operacyjnego hosta i maszyny Wirtualnej 


