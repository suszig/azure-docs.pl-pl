---
title: "Tworzenie wewnętrznego modułu równoważenia obciążenia — witryna Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak utworzyć wewnętrzny moduł równoważenia obciążenia za pomocą usługi Resource Manager w witrynie Azure Portal"
services: load-balancer
documentationcenter: na
author: KumudD
manager: jennoc
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 5b983ca9ff28aac7f0e0501f353c48deeb6adcd5
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Tworzenie wewnętrznego modułu równoważenia obciążenia w witrynie Azure Portal

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Szablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)


[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Rozpoczęcie tworzenia wewnętrznego modułu równoważenia obciążenia w witrynie Azure Portal

Wykonaj poniższe czynności, aby utworzyć wewnętrzny moduł równoważenia obciążenia w witrynie Azure Portal.

1. Otwórz przeglądarkę, przejdź do witryny [Azure Portal](http://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.
2. W lewym górnym rogu ekranu kliknij kolejno pozycje **Nowy** > **Sieci** > **Moduł równoważenia obciążenia**.
3. W bloku **Tworzenie modułu równoważenia obciążenia** wprowadź wartość pola **Nazwa** dla modułu równoważenia obciążenia.
4. W obszarze **Schemat** kliknij pozycję **Wewnętrzny**.
5. Kliknij pozycję **Sieć wirtualna**, a następnie wybierz sieć wirtualną, w której chcesz utworzyć moduł równoważenia obciążenia.

   > [!NOTE]
   > Jeśli nie widzisz sieci wirtualnej, której chcesz użyć, sprawdź wartość pola **Lokalizacja** dla modułu równoważenia obciążenia i odpowiednio ją zmień.

6. Kliknij pozycję **Podsieć**, a następnie wybierz podsieć, w której chcesz utworzyć moduł równoważenia obciążenia.
7. W obszarze **Przypisanie adresu IP** kliknij opcję **Dynamiczny** lub **Statyczny** w zależności od tego, czy adres IP modułu równoważenia obciążenia ma być ustalony (statyczny) czy nie.

   > [!NOTE]
   > W przypadku wyboru opcji użycia statycznego adresu IP należy podać adres modułu równoważenia obciążenia.

8. W obszarze **Grupa zasobów** określ nazwę nowej grupy zasobów dla modułu równoważenia obciążenia lub kliknij pozycję **wybierz istniejącą** i wybierz istniejącą grupę zasobów.
9. Kliknij przycisk **Utwórz**.

## <a name="configure-load-balancing-rules"></a>Konfigurowanie reguł równoważenia obciążenia

Po utworzeniu modułu równoważenia obciążenia przejdź do zasobu modułu równoważenia obciążenia w celu jego skonfigurowania.
Przed skonfigurowaniem reguły równoważenia obciążenia skonfiguruj pulę adresów zaplecza i sondę.

### <a name="step-1-configure-a-backend-pool"></a>Krok 1. Konfigurowanie puli zaplecza

1. W witrynie Azure Portal kliknij kolejno pozycje **Przeglądaj**  >  **Moduły równoważenia obciążenia**, a następnie kliknij utworzony wcześniej moduł równoważenia obciążenia.
2. W bloku **Ustawienia** kliknij pozycję **Pule zaplecza**.
3. W bloku **Pule adresów zaplecza** kliknij pozycję **Dodaj**.
4. W bloku **Dodawanie puli zaplecza** wprowadź wartość pola **Nazwa** dla puli zaplecza, a następnie kliknij przycisk **OK**.

### <a name="step-2-configure-a-probe"></a>Krok 2. Konfigurowanie sondy

1. W witrynie Azure Portal kliknij kolejno pozycje **Przeglądaj**  >  **Moduły równoważenia obciążenia**, a następnie kliknij utworzony wcześniej moduł równoważenia obciążenia.
2. W bloku **Ustawienia** kliknij pozycję **Sondy**.
3. W bloku **Sondy** kliknij pozycję **Dodaj**.
4. W bloku **Dodawanie sondy** wprowadź wartość pola **Nazwa** dla sondy.
5. W obszarze **Protokół** wybierz pozycję **HTTP** (w przypadku witryn internetowych) lub **TCP** (w przypadku innych aplikacji działających w oparciu o protokół TCP).
6. W obszarze **Port** określ port używany podczas uzyskiwania dostępu do sondy.
7. W obszarze **Ścieżka** (tylko w przypadku sond protokołu HTTP) określ ścieżkę do użycia jako sonda.
8. W obszarze **Interwał** określ częstotliwość sondowania aplikacji.
9. W obszarze **Próg złej kondycji** określ liczbę prób, która powinna zakończyć się niepowodzeniem, zanim maszyna wirtualna zaplecza zostanie oznaczona jako będąca w złej kondycji.
10. Kliknij przycisk **OK**, aby utworzyć sondę.

### <a name="step-3-configure-load-balancing-rules"></a>Krok 3. Konfigurowanie reguł równoważenia obciążenia

1. W witrynie Azure Portal kliknij kolejno pozycje **Przeglądaj**  >  **Moduły równoważenia obciążenia**, a następnie kliknij utworzony wcześniej moduł równoważenia obciążenia.
2. W bloku **Ustawienia** kliknij pozycję **Reguły równoważenia obciążenia**.
3. W bloku **Reguły równoważenia obciążenia** kliknij pozycję **Dodaj**.
4. W bloku **Dodaj regułę równoważenia obciążenia** wprowadź wartość pola **Nazwa** dla reguły.
5. W obszarze **Protokół** wybierz pozycję **HTTP** (w przypadku witryn internetowych) lub **TCP** (w przypadku innych aplikacji działających w oparciu o protokół TCP).
6. W obszarze **Port** określ port, z którym klienci łączą się w module równoważenia obciążenia.
7. W obszarze **Port zaplecza** określ port do użycia w puli zaplecza (przeważnie port modułu równoważenia obciążenia jest taki sam jak port zaplecza).
8. W obszarze **Pula zaplecza** wybierz utworzoną wcześniej pulę zaplecza.
9. W obszarze **Trwałość sesji** wybierz sposób obsługi trwałości sesji.
10. W obszarze **Limit czasu bezczynności (minuty)** określ limit czasu bezczynności.
11. W obszarze **Zmienny adres IP (bezpośredni zwrot serwera)** kliknij pozycję **Wyłączony** lub **Włączony**.
12. Kliknij przycisk **OK**.

## <a name="next-steps"></a>Następne kroki

[Configure a load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)

