---
title: "Sprawdź łączność z obserwatora sieciowego Azure - Azure portal | Dokumentacja firmy Microsoft"
description: "Ta strona wyjaśniono, jak użyć Sprawdź łączność z obserwatora sieciowego przy użyciu portalu Azure"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: jdial
ms.openlocfilehash: 1f19da71731039e1a39c4440f925b1369886a993
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-the-azure-portal"></a>Sprawdź łączność z obserwatora sieciowego Azure przy użyciu portalu Azure

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [Program PowerShell](network-watcher-connectivity-powershell.md)
> - [Interfejs wiersza polecenia 2.0](network-watcher-connectivity-cli.md)
> - [Interfejs API Azure REST](network-watcher-connectivity-rest.md)

Dowiedz się, jak używać łączności, aby sprawdzić, czy można nawiązać bezpośrednie połączenie TCP z maszyny wirtualnej do danego punktu końcowego.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym artykule przyjęto założenie, że masz następujące zasoby:

* Wystąpienie obserwatora sieciowego w regionie, aby sprawdzić łączność.

* Sprawdź łączność z maszyn wirtualnych.

> [!IMPORTANT]
> Sprawdź łączność wymaga rozszerzenia maszyny wirtualnej `AzureNetworkWatcherExtension`. Instalowanie rozszerzenia na maszynie Wirtualnej systemu Windows można znaleźć [rozszerzenie maszyny wirtualnej Azure sieci obserwatorów agenta dla systemu Windows](../virtual-machines/windows/extensions-nwa.md) i dla maszyny Wirtualnej systemu Linux, odwiedź [rozszerzenie maszyny wirtualnej Azure sieci obserwatorów agenta dla systemu Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="check-connectivity-to-a-virtual-machine"></a>Sprawdź połączenie z maszyną wirtualną

W tym przykładzie służy do sprawdzania łączności do docelowej maszyny wirtualnej za pośrednictwem portu 80.

Przejdź do Twojej obserwatora sieciowego i kliknij **sprawdzenia łączności z serwerem (wersja zapoznawcza)**. Wybierz maszynę wirtualną, aby sprawdzić łączność z. W **docelowego** wybierz **wybierz maszynę wirtualną** oraz wybrać poprawne maszyny wirtualnej i portu do testowania.

Po kliknięciu **Sprawdź**, są sprawdzane w łączności między maszynami wirtualnymi na określony port. W tym przykładzie docelowej maszyny Wirtualnej jest nieosiągalny, lista przeskoków są wyświetlane.

![Wyniki sprawdzania łączności dla maszyny wirtualnej][1]

## <a name="check-remote-endpoint-connectivity"></a>Sprawdź łączność zdalny punkt końcowy

Aby sprawdzić łączność i czas oczekiwania do zdalnego punktu końcowego, wybierz **ręcznie określ** przycisku radiowego w **docelowego** sekcji należy wprowadzić adres url i numer portu i kliknij przycisk **Sprawdź**.  Służy to do zdalnego punktów końcowych, takie jak punktów końcowych witryn sieci Web i magazynu.

![Wyniki sprawdzania łączności dla witryny sieci web][2]

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak można zautomatyzować przechwytywania pakietów z alertami maszyny wirtualnej, wyświetlając [utworzyć przechwytywania alertów wyzwalanych pakietów](network-watcher-alert-triggered-packet-capture.md)

Znajdź, jeśli niektórych ruch jest dozwolony w lub z maszyny Wirtualnej, odwiedzając [Sprawdź przepływ Sprawdź IP](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
