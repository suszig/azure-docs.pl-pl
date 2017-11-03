---
title: "Zarządzanie strefami DNS w usłudze Azure DNS - portalu Azure | Dokumentacja firmy Microsoft"
description: "Możesz zarządzać stref DNS przy użyciu portalu Azure. W tym artykule opisano sposób aktualizowanie, usuwanie i tworzenie stref DNS w usłudze Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 69a509612e6204fc93dd42bf2fe69cb165b5777c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Jak zarządzać stref DNS w portalu Azure

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-operations-dnszones-cli.md)

W tym artykule przedstawiono sposób zarządzania stref DNS przy użyciu portalu Azure. Można również zarządzać stref DNS przy użyciu wielu platform [interfejsu wiersza polecenia Azure](dns-operations-dnszones-cli.md) lub platformy Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

1. Logowanie się do witryny Azure Portal
2. W menu Centrum kliknij pozycję **Nowa > Sieć >** , a następnie kliknij pozycję **Strefa DNS**, aby otworzyć blok Tworzenie strefy DNS.

    ![Strefa DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. W bloku **Tworzenie strefy DNS** wprowadź następujące wartości, a następnie kliknij pozycję **Utwórz**:


   | **Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|contoso.com|Nazwa strefy DNS|
   |**Subskrypcja**|[Twoja subskrypcja]|Wybierz subskrypcję, aby utworzyć w jej ramach strefę DNS.|
   |**Grupa zasobów**|**Utwórz nową:** contosoDNSRG|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Lokalizacja**|Zachodnie stany USA||

> [!NOTE]
> Ustawienie Grupa zasobów dotyczy lokalizacji grupy zasobów i nie ma wpływu na strefę DNS. Lokalizacja strefy DNS jest zawsze „globalna” i nie jest wyświetlana.

## <a name="list-dns-zones"></a>Lista stref DNS

W portalu Azure, przejdź do **więcej usług** > **sieci** > **stref DNS**. Każdej strefy DNS jest jest własnych zasobów, informacje, takie jak liczba zestawów rekordów i serwery nazw są widoczne w tym widoku. Kolumna **serwery nazw** nie znajduje się w widok domyślny, aby dodać go kliknij **kolumn**, wybierz pozycję **serwery nazw** i kliknij przycisk **gotowe**.

![Lista stref DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Usuń strefę DNS

Przejdź do strefy DNS w portalu. Na **strefy DNS** bloku, kliknij przycisk **Usuń strefę**. Monit o potwierdzenie chcą usunąć strefę DNS. Usunięcie strefy DNS powoduje usunięcie wszystkich rekordów, które są zawarte w strefie.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak pracować z strefy DNS i rekordy odwiedzając [wprowadzenie do usługi Azure DNS przy użyciu portalu Azure](dns-getstarted-portal.md).