---
title: Monitorowanie połączenia sieciowe z obserwatora sieciowego Azure - Azure portal | Dokumentacja firmy Microsoft
description: Dowiedz się, jak monitorować łączność sieciową z obserwatora sieciowego Azure przy użyciu portalu Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: c7d98350dc8f66ebd4097f22b44dcbbe2653b25d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Połączenia sieciowe monitora obserwatora sieciowego Azure przy użyciu portalu Azure

Dowiedz się, jak monitorować łączność sieciową między maszyny wirtualnej platformy Azure i adres IP przy użyciu monitora połączenia. Monitor połączenia zapewnia monitorowanie na poziomie połączenia. Połączenie jest zdefiniowany jako kombinację źródłowego i docelowego adresu IP i portu. Monitor połączenia umożliwia realizację scenariuszy, takich jak monitorowanie łączności z maszyny wirtualnej w sieci wirtualnej na maszynie wirtualnej z uruchomionym programem SQL server w tej samej lub innej sieci wirtualnej, za pośrednictwem portu 1433. Monitor połączenia udostępnia połączenia czas oczekiwania, jako metrykę Azure Monitor rejestrowane co 60 sekund. On również umożliwia topologii przeskoku przeskoku i identyfikuje problemy konfiguracji wpływające na połączenie.


## <a name="prerequisites"></a>Wymagania wstępne

Przed wykonaniem kroków w tym artykule, musi spełniać następujące wymagania wstępne:

* Wystąpienia obserwatora sieciowego w regionie, który chcesz monitorować połączenia. Jeśli nie masz jeszcze jeden, możesz utworzyć jedną, wykonując kroki opisane w [utworzenia wystąpienia obserwatora sieciowego Azure](network-watcher-create.md).
* Monitorowanie z maszyny wirtualnej.
* Ma `AzureNetworkWatcherExtension` zainstalowana na maszynie wirtualnej, który chcesz monitorować połączenia z. Aby zainstalować rozszerzenie w maszynie wirtualnej systemu Windows, zobacz [rozszerzenie maszyny wirtualnej Azure sieci obserwatorów agenta dla systemu Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) i zainstalować rozszerzenie Zobacz maszyny wirtualnej systemu Linux [Azure sieci obserwatorów agenta rozszerzenie maszyny wirtualnej dla systemu Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure 

Zaloguj się w [Portalu Azure](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Utwórz monitor połączenia

Poniższych kroków spowoduje włączenie połączeń monitorowania do docelowej maszyny wirtualnej przez porty 80 i 1433:

1. Po lewej stronie portalu, wybierz **więcej usług**.
2. Zacznij pisać *obserwatora sieciowego*. Gdy **obserwatora sieciowego** pojawia się w wynikach wyszukiwania, wybierz go.
3. W obszarze **monitorowanie**, wybierz pozycję **monitor połączenia (wersja zapoznawcza)**. Funkcje w wersji zapoznawczej ma taki sam poziom niezawodności i dostępności regionu jako funkcje ogólnie wersji.
4. Wybierz **+ Dodaj**.
5. Wprowadź lub wybierz informacje dotyczące połączenia, chcesz monitorować, a następnie wybierz **Dodaj**. W tym przykładzie przedstawiono na rysunku poniżej połączenie monitorowane jest między *MultiTierApp0* i *Database0* maszyn wirtualnych:

    ![Dodawanie połączenia monitora](./media/connection-monitor/add-connection-monitor.png)

    Rozpoczyna monitorowanie. Monitor połączenia sondy co 60 sekund.

## <a name="view-connection-monitoring"></a>Monitorowanie połączenia widoku

1. Wykonać kroki od 1 do 3 w [utworzyć monitor połączenia](#create-a-connection-monitor) Aby wyświetlić połączenia monitorowania.
2. Na poniższej ilustracji przedstawiono szczegóły połączenia AppToDB(80). **Stan** jest dostępny. **Widoku wykresu** pokazuje **Średni czas obiegu** i **sondy nie powiodła się %**. Wykres przeskoku przeskoku informacje i pokazuje, że problemów wpływają na uzyskiwanie docelowego.

    ![Monitor połączenia widoku](./media/connection-monitor/view-connection-monitor.png)

3. Wyświetlanie *AppToDB(1433)* monitor pokazano na poniższej ilustracji widać, że dla tego samego źródłowych i docelowych maszyn wirtualnych, stan jest nieosiągalny za pośrednictwem portu 1433. **Widoku siatki** w tym scenariuszu zawiera informacje przeskoku przeskoku i wpływu na uzyskiwanie problem. W takim przypadku reguły NSG blokuje cały ruch na porcie 1433 na drugi przeskok.

    ![Monitor połączenia widoku](./media/connection-monitor/view-connection-monitor-2.png)

## <a name="next-steps"></a>Kolejne kroki

- Informacje o sposobie przechwytywania pakietów z alertami maszyny wirtualnej przez automatyzację [tworzenie przechwytywania pakietów wyzwolony alert](network-watcher-alert-triggered-packet-capture.md).
- Ustalić, czy niektóre ruch jest dozwolony w lub z maszyny wirtualnej za pomocą [Sprawdź przepływ IP](network-watcher-check-ip-flow-verify-portal.md).