---
title: "Rozwiązywanie problemów z automatycznie skalowana za pomocą zestawów skali maszyny wirtualnej | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z automatycznie skalowana za pomocą zestawów skali maszyny wirtualnej. Zrozumieć typowych problemów i ich rozwiązania."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: bd45a0fb99a77851aa7b91d23bd4b830b6f5cc7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Rozwiązywanie problemów z automatycznie skalowana za pomocą zestawów skali maszyny wirtualnej
**Problem** — po utworzeniu Skalowanie automatyczne infrastruktury w usłudze Azure Resource Manager przy użyciu zestawy skalowania maszyny Wirtualnej — na przykład, przez wdrożenie szablonu następująco: https://github.com/Azure/azure-quickstart-templates/tree/master/201- vmss-bottle — funkcja automatycznego skalowania — masz zdefiniowanych reguł skalowania i działa ona ponosić, z tą różnicą, że niezależnie od tego, ile obciążenia umieszczone na maszynach wirtualnych, nie będzie skalowania automatycznego.

## <a name="troubleshooting-steps"></a>Kroki rozwiązywania problemów
Kilka rzeczy, które należy uwzględnić obejmują:

* Liczby rdzeni każda maszyna wirtualna ma i czy ładowanie każdego rdzenia?
  Przykładowy szablon Szybki Start Azure powyżej ma do_work.php skrypt, który jest ładowany przez pojedynczy rdzeń. Jeśli używane jest większy niż rozmiar maszyny Wirtualnej, takie jak Standard_A1 lub D1 pojedynczego rdzenia maszyny Wirtualnej, a następnie należy uruchamiać wielokrotnie obciążenie. Sprawdź, ile rdzenie maszyn wirtualnych, przeglądając [rozmiary dla systemu Windows maszyny wirtualne na platformie Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Jak wiele maszyn wirtualnych w zestawie skalowania maszyn wirtualnych, możesz operacji pracy na każdej z nich?
  
    Staną się skalowania zdarzeń podczas umieszczania średnie wykorzystanie Procesora przez **wszystkich** w zestawie skalowania maszyn wirtualnych przekracza wartość progową, wraz z upływem czasu wewnętrznego zdefiniowanych w zasadach automatycznego skalowania.
* Czy zapomniano wszelkie zdarzenia skalowania?
  
    Przejrzyj dzienniki inspekcji w portalu Azure zdarzeń skali. Może być wystąpił skali górę i skalowania w dół, która została pominięta. Można filtrować według "Skala"...
  
    ![Dzienniki inspekcji][audit]
* Wartości progowe w skali i skalowalnego w poziomie są różni się wystarczająco?
  
    Załóżmy, że możesz ustawić regułę do skalowania w poziomie, gdy średnie wykorzystanie Procesora jest większa niż 50% w ciągu 5 minut, a także do skalowania w przypadku średnie wykorzystanie Procesora na mniej niż 50%. To spowodowałoby problem "flapping", gdy użycie procesora CPU jest bliski tego progu, z akcji skalowania stale zwiększania i zmniejszania rozmiaru zestawu. W związku z tym usługi skalowania automatycznego próbuje zapobiec "niestabilny", który można manifestu jako skalowania nie. W związku z tym upewnij się, że wartości progowe skalowalnego w poziomie i w skali różnią się wystarczająco Zezwalaj odstępem Between skalowania.
* Czy pisania szablonu JSON?
  
    Jest łatwy do popełnione, więc uruchomienie przy użyciu szablonu, takie jak jest tą, nad którym sprawdzone do pracy i małych zmiany przyrostowe. 
* Można ręcznie skalowanie przychodzący lub wychodzący?
  
    Spróbuj ponownie wdrożyć zasobów zestawu skali maszyny Wirtualnej z ustawieniem różnych "pojemność", aby ręcznie zmienić liczbę maszyn wirtualnych. Przykład szablonu w tym celu jest tutaj: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing — może być konieczne edytowanie szablonu, aby upewnić się, jak zestawu skalowania używa ma ten sam rozmiar maszyny. Czy można pomyślnie ręcznie zmienić liczbę maszyn wirtualnych, następnie wiadomo, że problem dotyczy tylko skalowania automatycznego.
* Sprawdź Twojej Microsoft.Compute/virtualMachineScaleSet i zasoby dostępne w elemencie Microsoft.Insights [Eksploratora zasobów Azure](https://resources.azure.com/)
  
    Jest to niezbędne rozwiązywania problemów, które pokazuje stan zasobów usługi Azure Resource Manager. Kliknij subskrypcję i przyjrzyj się grupa zasobów jest rozwiązywany problem. W obszarze dostawcy zasobów obliczeniowych przyjrzeć się zestawu skali maszyny Wirtualnej został utworzony i sprawdź widok wystąpienia, które prezentowany jest stan wdrożenia. Sprawdź również widok wystąpienia maszyny wirtualnej w zestawie skalowania maszyn wirtualnych. Następnie przejdź w elemencie Microsoft.Insights dostawcy zasobów i sprawdź, czy wygląd reguł skalowania automatycznego.
* Jest diagnostycznych rozszerzenia pracy i wysyłających dane dotyczące wydajności?
  
    **Aktualizacja:** Azure automatycznego skalowania zostało rozszerzone, aby użyć potoku metryki oparte na hoście, który nie wymaga już rozszerzenie diagnostyki do zainstalowania. Oznacza to następnych kilku akapitów już stosowane w przypadku utworzenia aplikacji Skalowanie automatyczne przy użyciu nowego potoku. Na przykład szablonów platformy Azure, które zostały przekonwertowane na Użyj procesu hosta: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Przy użyciu hosta na podstawie metryki skalowania automatycznego jest lepszym rozwiązaniem w następujących sytuacjach:
  
  * Mniej części ruchome jako rozszerzenia nie diagnostyki muszą być zainstalowane.
  * Szablony prostsze. Wystarczy dodać reguły automatycznego skalowania wgląd do istniejącego szablonu zestaw skali.
  * Bardziej niezawodne, raportowanie i szybsze uruchamianie nowych maszyn wirtualnych.
    
    Tylko przyczyn można nadal używać rozszerzenia diagnostycznych będzie konieczne będzie pamięci diagnostyki raportowania/skalowania. Host, na podstawie metryk nie raportują pamięci.
    
    Z tym pamiętać tylko wykonaj dalszej części tego artykułu, nadal używasz rozszerzenia diagnostycznych dla Twojego Skalowanie automatyczne.
    
    Funkcja automatycznego skalowania usługi Azure Resource Manager można pracować (ale nie ma już do) za pomocą maszyny Wirtualnej o nazwie rozszerzenia rozszerzenia diagnostyki. Emituje on dane wydajności do konta magazynu definiowane w szablonie. Te dane są agregowane przez usługę Azure Monitor.
    
    Jeśli usługa Insights nie może odczytać danych z maszyn wirtualnych, powinien wysłać wiadomość e-mail — na przykład jeśli maszyny wirtualne zostały w dół, więc poczty e-mail (po jednym określone podczas tworzenia konta platformy Azure).
    
    Możesz również przejść i wyglądać na dane. Spójrz na konto magazynu Azure, w Eksploratorze chmury. Na przykład za pomocą [programu Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), zaloguj się i pobranie używasz subskrypcji platformy Azure, a nazwa konta magazynu diagnostyki występujący w odwołaniu w definicji rozszerzenia diagnostyki w szablonie wdrożenia...
    
    ![Eksplorator chmury][explorer]
    
    W tym miejscu zobaczysz grupy tabel, w którym jest przechowywane dane z każdej maszyny Wirtualnej. Biorąc systemu Linux i metrykę procesora CPU, na przykład Szukaj podczas ostatnich wierszy. Eksplorator chmury programu Visual Studio obsługuje język zapytań, więc można uruchomić zapytania, takie jak "datetime gt sygnatura czasowa" 2016-02-02T21:20:00Z "" Aby upewnić się, Pobierz najnowsze zdarzenia (przyjmowane jest czas jest w formacie UTC). Czy dane, które widać w odpowiadają reguł skalowania skonfigurowaniu? W poniższym przykładzie Procesora dla maszyny 20 uruchomiona zwiększenie 100% w ciągu ostatnich 5 minut.
    
    ![Magazyn tabel][tables]
    
    Jeśli dane nie są dostępne, następnie zakłada się, że problem dotyczy diagnostycznych rozszerzenia uruchomionych w maszynach wirtualnych. Jeśli dane są dostępne, oznacza to, że znajduje się problemami z regułami skali lub w usłudze Insights. Sprawdź [Azure stan](https://azure.microsoft.com/status/).
    
    Po już został tych kroków, jeśli nadal występują problemy z automatycznego skalowania można wypróbować forach na [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), lub [przepełnienie stosu](http://stackoverflow.com/questions/tagged/azure), lub pomocy technicznej. Przygotuj się na udostępnianie szablonu i widok danych wydajności.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
