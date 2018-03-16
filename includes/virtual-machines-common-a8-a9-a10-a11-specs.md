---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ee32886ddb74bdbbe0f240310629c8ef26230a68
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
## <a name="deployment-considerations"></a>Zagadnienia dotyczące wdrażania
* **Subskrypcja platformy Azure** — Aby wdrożyć więcej niż kilka wystąpień obliczeniowych, należy wziąć pod uwagę z subskrypcji lub inne opcje zakupu. Jeśli używasz [bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/), możesz użyć ograniczonej liczby rdzeni obliczeniowych platformy Azure.

* **Cennik i dostępności** -rozmiarów tych maszyn wirtualnych są oferowane tylko w standardowej warstwie cenowej. Sprawdź [produkty dostępne według regionu] (https://azure.microsoft.com/regions/services/) dostępności w regionach platformy Azure. 
* **Limit przydziału rdzeni** — może być konieczne zwiększyć limit przydziału rdzeni w subskrypcji platformy Azure z wartości domyślnej. Subskrypcją również może ograniczyć liczbę rdzeni, którą można wdrożyć w niektórych rodzin rozmiar maszyny Wirtualnej, w tym serię H. Aby zażądać zwiększenia limitu przydziału [otwarcia żądania pomocy technicznej online klienta](../articles/azure-supportability/how-to-create-azure-support-request.md) bez dodatkowych opłat. (Domyślne limity może się różnić w zależności od kategorii subskrypcji).
  
  > [!NOTE]
  > Jeśli masz wymagana pojemność na dużą skalę, skontaktuj się z pomocą techniczną platformy Azure. Przydziały Azure są środki ogranicza nie gwarantuje pojemności. Niezależnie od limitu przydziału naliczane są tylko opłaty dla rdzeni użycie.
  > 
  > 
* **Sieć wirtualna** — moduł Azure [sieci wirtualnej](https://azure.microsoft.com/documentation/services/virtual-network/) nie trzeba używać wystąpień obliczeniowych. Jednak w przypadku wielu wdrożeń należy co najmniej oparte na chmurze sieci wirtualnej platformy Azure lub połączenie lokacja lokacja Jeśli musisz uzyskiwać dostęp do zasobów lokalnych. W razie potrzeby utwórz nową sieć wirtualną, aby wdrożyć wystąpienia. Dodawanie obliczeniowych maszyn wirtualnych do sieci wirtualnej w grupie koligacji nie jest obsługiwane.
* **Zmiana rozmiaru** — ze względu na ich specjalne sprzętu można rozmiaru wystąpień obliczeniowych w ramach tej samej rodziny rozmiaru (serii H lub obliczeniowych A-series). Na przykład można zmienić tylko wirtualna H-series, z jedną rozmiar z serii H do innego. Ponadto zmiana rozmiaru od rozmiaru nie obliczeniowych do rozmiaru obliczeniowych nie jest obsługiwane.  

## <a name="rdma-capable-instances"></a>Z funkcją RDMA wystąpień
Podzbiór wystąpień obliczeniowych (H16r, H16mr A8 i A9) funkcji interfejsu sieciowego dla zdalnego pamięci bezpośredniego dostępu (do pamięci RDMA) łączności. (Oznaczone ciągiem "r", takie jak NC24r rozmiary wybranej serii N są także z funkcją RDMA). Ten interfejs jest oprócz interfejsu standardowe sieć platformy Azure, dostępne dla innych rozmiarów maszyn wirtualnych. 
  
Ten interfejs umożliwia wystąpienia z funkcją RDMA do komunikacji za pośrednictwem sieci InfiniBand (IB), działających z rozdzielczością stawki FDR H16r, H16mr i z funkcją RDMA N-serii maszyn wirtualnych i szybkości QDR A8 i A9 maszyn wirtualnych. Tych funkcji RDMA może zwiększyć skalowalność i wydajność niektórych aplikacji komunikat interfejsu (Passing Interface).

> [!NOTE]
> IP przez IB nie jest obsługiwana na platformie Azure. Tylko RDMA over IB jest obsługiwana.
>

Wdrażanie maszyn wirtualnych z funkcją RDMA HPC w tym samym zestawie dostępności lub zestaw skali maszyny Wirtualnej (przy użyciu modelu wdrażania usługi Azure Resource Manager) lub samej usługi w chmurze (przy użyciu klasycznego modelu wdrażania). Postępuj zgodnie z dodatkowymi wymaganiami dotyczącymi z funkcją RDMA HPC maszyn wirtualnych do uzyskania dostępu do sieci Azure RDMA.