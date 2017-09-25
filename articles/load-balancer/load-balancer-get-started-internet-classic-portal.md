---
title: "Tworzenie modułu równoważenia obciążenia dostępnego z Internetu — Azure Portal (model klasyczny) | Microsoft Docs"
description: "Dowiedz się, jak utworzyć dostępny z Internetu moduł równoważenia obciążenia w klasycznym modelu wdrażania za pomocą klasycznego portalu Azure"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: a022154f5eca6de2d2dbfc1b9aa30d2ea0a7d650
ms.contentlocale: pl-pl
ms.lasthandoff: 01/24/2017

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Wprowadzenie do tworzenia dostępnego z Internetu modułu równoważenia obciążenia (klasycznego) za pomocą klasycznego portalu Azure

> [!div class="op_single_selector"]
> * [Klasyczna witryna Azure Portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [Program PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Przed rozpoczęciem pracy z zasobami platformy Azure należy pamiętać, że ma ona obecnie dwa modele wdrażania: za pomocą usługi Azure Resource Manager i model klasyczny. Przed rozpoczęciem pracy z dowolnym zasobem Azure należy zapoznać się z [modelami i narzędziami wdrażania](../azure-classic-rm.md). Dokumentację dotyczącą różnych narzędzi można wyświetlić, klikając karty w górnej części artykułu. W tym artykule opisano klasyczny model wdrażania. Możesz też zapoznać się z artykułem na temat [tworzenia dostępnego z Internetu modułu równoważenia obciążenia za pomocą usługi Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Konfigurowanie dostępnego z Internetu modułu równoważenia obciążenia do maszyn wirtualnych

Aby zrównoważyć obciążenie ruchu sieciowego z Internetu na maszynach wirtualnych usługi w chmurze, należy utworzyć zestaw o zrównoważonym obciążeniu. Na potrzeby tej procedury przyjęto założenie, że maszyny wirtualne zostały już utworzone i należą do tej samej usługi w chmurze.

**Konfigurowanie zestawu o zrównoważonym obciążeniu do maszyn wirtualnych**

1. W klasycznym portalu Azure kliknij opcję **Maszyny wirtualne**, a następnie kliknij nazwę maszyny wirtualnej w zestawie o zrównoważonym obciążeniu.
2. Kliknij kolejno opcje **Punkty końcowe** i **Dodaj**.
3. Na stronie **Add an endpoint to a virtual machine** (Dodaj punkt końcowy do maszyny wirtualnej) kliknij strzałkę w prawo.
4. Na stronie **Specify the details of the endpoint** (Podaj szczegóły punktu końcowego):

   * W obszarze **Nazwa**, wpisz nazwę punktu końcowego lub wybierz ją z listy wstępnie zdefiniowanych punktów końcowych dla stosowanych powszechnie protokołów.
   * W obszarze **Protokół** wybierz protokół wymagany przez dany typ punktu końcowego — TCP lub UDP.
   * W obszarze **Public Port and Private Port** (Port publiczny i port prywatny) wpisz numery portów, z których ma korzystać maszyna wirtualna, zgodnie z potrzebami. Możesz wykorzystać reguły portu prywatnego i zapory maszyny wirtualnej, aby przekierować ruch sieciowy w sposób odpowiedni do danego zastosowania. Port prywatny może być taki sam, jak publiczny. Na przykład w przypadku punktu końcowego ruchu sieciowego (HTTP) port 80 można przypisać zarówno jako publiczny, jak i prywatny.

5. Wybierz opcję **Create a load-balanced set** (Utwórz zestaw o zrównoważonym obciążeniu), a następnie kliknij strzałkę w prawo.
6. Na stronie **Configure the load-balanced set** (Konfiguracja zestawu o zrównoważonym obciążeniu) wpisz nazwę zestawu o zrównoważonym obciążeniu, a następnie przypisz wartości zachowania sondy usługi Azure Load Balancer. Moduł równoważenia obciążenia korzysta z sond, aby określić, czy maszyny wirtualne w zestawie o zrównoważonym obciążeniu mogą otrzymywać przychodzący ruch sieciowy.
7. Kliknij znacznik wyboru, aby utworzyć punkt końcowy o zrównoważonym obciążeniu. Zostanie wyświetlony komunikat **Tak** w kolumnie **Load-balanced set name** (Nazwa zestawu o zrównoważonym obciążeniu) na stronie **Punkty końcowe** dotyczącej maszyny wirtualnej.
8. W portalu kliknij kolejno opcję **Maszyny wirtualne**, nazwę dodatkowej maszyny wirtualnej w zestawie o zrównoważonym obciążeniu, opcję **Punkty końcowe** oraz opcję **Dodaj**.
9. Na stronie **Add an endpoint to a virtual machine** (Dodaj punkt końcowy do maszyny wirtualnej) kliknij opcję **Add endpoint to an existing load-balanced set** (Dodaj punkt końcowy do istniejącego zestawu o zrównoważonym obciążeniu), wybierz nazwę zestawu o zrównoważonym obciążeniu, a następnie kliknij strzałkę w prawo.
10. Na stronie **Specify the details of the endpoint** (Podaj szczegóły punktu końcowego) wpisz nazwę punktu końcowego, a następnie kliknij znacznik wyboru.

Aby dodać maszyny wirtualne do zestawu o zrównoważonym obciążeniu, wykonaj ponownie kroki 8–10.

## <a name="next-steps"></a>Następne kroki

[Get started configuring an internal load balancer](load-balancer-get-started-ilb-arm-ps.md) (Wprowadzenie do konfigurowania wewnętrznego modułu równoważenia obciążenia)

[Configure a load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)

