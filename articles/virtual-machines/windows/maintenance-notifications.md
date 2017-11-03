---
title: "Obsługa powiadomień dotyczących konserwacji dla maszyn wirtualnych systemu Windows na platformie Azure | Dokumentacja firmy Microsoft"
description: "Wyświetlanie powiadomień konserwacji dla maszyn wirtualnych systemu Windows działających na platformie Azure i uruchomić samoobsługi konserwacji."
services: virtual-machines-windows
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: zivr
ms.openlocfilehash: fec64b3c499577af6b1d6eddb1c761ee0af73772
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="handling-planned-maintenance-notifications-for-windows-virtual-machines"></a>Obsługa planowanych konserwacji powiadomienia dla maszyn wirtualnych systemu Windows

Azure wykonuje okresowo aktualizacje do poprawy niezawodności, wydajności i zabezpieczeń infrastruktury hosta dla maszyny wirtualnej. Aktualizacje są zmiany, takie jak stosowanie poprawek do środowiska macierzystego lub uaktualnianie i likwidowaniu sprzętu. Większość tych aktualizacji są wykonywane bez wpływu na maszyny wirtualne. Istnieją jednak przypadki, w których aktualizacje mają wpływ:

- Jeśli konserwacji nie wymaga ponownego uruchomienia systemu, platforma Azure korzysta migracji w miejscu wstrzymanie maszyny Wirtualnej, gdy host jest aktualizowany.

- Jeśli konserwacji wymaga ponownego uruchomienia, możesz uzyskać zawiadomienia o podczas jest planowanych konserwacji. W takich przypadkach podano przedział czasu, w którym można uruchomić obsługi samodzielnie, gdy jest używany dla Ciebie.


Planowana konserwacja, która wymaga ponownego uruchomienia, zaplanowano etapami. Każdy wave ma inny zakres (regiony).

- Wave rozpoczyna się od powiadomienia do klientów. Domyślnie powiadomienia są wysyłane do subskrypcji właściciel i współwłaściciele. Możesz dodać więcej adresatów i opcje obsługi wiadomości, takie jak wiadomości e-mail, SMS i elementów Webhook, aby powiadomienia.  
- Wkrótce po powiadomieniu Samoobsługowe okna jest ustawiona. W tym oknie można znaleźć, który maszyn wirtualnych znajduje się w tym wave i rozpocząć konserwacji przy użyciu aktywnego ponownego wdrażania. 
- Następujące okna samoobsługi rozpoczyna się okno zaplanowanej konserwacji. W tej chwili Azure harmonogramy i stosuje obsługi wymaganych do maszyny wirtualnej. 

Celem o dwa okna jest zapewniają wystarczająco dużo czasu, aby uruchomić konserwacji i ponownego uruchamiania maszyny wirtualnej, a Azure automatycznie rozpoczęcia konserwacji.


Portalu Azure, programu PowerShell, interfejsu API REST i interfejsu wiersza polecenia umożliwia zapytania dla okna obsługi dla maszyn wirtualnych i uruchomić samoobsługi konserwacji.

 > [!NOTE]
 > Jeśli użytkownik próbuje uruchomić konserwacji i kończyć się niepowodzeniem, Azure oznacza maszyny Wirtualnej jako **pominięte** i nie uruchamiaj ponownie go podczas czasu zaplanowanego okna obsługi. Zamiast tego należy kontaktować się ze w późniejszym czasie z nowego harmonogramu. 


[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="check-maintenance-status-using-powershell"></a>Sprawdź stan konserwacji przy użyciu programu PowerShell

Za pomocą programu Powershell Azure można również wyświetlić podczas planowania obsługi maszyn wirtualnych. Planowana konserwacja informacje są dostępne z [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) polecenia cmdlet, korzystając z `-status` parametru.
 
Informacje konserwacji są zwracane tylko wtedy, gdy planowanych konserwacji. Jeśli zdefiniowano żadnej konserwacji zaplanowane tej wpływa na Maszynie wirtualnej, polecenie cmdlet nie zwraca żadnych informacji konserwacji. 

```powershell
Get-AzureRmVM -ResourceGroupName rgName -Name vmName -Status
```

W obszarze MaintenanceRedeployStatus zwracane są następujące właściwości: 
| Wartość | Opis   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Wskazuje, czy w tej chwili można wykonać obsługi maszyny Wirtualnej ||
| PreMaintenanceWindowStartTime         | Na początku samoobsługi oknem obsługi po rozpoczęciu konserwacji na maszynie Wirtualnej ||
| PreMaintenanceWindowEndTime           | Koniec samoobsługi oknem obsługi po rozpoczęciu konserwacji na maszynie Wirtualnej ||
| MaintenanceWindowStartTime            | Na początku okna zaplanowanej konserwacji, jeśli można zainicjować obsługi na maszynie Wirtualnej ||
| MaintenanceWindowEndTime              | Koniec okna zaplanowanej konserwacji, jeśli można zainicjować obsługi na maszynie Wirtualnej ||
| LastOperationResultCode               | Wynik ostatniej próby zainicjowanie obsługi w maszynie Wirtualnej ||



Umożliwia również wyświetlenie stanu konserwacji dla wszystkich maszyn wirtualnych w grupie zasobów, przy użyciu [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) i nieokreślenie maszyny Wirtualnej.
 
```powershell
Get-AzureRmVM -ResourceGroupName rgName --Status
```

Następująca funkcja PowerShell ma identyfikator subskrypcji i listę maszyn wirtualnych, które są planowane do obsługi do drukowania.

```powershell

function MaintenanceIterator
{
    Select-AzureRmSubscription -SubscriptionId $args[0]

    $rgList= Get-AzureRmResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]
        $vmList = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>Uruchom konserwacji na maszynie Wirtualnej za pomocą programu PowerShell

W poprzedniej sekcji, korzystając z informacji z funkcji, następujące uruchamia konserwacji na maszynie Wirtualnej Jeśli **IsCustomerInitiatedMaintenanceAllowed** jest ustawiona na true.

```powershell
Restart-AzureRmVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="faq"></a>Często zadawane pytania


**Pytanie: Dlaczego należy przeprowadzić ponowny rozruch teraz moje maszyny wirtualne?**

**Odpowiedź:** podczas większość aktualizacji i uaktualnień do platformy Azure nie wpływają na dostępność maszyn wirtualnych, istnieją przypadki, w którym firma Microsoft nie należy unikać ponownego uruchamiania maszyn wirtualnych hostowanych na platformie Azure. Firma Microsoft współdzielenia kilka zmian, które wymagają firmie Microsoft w celu ponownego uruchomienia nasze serwery, które spowoduje ponowne uruchomienie maszyn wirtualnych.

**Pytanie: czy wykonać zalecenia wysokiej dostępności przy użyciu zestawu dostępności, mogę awaryjnym?**

**Odpowiedź:**ustawić maszyn wirtualnych wdrożonych w dostępności lub zestawy skalowania maszyny wirtualnej mają podstawowe pojęcie w zakresie domen aktualizacji (UD). Podczas przeprowadzania konserwacji, Azure będzie honorować ograniczenia UD i nie zostanie wykonany ponowny rozruch maszyny wirtualne z różnych UD (w tym samym zestawie dostępności).  Azure również oczekuje na co najmniej 30 minut przed przejściem do następnej grupy maszyn wirtualnych. 

Aby uzyskać więcej informacji o wysokiej dostępności zapoznaj się zarządzanie dostępność maszyn wirtualnych systemu Windows na platformie Azure lub Zarządzaj dostępnością maszyn wirtualnych systemu Linux na platformie Azure.

**Pytanie: czy masz zestawu w innym regionie odzyskiwania po awarii. Mogę awaryjnym?**

**Odpowiedź:** Azure każdy region jest łączyć się z innego regionu w tej samej lokalizacji geograficznej (takich jak USA, Europa lub Azja). Zaplanowane aktualizacje platformy Azure są wdrażane w powiązanych regionach pojedynczo, aby zminimalizować przestoje i ryzyko awarii aplikacji. Podczas zaplanowanej konserwacji Azure może zaplanować podobne okna dla użytkowników uruchomić konserwacji, jednak czasu zaplanowanego okna obsługi będzie różna sparowanego regionów.  

Aby uzyskać więcej informacji o regionach platformy Azure Zobacz regionów i dostępności dla maszyn wirtualnych na platformie Azure.  Widać pełną listę regionalnych pary tutaj.

**Pytanie: jak otrzymywać powiadomienia o zaplanowanej konserwacji?**

**Odpowiedź:** wave zaplanowanej konserwacji rozpoczyna się przez ustawienie harmonogramu co najmniej jeden regiony platformy Azure. Wkrótce po powiadomienie e-mail są wysyłane do właścicieli subskrypcji (jeden adres e-mail dla subskrypcji). Dodatkowych kanałów oraz odbiorców to powiadomienie może zostać skonfigurowany za pomocą alertów dotyczących działań w dzienniku. W przypadku, gdy należy wdrożyć maszynę wirtualną w regionie, w którym jest już zaplanowana zaplanowanej konserwacji, nie będą otrzymywać powiadomienia, ale raczej muszą sprawdzać stan konserwacji maszyny wirtualnej.

**Pytanie: nie widzę wskazania zaplanowanej konserwacji w portalu, programu Powershell lub interfejsu wiersza polecenia, na czym polega problem?**

**Odpowiedź:** informacje powiązane z zaplanowanej konserwacji jest dostępna podczas wave zaplanowanej konserwacji, tylko dla maszyn wirtualnych, które będą mieć wpływ go. Innymi słowy Jeśli dane nie są wyświetlane, możliwe że wave konserwacji ma już wykonane (lub nie jest uruchomiony) lub że maszyna wirtualna już znajduje się w zaktualizowany serwer.

**Q: utrzymanie powinno być uruchamianych na Moja maszyna wirtualna?**

**Odpowiedź:** ogólnie rzecz biorąc, obciążeń, które są wdrażane w usłudze w chmurze, zestaw dostępności lub zestawu skalowania maszyn wirtualnych, są odporne na zaplanowanej konserwacji.  Podczas zaplanowanej konserwacji pogarsza tylko jedną aktualizację domeny w danym momencie. Należy pamiętać, że kolejność domen aktualizacji jest w pełni funkcjonalne nie zawsze odbywa się po kolei.

Można uruchomić samodzielnie konserwacji w następujących przypadkach:
- Aplikacja działa na jednej maszynie wirtualnej i konieczne jest zastosowanie wszystkich konserwacji godzinami
- Musisz koordynować czas konserwacji w ramach Twojej umowy SLA
- Wymagają więcej niż 30 minut między każdym ponownym uruchomieniu maszyny Wirtualnej, nawet w ramach dostępności ustawieniu.
- Chcesz wyłączyć całej aplikacji (Konfiguracja wielu warstw, wielu domen aktualizacji), aby dokończyć szybsze konserwacji.

**Pytanie: czy istnieje sposób informacji, aby dokładnie Moja maszyna wirtualna jest ograniczona?**

**Odpowiedź:** przy ustalaniu harmonogramu, definiujemy okno czasu kilka dni. Jednak dokładne sekwencjonowania serwerów (i maszyn wirtualnych), w tym przedziale czasu jest nieznany. Klienci, którzy wiedzieć dokładny czas ich maszyn wirtualnych można użyć zdarzenia zaplanowane i zapytania z poziomu maszyny wirtualnej i otrzymasz powiadomienie 10 minut przed ponownym uruchomieniem maszyny Wirtualnej.
  
**Pytanie: jak długo ponownego uruchomienia Moja maszyna wirtualna Trwa?**

**Odpowiedź:** w zależności od rozmiaru maszyny Wirtualnej, ponowne uruchomienie komputera może potrwać kilka minut. Należy pamiętać, że w przypadku, gdy używasz usługi w chmurze, skalowanie Ustawia lub zestawu dostępności, będziesz mieć możliwość 30 minut między wszystkimi grupami maszyn wirtualnych (UD). 

**Pytanie: jakie będzie środowisko w przypadku usługi w chmurze, zestawy skalowania i sieci szkieletowej usług?**

**Odpowiedź:** podczas zaplanowanej konserwacji wpływ na tych platformach, klienci korzystający z tych platform są uznawane za bezpieczne podane tego tylko maszyny wirtualne w jednej uaktualnienia domeny (UD) jest ograniczona w danym momencie.  

**Pytanie: czy mogę zostać odebrana wiadomość e-mail na temat likwidowania sprzętu, jest to taka sama jak zaplanowanej konserwacji?**

**Odpowiedź:** podczas likwidowania sprzętu jest zdarzeń planowanych konserwacji, mamy jeszcze nie został załadowany ten przypadek użycia do nowego środowiska.  Oczekuje się, klienci mogą uzyskać mylić w przypadku, gdy otrzymają dwóch podobnych wiadomości e-mail o dwóch różnych zaplanowanej konserwacji fal.

**Pytanie: nie widzę żadnych informacji konserwacji na moich maszynach wirtualnych. Co poszło źle?**

**Odpowiedź:** istnieje kilka przyczyn, dlaczego czasie nie widać żadnych informacji konserwacji na maszyny wirtualne:
1.  Używane są oznaczone jako Microsoft wewnętrzny subskrypcji.
2.  Maszyny wirtualne nie są zaplanowane do obsługi. Możliwe, że wave obsługi zostało zakończone, anulowane lub zmodyfikować tak, aby maszyny wirtualne są nie wpływa już go.
3.  Nie masz kolumny "Obsługa" dodane do widoku listy maszyny Wirtualnej. Gdy ta kolumna dodano widok domyślny, klientów, którzy skonfigurowany tak, aby zobaczyć kolumn innych niż domyślne należy ręcznie dodać **konserwacji** kolumny do widoku listy ich maszyny Wirtualnej.

**Pytanie: czy Moje maszyny Wirtualnej jest zaplanowane do obsługi po raz drugi. Dlaczego?**

**Odpowiedź:** istnieje kilka przypadków użycia, gdy pojawi się zaplanowane do obsługi po ukończeniu już konserwacji-ponownego wdrażania maszyny Wirtualnej:
1.  Firma Microsoft ma anulowane wave konserwacji i ponownie uruchomić go z różnych ładunku. Możliwe, że wykryliśmy błędnej ładunku i po prostu należy wdrożyć dodatkowe ładunku.
2.  Maszyna wirtualna została *usługi zabliźnione* do innego węzła z powodu awarii sprzętowej
3.  Wybrano opcję zatrzymania (deallocate) i uruchom ponownie maszynę Wirtualną
4.  Masz **automatycznego zamykania** włączony dla maszyny Wirtualnej


## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak można zarejestrować obsługi zdarzeń z wewnątrz maszynę Wirtualną przy użyciu [zaplanowane zdarzenia](scheduled-events.md).