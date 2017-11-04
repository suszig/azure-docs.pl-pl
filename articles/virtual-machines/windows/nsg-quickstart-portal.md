---
title: "Otwieranie portów z maszyną wirtualną przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak otworzyć port / utworzyć punktu końcowego maszyny Wirtualnej systemu Windows przy użyciu modelu wdrażania Menedżera zasobów w portalu Azure"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: 33bc0be0aeae6d0276fd8999b9ac0a010e3067ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Jak otworzyć porty do maszyny wirtualnej z portalu Azure
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Szybkie polecenia
Możesz również [wykonaj te czynności przy użyciu programu Azure PowerShell](nsg-quickstart-powershell.md).

Najpierw należy utworzyć grupy zabezpieczeń sieci. Wybierz grupę zasobów w portalu, wybierz **Dodaj**, następnie wyszukaj i wybierz **sieciowej grupy zabezpieczeń**:

![Dodaj grupę zabezpieczeń sieci](./media/nsg-quickstart-portal/add-nsg.png)

Wprowadź nazwę dla swojej grupy zabezpieczeń sieci, wybierz lub Utwórz grupę zasobów, a następnie wybierz lokalizację. Wybierz **Utwórz** po zakończeniu:

![Utwórz grupę zabezpieczeń sieci](./media/nsg-quickstart-portal/create-nsg.png)

Wybierz nową grupę zabezpieczeń sieci. Wybierz "Reguły zabezpieczeń ruchu przychodzącego", a następnie wybierz **Dodaj** przycisk, aby utworzyć regułę:

![Dodaj regułę ruchu przychodzącego](./media/nsg-quickstart-portal/add-inbound-rule.png)

Wybierz popularne **usługi** z menu rozwijanego, takich jak *HTTP*. Możesz też wybrać *niestandardowy* zapewnienie określonego portu do użycia. W razie potrzeby można zmienić priorytet lub nazwy. Priorytet ma wpływ na kolejność, w której reguły są stosowane - niższa wartość liczbową, wcześniejszej jest stosowana reguła. Możesz też wybrać **zaawansowane** u góry tego ekranu, aby wprowadzić określonego źródłowego adresu IP bloku lub portu zakres, np. Gdy wszystko będzie gotowe, wybierz **OK** Aby utworzyć regułę:

![Utwórz regułę ruchu przychodzącego](./media/nsg-quickstart-portal/create-inbound-rule.png)

Z ostatnim krokiem jest do skojarzenia z sieciową grupę zabezpieczeń z podsieci lub w konkretnym interfejsie sieciowym. Umożliwia kojarzenie sieciową grupę zabezpieczeń z podsiecią. Wybierz **podsieci**, a następnie wybierz **skojarzyć**:

![Skojarzenia sieciowej grupy zabezpieczeń z podsiecią](./media/nsg-quickstart-portal/associate-subnet.png)

Wybierz sieci wirtualnej, a następnie wybierz odpowiednie podsieci:

![Kojarzenie grupy zabezpieczeń sieci z obsługą sieci wirtualnej](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Teraz utworzono grupę zabezpieczeń sieci, utworzyć regułę ruchu przychodzącego, która zezwala na ruch na porcie 80, a jego skojarzony z podsiecią. Wszystkie maszyny wirtualne, gdy nawiązujesz połączenie z tym podsieci są dostępne na porcie 80.

## <a name="more-information-on-network-security-groups"></a>Więcej informacji na temat grup zabezpieczeń sieci
Szybkie polecenia tutaj pozwala rozpocząć pracę z ruchem przepływać do maszyny Wirtualnej. Grupy zabezpieczeń sieci zapewniają wiele funkcje i poziom szczegółowości kontrolowania dostępu do zasobów. Możesz przeczytać dodatkowe informacje [Tworzenie grupy zabezpieczeń sieci i listy ACL zasady tutaj](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Dla aplikacji sieci web wysokiej dostępności należy umieszczać maszyny wirtualne za usługą równoważenia obciążenia Azure. Moduł równoważenia obciążenia dystrybuuje ruch do maszyn wirtualnych z sieciową grupą zabezpieczeń, który umożliwia filtrowanie ruchu. Aby uzyskać więcej informacji, zobacz [jak załadować saldo maszyn wirtualnych systemu Linux na platformie Azure, aby utworzyć aplikację wysokiej dostępności](tutorial-load-balancer.md).

## <a name="next-steps"></a>Następne kroki
W tym przykładzie utworzono prosta Reguła zezwalająca na ruch HTTP. Można znaleźć informacje dotyczące tworzenia środowisk bardziej szczegółowe zawierają następujące artykuły:

* [Omówienie usługi Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Co to jest sieciowa grupa zabezpieczeń?](../../virtual-network/virtual-networks-nsg.md)