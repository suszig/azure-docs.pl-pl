---
title: "Rozwiązywanie problemów z Microsoft Azure stosu | Dokumentacja firmy Microsoft"
description: "Azure stosu dotyczących rozwiązywania problemów."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 3b40a657ee8eb391d14a38cb95acc0729a8dda21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Rozwiązywanie problemów z Microsoft Azure stosu

*Dotyczy: Azure stosu Development Kit*

Ten dokument zawiera typowe informacje dotyczące rozwiązywania problemów dla stosu Azure. 

Ponieważ Azure stosu techniczne Development Kit jest oferowany jako środowisko oceny, nie jest oficjalną obsługiwane z usług obsługi klienta firmy Microsoft.  Jeśli występuje problem nie opisano, upewnij się sprawdzić [Forum MSDN stosu Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) dalszą pomoc i informacje.  

Zalecenia dotyczące rozwiązywania problemów, które zostały opisane w tej sekcji są uzyskiwane z wielu źródeł i może lub nie może rozpoznać określonego problemu. Przykłady kodu są dostarczane, ponieważ jest i nie można zagwarantować oczekiwanych rezultatów. W tej sekcji podlega częste zmiany i aktualizacje zaimplementowanego ulepszenia produktu.

## <a name="deployment"></a>Wdrożenie
### <a name="deployment-failure"></a>Niepowodzenie wdrożenia
Jeśli wystąpi błąd podczas instalacji, można użyć opcji Uruchom ponownie skrypt wdrożenia ponowne wdrożenie z kroku zakończonego niepowodzeniem.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Po zakończeniu wdrożenia sesji programu PowerShell jest wciąż otwarty i nie wyświetla żadnych danych wyjściowych
To zachowanie jest prawdopodobnie tylko wynik domyślne zachowanie okno poleceń programu PowerShell, gdy została wybrana. Rozwój wdrożenie zestawu faktycznie ma zakończyło się pomyślnie, ale skrypt został wstrzymany, wybierając okna. Możesz sprawdzić, czy jest to możliwe, wyszukując słowa "Wybierz" pasek tytułu okna wiersza polecenia.  Naciśnij klawisz ESC, aby usunąć jej zaznaczenie, a powinien być wyświetlany komunikat o zakończeniu po nim.

## <a name="templates"></a>Szablony
### <a name="azure-template-wont-deploy-to-azure-stack"></a>Szablon Azure nie będą wdrażane Azure stosu
Upewnij się, że:

* Szablon musi korzystać z usługi Microsoft Azure, który jest już dostępny, lub w wersji zapoznawczej w stosie Azure.
* Interfejsy API używane dla konkretnego zasobu są obsługiwane przez lokalne wystąpienie usługi Azure stosu i przeznaczonych prawidłowej lokalizacji ("local" Azure stosu development kit, vs "Wschodnie stany USA" lub "Indie Południowe" na platformie Azure).
* Należy przejrzeć [w tym artykule](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) o poleceniach cmdlet AzureRmResourceGroupDeployment testu, który przechwytuje niewielkie różnice w składni usługi Azure Resource Manager.

Umożliwia także szablony stosu Azure już podany w [repozytorium GitHub](http://aka.ms/AzureStackGitHub/) ułatwiające rozpoczęcie pracy.

## <a name="virtual-machines"></a>Maszyny wirtualne
### <a name="default-image-and-gallery-item"></a>Element domyślny obraz i galerii
Najpierw należy dodać element obrazu i galerii systemu Windows Server przed wdrożeniem maszyn wirtualnych Azure stosu.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Po ponownym uruchomieniu hosta Moje stosu Azure, niektóre maszyny wirtualne nie może automatycznie uruchomić.
Po jego ponownym uruchomieniu hosta, można zauważyć, że usługi Azure stosu nie są natychmiast dostępne.  Jest to spowodowane stosu Azure [infrastruktury maszyn wirtualnych](azure-stack-architecture.md#virtual-machine-roles) i RPs małego nieco potrwać do sprawdzania spójności, ale ostatecznie rozpocznie się automatycznie.

Ponadto tej dzierżawy maszyn wirtualnych nie automatycznie uruchomić po ponownym uruchomieniu hosta zestawu programowanie stosu Azure.  Jest to znany problem i wymaga kilku ręczne czynności w celu ich Przełącz do trybu online:

1.  Na hoście zestawu programowanie stosu Azure, należy uruchomić **Menedżera klastra trybu Failover** z Start Menu.
2.  Wybierz klaster **S Cluster.azurestack.local**.
3.  Wybierz **ról**.
4.  Maszyny wirtualne dzierżawców będą wyświetlane w *zapisane* stanu.  Po wszystkich maszyn wirtualnych infrastruktury są uruchomione, kliknij prawym przyciskiem myszy dzierżawione maszyny wirtualne i wybierz **Start** wznowienie maszyny Wirtualnej.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Usunięto niektóre maszyny wirtualne, ale również widzieć plików VHD na dysk. Jest to zachowanie oczekiwane?
Tak, to zachowanie oczekiwane. Ponieważ została zaprojektowana w ten sposób:

* Po usunięciu maszyny Wirtualnej, wirtualne dyski twarde nie są usuwane. Dyski są oddzielne zasoby w grupie zasobów.
* Konto magazynu zostaje usunięta, usunięcie jest widoczna od razu za pośrednictwem usługi Azure Resource Manager (portal programu PowerShell), ale dyski, które może on zawierać nadal są przechowywane w magazynie do momentu uruchamia wyrzucanie elementów bezużytecznych.

Jeśli widzisz "oddzielony" wirtualne dyski twarde, należy znać, jeśli są one częścią folderu dla konta magazynu, który został usunięty. Jeśli konto magazynu nie została usunięta, jest normalne, że są one nadal istnieje.

Więcej o konfigurowaniu przechowywania odzyskiwanie próg i na żądanie w [Zarządzanie kontami magazynu](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Magazyn
### <a name="storage-reclamation"></a>Odzyskiwanie pamięci masowej
Może upłynąć do 14 godzin regeneracji pojemność były wyświetlane w portalu. Odzyskiwanie miejsca zależy od różnych czynników, takich jak procent użycia plików wewnętrzny kontenera w magazynie obiektów blob bloku. W związku z tym w zależności od tego, jak dużo danych zostanie usunięty, nie ma żadnej gwarancji na ilość miejsca, które można odzyskać po uruchomieniu przez moduł Garbage Collector.

## <a name="powershell"></a>PowerShell
### <a name="resource-providers-not-registered"></a>Nie zarejestrowano dostawców zasobów
Podczas nawiązywania połączenia subskrypcji dzierżawcy przy użyciu programu PowerShell, można zauważyć, że dostawców zasobów nie są automatycznie zarejestrowany. Użyj [modułu Connect](https://github.com/Azure/AzureStack-Tools/tree/master/Connect), lub uruchomić następujące polecenie w środowiska PowerShell (po [zainstalować i połączyć](azure-stack-connect-powershell.md) dzierżawcy): 
  
       Get-AzureRMResourceProvider | Register-AzureRmResourceProvider

## <a name="cli"></a>Interfejs wiersza polecenia

* Tj interfejsu wiersza polecenia w trybie interakcyjnym `az interactive` polecenie nie jest jeszcze obsługiwane w stosie Azure.
* Aby uzyskać listę dostępnych w stosie Azure obrazów maszyny wirtualnej, użyj `az vm images list --all` polecenia zamiast `az vm image list` polecenia. Określanie `--all` option zapewnia, że odpowiedzi zwraca tylko obrazy, które są dostępne w środowisku Azure stosu. 
* Aliasy obrazu maszyny wirtualnej, które są dostępne w systemie Azure nie może być stosowane do stosu Azure. Korzystając z obrazów maszyny wirtualnej, użyć całego parametru URN (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) zamiast alias obrazu. I to URNmust jest zgodna ze specyfikacją obrazu wynikające z `az vm images list` polecenia.
* Domyślnie 2.0 interfejsu wiersza polecenia używa "Standard_DS1_v2" jako domyślny rozmiar obrazu maszyny wirtualnej. Jednak ten rozmiar nie jest jeszcze dostępna w stosie Azure, dlatego należy określić `--size` parametru jawnie, podczas tworzenia maszyny wirtualnej. Można pobrać listy rozmiarów maszyn wirtualnych, które są dostępne w stosie Azure przy użyciu `az vm list-sizes --location <locationName>` polecenia.


## <a name="windows-azure-pack-connector"></a>Łącznik usługi Windows Azure Pack
* Jeśli zmienisz hasło konta azurestackadmin po wdrożeniu zestaw deweloperski stosu Azure, nie można skonfigurować tryb usługi chmury. W związku z tym nie będzie można połączyć się środowiska docelowego pakietu Windows Azure Pack.
* Po ustawieniu trybu wielu chmury:
    * Użytkownik może pulpit nawigacyjny był widoczny tylko wtedy, gdy resetowania ustawień portalu. (W aplikacji portal użytkowników, kliknij ikonę ustawień portalu (koło zębate ikonę w prawym górnym rogu). W obszarze **przywrócić ustawienia domyślne**, kliknij przycisk **Zastosuj**.)
    * Tytuły pulpitu nawigacyjnego nie może występować. Jeśli ten problem występuje, należy ręcznie dodać je ponownie.
    * Niektóre Kafelki może być niepoprawnie wyświetlana po dodaniu ich do pulpitu nawigacyjnego. Aby rozwiązać ten problem, należy odświeżyć przeglądarkę.



