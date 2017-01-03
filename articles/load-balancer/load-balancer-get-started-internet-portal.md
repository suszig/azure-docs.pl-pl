---
title: "Tworzenie modułu równoważenia obciążenia dostępnego z Internetu za pomocą usługi Resource Manager w witrynie Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak utworzyć moduł równoważenia obciążenia dostępny z Internetu za pomocą usługi Resource Manager w witrynie Azure Portal"
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: 5ce773311b691bf28ea3e9590ab74603a1a9b641
ms.openlocfilehash: 052a9e6a511875dcb2a79ecad89ee09b32948d06

---

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Tworzenie modułu równoważenia obciążenia dostępnego z Internetu za pomocą witryny Azure Portal

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [Program PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Szablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano model wdrażania usługi Resource Manager. Możesz też zapoznać się z artykułem na temat [tworzenia modułu równoważenia obciążenia dostępnego z Internetu za pośrednictwem wdrożenia klasycznego](load-balancer-get-started-internet-classic-portal.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

W tym artykule opisano zadania, które należy wykonać, aby utworzyć moduł równoważenia obciążenia, oraz szczegółowo wyjaśniono wszystkie czynności niezbędne do osiągnięcia tego celu.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Co jest wymagane do utworzenia modułu równoważenia obciążenia dostępnego z Internetu?

Aby wdrożyć moduł równoważenia obciążenia, należy utworzyć i skonfigurować poniższe obiekty.

* Konfiguracja IP frontonu — publiczne adresy IP dla przychodzącego ruchu sieciowego.
* Pula adresów zaplecza — interfejsy sieciowe (NIC) maszyn wirtualnych odbierających ruch sieciowy z modułu równoważenia obciążenia.
* Reguły równoważenia obciążenia — reguły mapowania portu publicznego modułu równoważenia obciążenia na port w puli adresów zaplecza.
* Reguły NAT ruchu przychodzącego — reguły mapowania portu publicznego modułu równoważenia obciążenia na port określonej maszyny wirtualnej w puli adresów zaplecza.
* Sondy — sondy kondycji używane do sprawdzania dostępności wystąpień maszyn wirtualnych w puli adresów zaplecza.

Więcej informacji o składnikach modułu równoważenia obciążenia tworzonego za pomocą usługi Azure Resource Manager można znaleźć w artykule [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia).

## <a name="set-up-a-load-balancer-in-azure-portal"></a>Konfigurowanie modułu równoważenia obciążenia w witrynie Azure Portal

> [!IMPORTANT]
> Jednym z złożeń przyjętych na potrzeby poniższego przykładu jest utworzenie sieci wirtualnej o nazwie **myVNet**. Aby ją utworzyć, zapoznaj się z artykułem o [tworzeniu sieci wirtualnej](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Kolejnym założeniem jest istnienie podsieci **LB-Subnet-BE** w sieci **myVNet** oraz dwóch maszyn wirtualnych (**web1** i **web2**) w zestawie dostępności o nazwie **myAvailSet** w sieci **myVNet**. Aby dowiedzieć się, jak utworzyć maszynę wirtualną, [kliknij tutaj](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

1. Przejdź w przeglądarce do witryny Azure Portal ([http://portal.azure.com](http://portal.azure.com)) i zaloguj się przy użyciu konta Azure.
2. Po lewej stronie ekranu wybierz kolejno opcje **Nowy** > **Sieć** > **Moduł równoważenia obciążenia**.
3. W bloku **Tworzenie modułu równoważenia obciążenia** wpisz nazwę modułu równoważenia obciążenia. W tym przykładzie ma on nazwę **myLoadBalancer**.
4. W obszarze **Typ** wybierz opcję **Publiczny**.
5. W obszarze **Publiczny adres IP** utwórz nowy publiczny adres IP o nazwie **myPublicIP**.
6. W obszarze Grupa zasobów wybierz **myRG**. Następnie wybierz odpowiednią **Lokalizację** i kliknij przycisk **OK**. Rozpocznie się wdrażanie modułu równoważenia obciążenia. Potrwa to kilka minut.

    ![Aktualizowanie grupy zasobów modułu równoważenia obciążenia](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)

## <a name="create-a-back-end-address-pool"></a>Tworzenie puli adresów zaplecza

1. Po wdrożeniu modułu równoważenia obciążenia wybierz go z puli zasobów. W obszarze ustawień wybierz opcję Pule zaplecza. Wpisz nazwę puli zaplecza. Następnie kliknij opcję **Dodaj** w górnej części wyświetlonego bloku.
2. Kliknij opcję **Dodaj maszynę wirtualną** w bloku **Dodaj pulę zaplecza**.  Wybierz opcję **Wybierz zestaw dostępności** w obszarze **Zestaw dostępności** i wybierz pozycję **myAvailSet**. Następnie wybierz opcję **Wybierz maszyny wirtualne** w sekcji Maszyny wirtualne bloku i kliknij pozycje **web1** i **web2**, które oznaczają dwie maszyny wirtualne utworzone do równoważenia obciążenia. Sprawdź, czy obie pozycje mają po lewej stronie niebieski symbol zaznaczenia pokazany na poniższej ilustracji. Następnie kliknij kolejno opcje **Wybierz**, przycisk OK w bloku **Wybieranie maszyn wirtualnych** i przycisk **OK** w bloku **Dodawanie puli zaplecza**.

    ![Dodawanie do puli adresów zaplecza ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Sprawdź, czy lista rozwijana powiadomień zawiera informację o zapisaniu puli zaplecza modułu równoważenia obciążenia (poza informacją o aktualizacji interfejsu sieciowego obu maszyn wirtualnych — **web1** i **web2**).

## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Tworzenie sondy, reguły równoważenia obciążenia i reguł NAT

1. Utwórz sondę kondycji.

    W obszarze Ustawienia modułu równoważenia obciążenia wybierz pozycję Sondy. Następnie kliknij opcję **Dodaj** w górnej części bloku.

    Istnieją dwie metody konfiguracji sondy: HTTP lub TCP. W tym przykładzie pokazano konfigurację HTTP, ale konfiguracja TCP przebiega bardzo podobnie.
    Zaktualizuj niezbędne informacje. Jak wspomniano wcześniej, moduł **myLoadBalancer** będzie równoważyć obciążenie na porcie 80. Wybrana ścieżka to HealthProbe.aspx, interwał wynosi 15 sekund, a próg złej kondycji wynosi 2. Na koniec kliknij przycisk **OK**, aby utworzyć sondę.

    Umieść kursor myszy na ikonie „i”, aby dowiedzieć się więcej o poszczególnych ustawieniach oraz o sposobach dostosowywania ich do swoich potrzeb.

    ![Dodawanie sondy](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Utwórz regułę modułu równoważenia obciążenia.

    W sekcji ustawień modułu równoważenia obciążenia kliknij pozycję Reguły równoważenia obciążenia. W nowym bloku kliknij opcję **Dodaj**. Nadaj nazwę regule. W tym przykładzie jej nazwa to HTTP. Wybierz port frontonu i port zaplecza. W tym przykładzie w obu przypadkach wybrano port 80. Wybierz pozycję **LB-backend** jako pulę zaplecza oraz utworzoną wcześniej sondę **HealthProbe**. Inne elementy konfiguracji można ustawić odpowiednio do potrzeb. Następnie kliknij przycisk OK, aby zapisać regułę równoważenia obciążenia.

    ![Dodawanie reguły równoważenia obciążenia](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Tworzenie reguł NAT dla ruchu przychodzącego

    Kliknij reguły NAT dla ruchu przychodzącego w sekcji ustawień modułu równoważenia obciążenia. W nowym bloku kliknij opcję **Dodaj**. Następnie nadaj nazwę regule NAT dla ruchu przychodzącego. W tym przykładzie jej nazwa to **inboundNATrule1**. Miejscem docelowym powinien być utworzony wcześniej publiczny adres IP. W obszarze Usługa wybierz opcję Niestandardowa i wybierz protokół, który ma zostać użyty. W tym przykładzie wybrano protokół TCP. Wprowadź numer portu (3441) oraz port docelowy — tutaj będzie to 3389. Następnie kliknij przycisk OK, aby zapisać regułę.

    Po utworzeniu pierwszej reguły powtórz tę procedurę dla drugiej reguły NAT dla ruchu przychodzącego (inboundNATrule2), która będzie obowiązywać między portem 3442 a portem docelowym 3389.

    ![Dodawanie reguły NAT dla ruchu przychodzącego](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Usuwanie modułu równoważenia obciążenia

Aby usunąć moduł równoważenia obciążenia, wybierz go. W bloku *Moduł równoważenia obciążenia* kliknij znajdującą się na górze opcję **Usuń**. Gdy pojawi się monit, kliknij przycisk **Tak**.

## <a name="next-steps"></a>Następne kroki

[Get started configuring an internal load balancer](load-balancer-get-started-ilb-arm-cli.md) (Wprowadzenie do konfigurowania wewnętrznego modułu równoważenia obciążenia)

[Configure a load balancer distribution mode](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)



<!--HONumber=Dec16_HO2-->


