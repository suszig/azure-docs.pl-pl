---
title: "Tworzenie zestawu skalowania maszyn wirtualnych przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Wdrażanie zestawów skalowania za pomocą portalu Azure."
keywords: zestawy skalowania maszyny wirtualnej
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc52368a1a14ad094e7ab9180b90a9aa4ccdb6d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Tworzenie zestawu skalowania maszyn wirtualnych przy użyciu portalu Azure
Ten samouczek pokazuje, jak łatwo jest utworzenie zestawu skalowania maszyn wirtualnych w ciągu kilku minut za pomocą portalu Azure. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="choose-the-vm-image-from-the-marketplace"></a>Wybieranie obrazu maszyny wirtualnej z portalu Marketplace
Z portalu możesz z łatwością wdrożyć skali ustawiony za pomocą CentOS, CoreOS Debian, Ubuntu Server, inne obrazy systemu Linux i obrazów systemu Windows Server.

Po pierwsze, przejdź do [portalu Azure](https://portal.azure.com) w przeglądarce sieci web. Kliknij przycisk **nowy**, wyszukiwania **zestaw skali**, a następnie wybierz **zestaw skali maszyny wirtualnej** wpis:

![wyszukiwania portalu zestawu skalowania maszyny wirtualnej platformy Azure](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>Tworzenie zestawu skalowania
Teraz możesz użyć domyślnych ustawień i szybko utworzyć zestaw skali.

* Wprowadź nazwę dla zestawu skalowania.  
Ta nazwa staje się podstawowej nazwy FQDN usługi równoważenia obciążenia przed zestaw skalowania, upewnij się, że nazwa jest unikatowa w obrębie wszystkie platformy Azure.

* Wybierz żądany typ systemu operacyjnego.

* Wprowadź odpowiednie nazwy użytkownika, a następnie wybierz typ uwierzytelniania, który chcesz.  
Jeśli wybierzesz hasło musi być co najmniej 12 znaków i spełniają trzy z czterech następujących wymagań złożoności: mała litera, Wielka litera, cyfra i znak specjalny. Więcej informacji na temat [wymagań dotyczących nazwy użytkownika i hasła](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Jeśli wybierzesz **klucz publiczny SSH**, mieć się, że tylko Wklej klucz publiczny *nie* klucz prywatny:

* Wybierz **tak** lub **nr** do **włączyć skalowanie poza 100 wystąpień**.  
Jeśli tak, ustaw skali może obejmować wieloma grupami umieszczania. Aby uzyskać więcej informacji, zobacz [tej dokumentacji](./virtual-machine-scale-sets-placement-groups.md).

* Upewnij się wybrać odpowiednie **wystąpienie rozmiar**.  
Aby uzyskać więcej informacji na temat rozmiarów maszyn wirtualnych, zobacz [rozmiarów maszyn wirtualnych systemu Windows](..\virtual-machines\windows\sizes.md) lub [rozmiarów maszyn wirtualnych systemu Linux](..\virtual-machines\linux\sizes.md).

* Wprowadź nazwę grupy żądanego zasobu i lokalizacji.  
Jeśli Twoim regionie i **wystąpienie rozmiar** obsługuje stref dostępności **stref dostępności** pole jest włączone. Aby uzyskać więcej informacji o dostępności strefy, zobacz [omówienie](../availability-zones/az-overview.md) artykułu.

* Wprowadź etykiety nazwa żądanej domeny (podstawowa nazwa FQDN modułu równoważenia obciążenia przed zestaw skali).  
Etykieta musi być unikatowa we wszystkich Azure.

* Wybierz obraz dysku odpowiedni system operacyjny, liczba wystąpień i rozmiaru maszyny.

* Wybierz typ żądany dysk: zarządzane lub niezarządzane.  
Aby uzyskać więcej informacji, zobacz [tej dokumentacji](./virtual-machine-scale-sets-managed-disks.md). Jeśli zdecydujesz się na ma zestaw skalowania span wiele grup umieszczania, tej opcji nie będzie dostępne, ponieważ jest wymagane dla zestawów skalowania zakresu umieszczania grupy dysków zarządzanych w.

* Włącz lub wyłącz skalowania automatycznego i konfigurować, jeśli włączona.

![Monituj utworzyć portalu zestawu skali maszyny wirtualnej platformy Azure](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>Połączenie z maszyną wirtualną w zestawie skalowania
Jeśli zdecydujesz się ograniczyć zestaw do grupy umieszczania pojedynczego użytkownika skalowania, zestaw skalowania jest wdrażana przy użyciu reguł NAT skonfigurowany, aby można było nawiązać połączenia z łatwością zestaw skalowania (Jeśli nie, do nawiązania połączenia w zestawie skalowania, prawdopodobnie należy utworzyć jumpbox w tej samej sieci wirtualnej jako zestaw skalowania maszyn wirtualnych). Aby je wyświetlić, przejdź do `Inbound NAT Rules` karcie usługi równoważenia obciążenia dla zestawu skalowania:

![reguły nat portalu zestawu skalowania maszyny wirtualnej platformy Azure](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

Można połączyć na każdej maszynie Wirtualnej w skali ustawić za pomocą tych zasad translatora adresów Sieciowych. Na przykład dla zestawu skalowania systemu Windows, jeśli brak reguły NAT na przychodzący port 50000, można łączysz na komputerze za pomocą protokołu RDP na `<load-balancer-ip-address>:50000`. Dla zestawu skalowania Linux, można połączyć za pomocą polecenia `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Następne kroki
Dokumentacja na temat wdrażania skalowania ustawia z interfejsu wiersza polecenia, można znaleźć [tej dokumentacji](virtual-machine-scale-sets-cli-quick-create.md).

Dla dokumentacji na temat wdrażania skalowania ustawia z programu PowerShell, zobacz [tej dokumentacji](virtual-machine-scale-sets-windows-create.md).

Dla dokumentacji na temat wdrażania skalowania zestawy w programie Visual Studio, zobacz [tej dokumentacji](virtual-machine-scale-sets-vs-create.md).

Dokumentacja ogólna, zapoznaj się z [strony Przegląd dokumentacji dla zestawów skalowania](virtual-machine-scale-sets-overview.md).

Aby uzyskać ogólne informacje, zapoznaj się z [strony głównej docelowej dla zestawów skalowania](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

