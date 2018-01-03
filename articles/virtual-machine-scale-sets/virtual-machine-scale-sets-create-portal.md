---
title: "Utwórz skali maszyny wirtualnej systemu Linux w portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak szybko utworzyć skalowania maszyny wirtualnej w portalu Azure"
keywords: zestawy skalowania maszyny wirtualnej
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fa6bf6b34d8b93ffa9aceaf7c6112c63d4cb9f1c
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Tworzenie zestawu skalowania maszyny wirtualnej w portalu Azure
Zestaw skali maszyny wirtualnej umożliwia wdrażanie i zarządzanie nimi zestaw identyczne, automatyczne skalowanie maszyn wirtualnych. Można ręcznie skalować liczbę maszyn wirtualnych w zestawie skalowania lub definiowania reguł do skalowania automatycznego na podstawie użycia zasobów, takie jak procesor CPU, pamięci żądanie lub ruchu sieciowego. W tym artykule Rozpoczęto pobieranie tworzenia skali maszyny wirtualnej w portalu Azure. Można również utworzyć zestaw o skali [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) lub [programu Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).


## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Utwórz zestaw skali maszyny wirtualnej
Można wdrożyć zestaw z obrazu systemu Windows Server lub obrazu systemu Linux, takich jak RHEL, CentOS, Ubuntu i SLES skalowania.

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.
2. Wyszukaj *zestaw skali*, wybierz **zestaw skali maszyny wirtualnej**, a następnie wybierz pozycję **Utwórz**.
3. Wprowadź nazwę dla zestawu skalowania, takich jak *myScaleSet*.
4. Wybierz żądany typ systemu operacyjnego, takich jak *systemu Windows Server Datacenter 2016*.
5. Wprowadź nazwę grupy żądanego zasobu, takich jak *myResourceGroup*i lokalizację, takich jak *wschodnie stany USA*.
6. Wprowadź odpowiednie nazwy użytkownika, a następnie wybierz typ uwierzytelniania, który chcesz.
    - A **hasło** musi być co najmniej 12 znaków i spełniają trzy z czterech następujących wymagań złożoności: mała litera, Wielka litera, cyfra i znak specjalny. Aby uzyskać więcej informacji, zobacz [nazwy użytkownika i hasła, wymagania](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Jeśli wybierzesz obraz dysku systemu operacyjnego Linux, zamiast tego można wybrać **klucz publiczny SSH**. Podaj tylko klucz publiczny, taki jak *~/.ssh/id_rsa.pub*. Można użyć powłoki chmury Azure z portalu, aby [tworzenia i używania kluczy SSH](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Wprowadź **nazwa adresu publicznego adresu IP**, takich jak *myPublicIP*.
8. Wprowadź unikatową **etykieta nazwy domeny**, takich jak *myuniquedns*. Etykieta DNS stanowi podstawę nazwy FQDN dla usługi równoważenia obciążenia przed zestaw skali.
9. Aby potwierdzić skali Ustaw opcje, wybierz **Utwórz**.

    ![Utwórz skali maszyny wirtualnej w portalu Azure](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Połączenie z maszyną wirtualną w zestawie skalowania
Po utworzeniu skali ustawionych w portalu usługi równoważenia obciążenia jest tworzony. Reguły adres Translatora adresów sieciowych są używane do dystrybucji ruchu do wystąpienia zestawu skali dla połączenia zdalnego, takich jak połączenie RDP lub SSH.

Aby wyświetlić te NAT zasady i informacje o połączeniu dla programu skali Ustaw wystąpień:

1. Wybierz grupę zasobów, utworzony w poprzednim kroku, takie jak *myResourceGroup*.
2. Wybierz z listy zasobów z **modułu równoważenia obciążenia**, takich jak *myScaleSetLab*.
3. Wybierz **reguł ruchu przychodzącego translatora adresów Sieciowych** z menu po lewej stronie okna.

    ![Reguły NAT ruchu przychodzącego umożliwiają połączenie do wystąpień zestawu skali maszyny wirtualnej](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Można połączyć na każdej maszynie Wirtualnej w skali ustawić za pomocą tych zasad translatora adresów Sieciowych. Każde wystąpienie maszyny Wirtualnej zawiera docelowy adres IP i numer portu TCP. Na przykład, jeśli docelowy adres IP jest *104.42.1.19* i TCP port jest *50001*, należy połączyć się z wystąpieniem maszyny Wirtualnej w następujący sposób:

- Zestaw Windows skali łączyć się wystąpienia maszyny Wirtualnej z protokołem RDP na`104.42.1.19:50001`
- Zestaw skali Linux łączyć się wystąpienia maszyny Wirtualnej przy użyciu protokołu SSH w`ssh azureuser@104.42.1.19 -p 50001`

Po wyświetleniu monitu wprowadź poświadczenia, określona w poprzednim kroku, podczas tworzenia zestawu skalowania. Wystąpienia zestawu skali są regularnie maszyn wirtualnych współpracujących z normalnego. Aby uzyskać więcej informacji na temat wdrażania i uruchamiania aplikacji w skali sieci zestaw wystąpień, zobacz [wdrożyć aplikację na zestawy skalowania maszyny wirtualnej](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy już potrzebny, Usuń grupę zasobów, zestawu skalowania maszyny wirtualnej, a wszystkie powiązane zasoby. Aby to zrobić, wybierz grupę zasobów dla maszyny wirtualnej i kliknij pozycję **Usuń**.


## <a name="next-steps"></a>Kolejne kroki
W tym artykule Rozpoczęto pobieranie utworzono podstawowe skali w portalu Azure. W przypadku większej skalowalności i automatyzacji rozwiń na skalę zestaw o następujące artykuły:

- [Wdrażanie aplikacji na zestawy skalowania maszyny wirtualnej](virtual-machine-scale-sets-deploy-app.md)
- Automatycznie skalowany przy [programu Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), [interfejsu wiersza polecenia Azure](virtual-machine-scale-sets-autoscale-cli.md), lub [portalu Azure](virtual-machine-scale-sets-autoscale-portal.md)
- [Użycie automatycznych uaktualnień systemu operacyjnego dla wystąpień zestawu skalowania maszyn wirtualnych](virtual-machine-scale-sets-automatic-upgrade.md)
