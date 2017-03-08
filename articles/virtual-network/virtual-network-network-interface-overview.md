---
title: Interfejsy sieciowe na platformie Azure | Microsoft Docs
description: "Dowiedz się o interfejsach sieciowych platformy Azure i poznaj sposoby ich używania razem z maszynami wirtualnymi."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f58b503f-18bf-4377-aa63-22fc8a96e4be
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: 395cff80b3f97b6340e15f370c13f783e2f5dde3
ms.lasthandoff: 02/28/2017


---
# <a name="what-are-network-interfaces"></a>Co to są interfejsy sieciowe?

Interfejs sieciowy (karta sieciowa) to wzajemne połączenie między maszyną wirtualną (VM) i podstawową siecią oprogramowania. W tym artykule opisano interfejs sieciowy oraz sposób użycia w modelu wdrażania przy użyciu usługi Azure Resource Manager.

Firma Microsoft zaleca wdrażanie nowych zasobów za pomocą modelu wdrażania przy użyciu usługi Resource Manager, ale możesz również wdrażać maszyny wirtualne z łącznością sieciową w [klasycznym](virtual-network-ip-addresses-overview-classic.md) modelu wdrażania. Jeśli znasz klasyczny model, istnieją ważne różnice dotyczące sieci maszyny wirtualnej w modelu wdrażania przy użyciu usługi Resource Manager. Dowiesz się więcej o różnicach po przeczytaniu artykułu [Virtual machine networking - Classic](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) (Sieć maszyny wirtualnej — klasyczna).

Na platformie Azure interfejs sieciowy:

1. Jest zasobem, który można utworzyć, usunąć i który ma swoje własne konfigurowalne ustawienia.
2. Musi być podłączony do jednej podsieci w jednej sieci wirtualnej platformy Azure, gdy jest on tworzony. Jeśli nie znasz sieci wirtualnych, dowiesz się więcej o nich po przeczytaniu artykułu [Omówienie usługi Virtual Network](virtual-networks-overview.md). Karta sieciowa musi być podłączona do sieci wirtualnej, która znajduje się w tej samej [lokalizacji](https://azure.microsoft.com/regions) i [subskrypcji](../azure-glossary-cloud-terminology.md#subscription) platformy Azure co karta sieciowa. Po utworzeniu karty sieciowej możesz zmienić podsieć, z którą jest ona połączona, ale nie możesz zmienić sieci wirtualnej, z którą jest połączona.
3. Ma przypisaną nazwę, której nie można zmienić po utworzeniu karty sieciowej. Nazwa musi być unikatowa w obrębie [grupy zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups) platformy Azure, ale nie musi być unikatowa w ramach subskrypcji, w lokalizacji platformy Azure, w której została utworzona, lub sieci wirtualnej, z którą jest połączona. W ramach subskrypcji platformy Azure jest zazwyczaj tworzonych kilka kart sieciowych. Zalecane jest, aby zamiast używania nazw domyślnych wymyślić konwencję nazewnictwa, która ułatwi zarządzanie wieloma kartami sieciowymi. Aby zapoznać się z sugestiami, zobacz artykuł [Zalecane konwencje nazewnictwa dla zasobów platformy Azure](../guidance/guidance-naming-conventions.md).
4. Może być podłączona do maszyny wirtualnej, ale można ją połączyć tylko z jedną maszyną wirtualną, która istnieje w tej samej lokalizacji co karta sieciowa.
5. Ma adres MAC, który jest trwały dla karty sieciowej tak długo, jak długo pozostaje ona podłączona do maszyny wirtualnej. Adres MAC jest trwały bez względu na to, czy maszyna wirtualna zostanie ponownie uruchomiona (w ramach systemu operacyjnego) lub zatrzymana (cofnięcie przydziału) i uruchomiona za pomocą witryny Azure Portal, programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure. Jeśli karta sieciowa zostanie odłączona od maszyny wirtualnej i dołączona do innej maszyny wirtualnej, otrzyma inny adres MAC. Jeśli karta sieciowa zostanie usunięta, adres MAC zostanie przypisany do innych kart sieciowych.
6. Musi mieć przypisany podstawowy **prywatny** statyczny lub dynamiczny adres IP *IPv4*.
7. Może mieć powiązany jeden lub więcej zasobów publicznych adresów IP. Aby uzyskać więcej informacji, zapoznaj się z dokumentacją [Wiele adresów IP dla karty sieciowej](virtual-network-multiple-ip-addresses-portal.md).
8. Obsługuje przyspieszoną sieć za pomocą wirtualizacji we/wy z jednym elementem głównym (SR-IOV) dla określonych rozmiarów maszyny wirtualnej z uruchomioną określoną wersją systemu operacyjnego Microsoft Windows Server. Aby dowiedzieć się więcej o tej funkcji WERSJI ZAPOZNAWCZEJ, przeczytaj artykuł [Accelerated networking for a virtual machine](virtual-network-accelerated-networking-powershell.md) (Przyspieszona sieć dla maszyny wirtualnej).
9. Może odbierać ruch nieprzeznaczony do prywatnych adresów IP przypisanych do niej, jeśli przesyłanie dalej IP jest włączone dla karty sieciowej. Jeśli na przykład na maszynie wirtualnej działa oprogramowanie zapory, określa ono trasy pakietów, które nie są przeznaczone do własnych adresów IP. Na maszynie wirtualnej nadal musi być uruchomione oprogramowanie umożliwiające routing lub przesyłanie dalej ruchu, ale w tym celu przesyłanie dalej IP musi być włączone dla karty sieciowej.
10. Często jest tworzony w tej samej grupie zasobów, do której jest dołączona maszyna wirtualna lub w tej samej sieci wirtualnej, do której jest podłączona, chociaż nie jest to wymagane.

## <a name="vms-with-multiple-network-interfaces"></a>Maszyny wirtualne z wieloma interfejsami sieciowymi
Do maszyny wirtualnej można dołączyć wiele kart sieciowych, ale robiąc to, należy pamiętać o poniższym:  

* Rozmiar maszyny wirtualnej musi obsługiwać wiele kart sieciowych. Aby dowiedzieć się więcej o tym, które rozmiary maszyn wirtualnych obsługują wiele kart sieciowych, przeczytaj artykuły [Windows Server VM sizes](../virtual-machines/virtual-machines-windows-sizes.md) (Rozmiary maszyn wirtualnych w systemie Windows Server) lub [Linux VM sizes](../virtual-machines/virtual-machines-linux-sizes.md) (Rozmiary maszyn wirtualnych w systemie Linux).
* Maszyna wirtualna musi zostać utworzona z co najmniej dwoma kartami sieciowymi. Jeśli maszyna wirtualna została utworzona tylko z jedną kartą sieciową, nawet jeśli rozmiar maszyny wirtualnej obsługuje więcej niż jedną, nie możesz dołączyć dodatkowych kart sieciowych do maszyny wirtualnej po jej utworzeniu. Jeżeli maszyna wirtualna została utworzona z przynajmniej dwoma kartami sieciowymi, możesz dołączyć dodatkowe karty sieciowe do maszyny wirtualnej po jej utworzeniu pod warunkiem, że rozmiar maszyny wirtualnej obsługuje więcej niż dwie karty sieciowe.  
* Od maszyny wirtualnej możesz odłączyć dodatkowe karty sieciowe (podstawowej karty sieciowej nie można odłączyć), jeśli maszyna wirtualna ma dołączone przynajmniej trzy karty sieciowe. Nie możesz odłączyć kart sieciowych, jeśli do maszyny wirtualnej są dołączone co najwyżej dwie karty sieciowe.  
* Kolejność kart sieciowych wewnątrz maszyny wirtualnej będzie losowa i może również ulec zmianie między aktualizacjami infrastruktury platformy Azure. Jednak adresy IP i odpowiednie adresy Ethernet MAC pozostaną takie same. Na przykład załóżmy, że system operacyjny identyfikuje kartę sieciową Azure NIC1 jako Eth1. Interfejs sieciowy Eth1 ma adres IP 10.1.0.100 i adres MAC 00-0D-3A-B0-39-0D. Po aktualizacji i ponownym uruchomieniu infrastruktury platformy Azure system operacyjny może teraz zidentyfikować kartę sieciową Azure NIC1 jako Eth2, ale adresy IP i MAC będą takie same, jak wtedy, gdy system operacyjny zidentyfikował kartę sieciową Azure NIC1 jako Eth1. Jeśli ponowne uruchomienie jest inicjowane przez klienta, kolejność kart sieciowych pozostanie taka sama w ramach systemu operacyjnego.  
* Jeśli maszyna wirtualna jest członkiem [zestawu dostępności](../azure-glossary-cloud-terminology.md#availability-set), wszystkie maszyny wirtualne w zestawie dostępności muszą mieć jedną kartę sieciową albo wiele kart sieciowych. Jeśli maszyny wirtualne mają wiele kart sieciowych, liczba kart, które ma każda z nich, nie musi być taka sama, dopóki każda maszyna wirtualna ma co najmniej dwie karty sieciowe.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak utworzyć maszynę wirtualną z jedną kartą sieciową, czytając artykuł [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
* Dowiedz się, jak utworzyć maszynę wirtualną z wieloma kartami sieciowymi, czytając artykuł [Wdrażanie maszyny wirtualnej z wieloma kartami sieciowymi](virtual-network-deploy-multinic-arm-ps.md).
* Dowiedz się, jak utworzyć kartę sieciową z wieloma konfiguracjami IP, czytając artykuł [Wiele adresów IP dla maszyn wirtualnych platformy Azure](virtual-network-multiple-ip-addresses-powershell.md).


