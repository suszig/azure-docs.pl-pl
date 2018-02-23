---
title: "Monitorowanie połączenia sieciowe z obserwatora sieciowego Azure - Azure portal | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak monitorować łączność sieciową z obserwatora sieciowego Azure przy użyciu portalu Azure."
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: beaa458d727a05eccf496933deb3c998828868cd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Połączenia sieciowe monitora obserwatora sieciowego Azure przy użyciu portalu Azure

Dowiedz się, jak monitorować łączność sieciową między maszyny wirtualnej platformy Azure i adres IP przy użyciu monitora połączenia. Adres IP można przypisać do innej zasobów platformy Azure lub zasobu internetowego lub lokalnie.

## <a name="prerequisites"></a>Wymagania wstępne

Przed wykonaniem kroków w tym artykule, musi spełniać następujące wymagania wstępne:

* Wystąpienia obserwatora sieciowego w regionie, który chcesz monitorować połączenia. Jeśli nie masz jeszcze jeden, możesz utworzyć jedną, wykonując kroki opisane w [utworzenia wystąpienia obserwatora sieciowego Azure](network-watcher-create.md).
* Monitorowanie z maszyny wirtualnej.
* Ma `AzureNetworkWatcherExtension` zainstalowana na maszynie wirtualnej, który chcesz monitorować połączenia z. Aby zainstalować rozszerzenie w maszynie wirtualnej systemu Windows, zobacz [rozszerzenie maszyny wirtualnej Azure sieci obserwatorów agenta dla systemu Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) i zainstalować rozszerzenie Zobacz maszyny wirtualnej systemu Linux [Azure sieci obserwatorów agenta rozszerzenie maszyny wirtualnej dla systemu Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure 

Zaloguj się w [Portalu Azure](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Utwórz monitor połączenia

1. Po lewej stronie portalu, wybierz **więcej usług**.
2. Zacznij pisać *obserwatora sieciowego*. Gdy **obserwatora sieciowego** pojawia się w wynikach wyszukiwania, wybierz go.
3. W obszarze **monitorowanie**, wybierz pozycję **monitor połączenia (wersja zapoznawcza)**. Funkcje w wersji zapoznawczej ma taki sam poziom niezawodności i dostępności regionu jako funkcje ogólnie wersji.
4. Wybierz **+ Dodaj**.
5. Wprowadź lub wybierz odpowiednie informacje dla połączenia, chcesz monitorować, a następnie wybierz **Dodaj**. W tym przykładzie połączenie między *myVmSource* i *myVmDestination* maszyn wirtualnych jest monitorowany za pośrednictwem portu 80.
    
    |  Ustawienie                                 |  Wartość               |
    |  -------------------------------------   |  ------------------- |
    |  Name (Nazwa)                                    |  myConnectionMonitor |
    |  Źródłowej maszyny wirtualnej                  |  myVmSource          |
    |  Port źródłowy                             |                      |
    |  Docelowy, wybierz maszynę wirtualną   |  myVmDestination     |
    |  Port docelowy                        |  80                  |

6. Rozpoczyna monitorowanie. Monitor połączenia sondy co 60 sekund.
7. Monitor połączenia pokazuje obiegu Średni czas procent badania i które się nie powieść. Mogą wyświetlać dane monitorowania w siatce lub wykres.

## <a name="next-steps"></a>Kolejne kroki

- Informacje o sposobie przechwytywania pakietów z alertami maszyny wirtualnej przez automatyzację [tworzenie przechwytywania pakietów wyzwolony alert](network-watcher-alert-triggered-packet-capture.md).

- Ustalić, czy niektóre ruch jest dozwolony w lub z maszyny wirtualnej za pomocą [Sprawdź przepływ IP](network-watcher-check-ip-flow-verify-portal.md).