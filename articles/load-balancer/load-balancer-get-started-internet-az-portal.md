---
title: Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą portalu Azure | Dokumentacja firmy Microsoft
description: Jak utworzyć publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP z portalu Azure
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 10a264609469245d4743886b58730304da3df7bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą portalu Azure

W tym artykule opisano przez proces tworzenia publiczny [standardowe usługi równoważenia obciążenia](https://aka.ms/azureloadbalancerstandard) z strefowo nadmiarowy frontonu przy użyciu adresu publicznego adresu IP standardowa.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="register-for-availability-zones-preview"></a>Zarejestruj się, aby Podgląd stref dostępności
 
Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do portalu Azure pod adresem https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Tworzenie modułu równoważenia obciążenia nadmiarowe strefy

1. W przeglądarce przejdź do portalu Azure: [ http://portal.azure.com ](http://portal.azure.com) i zaloguj się za pomocą konta platformy Azure.
2. Po lewej stronie górnej części ekranu, wybierz **Utwórz zasób** > **sieci** > **usługi równoważenia obciążenia.**
3. W ** Utwórz moduł równoważenia obciążenia w obszarze **nazwa** typu **myPublicLB**.
4. W obszarze **Typ** wybierz opcję **Publiczny**.
5. W obszarze jednostka SKU, wybierz **Standard (wersja zapoznawcza)**.
6. Kliknij przycisk **publicznego adresu IP**, kliknij przycisk **Utwórz nowy**na **tworzenie publicznego adresu IP** strony, w obszarze nazwy typu **myPublicIPStandard**, i Aby uzyskać **dostępność strefy (wersja zapoznawcza)**, wybierz pozycję **Strefowo nadmiarowy**.
7. W obszarze **lokalizacji**, wybierz pozycję **Wschodnią US2**, a następnie kliknij przycisk **OK**. Rozpocznie się wdrażanie modułu równoważenia obciążenia. Potrwa to kilka minut.

    ![Utwórz strefowo nadmiarowy standardowe usługi równoważenia obciążenia z portalu Azure](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Kolejne kroki
- Dowiedz się, jak [tworzenie publicznego adresu IP w strefie dostępności](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



