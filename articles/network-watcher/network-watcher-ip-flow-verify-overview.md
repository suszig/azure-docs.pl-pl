---
title: "Wprowadzenie do przepływu IP Zweryfikuj obserwatora sieciowego Azure | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera omówienie sieci IP obserwatora przepływu Sprawdź możliwości"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: b2ad45e76320c59d18dce7b39166679801b4170a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Wprowadzenie do przepływu IP Zweryfikuj obserwatora sieciowego Azure

Przepływ IP Sprawdź kontroli, jeśli pakiet jest dozwolony lub odrzucany do lub z maszyny wirtualnej na podstawie informacji 5-elementowej. Te informacje składa się z kierunku, protokół lokalny adres IP, zdalny adres IP, portu lokalnego i portu zdalnego. Jeśli pakiet jest zabroniony przez grupę zabezpieczeń, zwracana jest nazwa reguły, z którego pakiet. Podczas gdy można wybrać dowolny źródłowy lub docelowy adres IP, funkcja ta ułatwia ona administratorom szybkie diagnozowanie problemów z łącznością z lub do Internetu i z lub do środowiska lokalnego.

Sprawdź przepływ IP jest przeznaczony dla interfejsu sieciowego maszyny wirtualnej. Przepływ ruchu jest następnie weryfikowany na podstawie ustawień skonfigurowanych do lub z tym interfejs sieciowy. Ta funkcja jest przydatna w potwierdzaniu blokuje ruch przychodzący lub wychodzący z maszyny wirtualnej lub jeśli reguły do grupy zabezpieczeń sieci.

Sprawdź wystąpienia obserwatora sieciowego musi być utworzony we wszystkich regionach, które zamierzasz uruchomić przepływ IP. Obserwatora sieciowego jest usługą regionalnych i może być przeprowadzony tylko na zasobów w tym samym regionie. To nie wpływa Sprawdź wyniki przepływu IP trasy skojarzona z kartą Sieciową nadal zostanie zwrócony.

![1][1]

## <a name="next-steps"></a>Następne kroki

Można znaleźć w artykule, aby dowiedzieć się, jeśli pakiet jest dozwolony lub niedozwolony dla określonej maszyny wirtualnej za pośrednictwem portalu. [Sprawdź, czy ruch jest dozwolony na maszynie Wirtualnej z przepływem Sprawdź IP przy użyciu portalu](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












