---
title: "Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą portalu Azure | Dokumentacja firmy Microsoft"
description: "Jak utworzyć publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP z portalu Azure"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: kumud
ms.openlocfilehash: d09115d7b01b72476ebc9eb883a43aa4c59d2e3b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą portalu Azure

W tym artykule opisano przez proces tworzenia publiczny [standardowe usługi równoważenia obciążenia](https://aka.ms/azureloadbalancerstandard) z strefowo nadmiarowy frontonu przy użyciu adresu publicznego adresu IP standardowa.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Zarejestruj się, aby Podgląd standardowe strefy dostępności, standardowe usługi równoważenia obciążenia i publicznego adresu IP
 
>[!NOTE]
[Standardowy SKU usługi równoważenia obciążenia](https://aka.ms/azureloadbalancerstandard) jest obecnie w przeglądzie. W okresie obowiązywania wersji zapoznawczej ta funkcja może nie oferować dostępności i niezawodności na tym samym poziomie, co funkcje w wersji ogólnodostępnej. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Użyj ogólnie dostępna [podstawowy SKU usługi równoważenia obciążenia](load-balancer-overview.md) dla usług produkcji. 

> [!NOTE]
> Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Przed wybraniem strefy lub strefowo nadmiarowy opcję frontonu publicznego adresu IP usługi równoważenia obciążenia, należy najpierw wykonać kroki w [zarejestrować w wersji zapoznawczej stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do witryny Azure Portal pod adresem https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Tworzenie modułu równoważenia obciążenia nadmiarowe strefy

1. Przejdź w przeglądarce do witryny Azure Portal ([http://portal.azure.com](http://portal.azure.com)) i zaloguj się przy użyciu konta Azure.
2. Po lewej stronie górnej części ekranu, wybierz **Utwórz zasób** > **sieci** > **usługi równoważenia obciążenia.**
3. W ** Utwórz moduł równoważenia obciążenia w obszarze **nazwa** typu **myPublicLB**.
4. W obszarze **Typ** wybierz opcję **Publiczny**.
5. W obszarze jednostka SKU, wybierz **Standard (wersja zapoznawcza)**.
6. Kliknij przycisk **publicznego adresu IP**, kliknij przycisk **Utwórz nowy**na **tworzenie publicznego adresu IP** strony, w obszarze nazwy typu **myPublicIPStandard**, i Aby uzyskać **dostępność strefy (wersja zapoznawcza)**, wybierz pozycję **Strefowo nadmiarowy**.
7. W obszarze **lokalizacji**, wybierz pozycję **Wschodnią US2**, a następnie kliknij przycisk **OK**. Rozpocznie się wdrażanie modułu równoważenia obciążenia. Potrwa to kilka minut.

    ![Utwórz strefowo nadmiarowy standardowe usługi równoważenia obciążenia z portalu Azure](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Kolejne kroki
- Dowiedz się, jak [tworzenie publicznego adresu IP w strefie dostępności](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



