---
title: "Utwórz bramę aplikacji - portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć bramę aplikacji przy użyciu portalu"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 17d09ce98c40717d1db0927f791a7c97ea7835e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-with-the-portal"></a>Tworzenie bramy aplikacji za pomocą portalu

[Brama aplikacji w](application-gateway-introduction.md) jest dedykowany urządzenie wirtualne udostępniające kontroler dostarczania aplikacji (ADC) jako usługa, oferty różne obciążenia warstwy 7 równoważenia możliwości aplikacji. W tym artykule przeprowadza użytkownika przez kroki, aby utworzyć bramę aplikacji przy użyciu portalu Azure i Dodawanie istniejącego serwera jako członka wewnętrznej bazy danych.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do portalu Azure pod adresem [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Utwórz bramę aplikacji

Aby utworzyć bramę aplikacji, wykonaj kroki, które należy wykonać. Brama aplikacji wymaga jego własnej podsieci. Podczas tworzenia sieci wirtualnej, upewnij się, że pozostanie wystarczająco dużo przestrzeni adresowej do mają wiele podsieci. Po wdrożeniu brama aplikacji w podsieci tylko inne bramy aplikacji można dodać do niego.

1. W okienku Ulubione w portalu kliknij **nowy**
1. W bloku **Nowy** kliknij pozycję **Sieć**. W **sieci** bloku, kliknij przycisk **brama aplikacji w**, jak pokazano na poniższej ilustracji:

    ![Tworzenie bramy aplikacji][1]

1. W **podstawy** bloku, zostanie wyświetlone, wprowadź następujące wartości, a następnie kliknij przycisk **OK**:

   | **Ustawienie** | **Wartość** | **Szczegóły**|
   |---|---|---|
   |**Nazwa**|AdatumAppGateway|Nazwa bramy aplikacji|
   |**Warstwy**|Standardowa|Dostępne wartości to Standard i zapory aplikacji sieci Web. Odwiedź stronę [zapory aplikacji sieci web](application-gateway-web-application-firewall-overview.md) Aby dowiedzieć się więcej na temat zapory aplikacji sieci Web.|
   |**Rozmiar jednostki SKU**|Medium|Wybór, w przypadku wybrania warstwy standardowa jest mały, średni i duży. W przypadku wybrania warstwy zapory aplikacji sieci Web, opcje tylko są średni i duży.|
   |**Liczba wystąpień**|2|Liczba wystąpień brama aplikacji w celu zapewnienia wysokiej dostępności. Liczby wystąpień 1 należy używać tylko do celów testowych.|
   |**Subskrypcja**|[Twoja subskrypcja]|Wybierz subskrypcję, w której chcesz utworzyć bramę aplikacji.|
   |**Grupa zasobów**|**Utwórz nowy:** AdatumAppGatewayRG|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Lokalizacja**|Zachodnie stany USA||

1. W **ustawienia** bloku, który jest wyświetlany w obszarze **sieci wirtualnej**, kliknij przycisk **wybierz sieć wirtualną**. **Sieci wirtualnej wybierz** zostanie otwarty blok.  Kliknij przycisk **Utwórz nowy** otworzyć **Utwórz sieć wirtualną** bloku.

   ![Wybierz sieć wirtualną][2]

1. Na **bloku sieci wirtualnej Utwórz** wprowadź następujące wartości, a następnie kliknij przycisk **OK**. **Utwórz sieć wirtualną** i **sieci wirtualnej wybierz** zamknąć bloków. Ten krok powoduje wypełnienie **podsieci** na **ustawienia** blok podsieci wybrany.

   | **Ustawienie** | **Wartość** | **Szczegóły**|
   |---|---|---|
   |**Nazwa**|AdatumAppGatewayVNET|Nazwa bramy aplikacji|
   |**Przestrzeń adresów**|10.0.0.0/16|Jest to przestrzeni adresowej dla sieci wirtualnej|
   |**Nazwa podsieci**|AppGatewaySubnet|Nazwa podsieci bramy aplikacji|
   |**Zakres adresów podsieci**|10.0.0.0/28|Ta podsieć umożliwia więcej dodatkowe podsieci w sieci wirtualnej dla członków puli wewnętrznej bazy danych|

1. Na **ustawienia** bloku w obszarze **konfiguracji IP frontonu**, wybierz **publicznego** jako **typ adresu IP**

1. Na **ustawienia** bloku w obszarze **publicznego adresu IP** kliknij **wybierz publiczny adres IP**, **wybierz publiczny adres IP** zostanie otwarty blok Kliknij przycisk **Utwórz nowy**.

   ![Wybierz publiczny adres ip][3]

1. Na **tworzenie publicznego adresu IP** bloku, zaakceptuj wartość domyślną, a następnie kliknij przycisk **OK**. Blok zamyka i wypełnia **publicznego adresu IP** z wybranego publicznego adresu IP.

1. Na **ustawienia** bloku w obszarze **konfiguracji odbiornika**, kliknij przycisk **HTTP** w obszarze **protokołu**. Wprowadź port używany w **portu** pola.

2. Kliknij przycisk **OK** na **ustawienia** bloku, aby kontynuować.

1. Sprawdź ustawienia na **Podsumowanie** bloku i kliknij przycisk **OK** aby rozpocząć tworzenie bramy aplikacji. Tworzenie bramy aplikacji jest długo działające zadania i czas do ukończenia.

## <a name="add-servers-to-backend-pools"></a>Dodawanie serwerów do puli wewnętrznej bazy danych

Po utworzeniu bramy aplikacji, systemów tego hosta aplikacji o zrównoważonym obciążeniu nadal należy do dodania do bramy aplikacji. Adresy IP, nazwy FQDN lub kart sieciowych z tych serwerów są dodawane do puli adresów zaplecza.

### <a name="ip-address-or-fqdn"></a>Adres IP lub nazwa FQDN

1. Bramy aplikacji utworzone w portalu Azure **ulubione** okienku, kliknij przycisk **wszystkie zasoby**. Kliknij przycisk **AdatumAppGateway** brama aplikacji w bloku wszystkich zasobów. Jeśli subskrypcja już ma kilka zasobów, możesz wprowadzić **AdatumAppGateway** w **filtrować według nazwy...** aby łatwo uzyskać dostęp do bramy aplikacji.

1. Brama aplikacji utworzonej jest wyświetlany. Kliknij przycisk **pul zaplecza**i wybierz bieżącej puli zaplecza **appGatewayBackendPool**, **appGatewayBackendPool** zostanie otwarty blok.

   ![Pule zaplecza bramy aplikacji][4]

1. Kliknij przycisk **dodać docelowy** do dodawania adresów IP z wartości nazw FQDN. Wybierz **IP adres lub nazwa FQDN** jako **typu** i wprowadź adres IP lub nazwa FQDN w polu. Powtórz ten krok dla członków puli dodatkowych wewnętrznej bazy danych. Po zakończeniu kliknij przycisk **zapisać**.

### <a name="virtual-machine-and-nic"></a>Maszyna wirtualna i karty Sieciowej

Karty sieciowe maszyny wirtualnej można również dodać jako członków puli wewnętrznej bazy danych. Tylko dla maszyn wirtualnych w tej samej sieci wirtualnej co brama aplikacji są dostępne za pośrednictwem listy rozwijanej.

1. Bramy aplikacji utworzone w portalu Azure **ulubione** okienku, kliknij przycisk **wszystkie zasoby**. Kliknij przycisk **AdatumAppGateway** brama aplikacji w bloku wszystkich zasobów. Jeśli subskrypcja już ma kilka zasobów, możesz wprowadzić **AdatumAppGateway** w **filtrować według nazwy...** aby łatwo uzyskać dostęp do bramy aplikacji.

1. Brama aplikacji utworzonej jest wyświetlany. Kliknij przycisk **pul zaplecza**i wybierz bieżącej puli zaplecza **appGatewayBackendPool**, **appGatewayBackendPool** zostanie otwarty blok.

   ![Pule zaplecza bramy aplikacji][5]

1. Kliknij przycisk **dodać docelowy** do dodawania adresów IP z wartości nazw FQDN. Wybierz **maszyny wirtualnej** jako **typu** i wybierz maszynę wirtualną, a kartę Sieciową do użycia. Po zakończeniu kliknij przycisk **Zapisz**

   > [!NOTE]
   > Tylko dla maszyn wirtualnych w tej samej sieci wirtualnej co brama aplikacji są dostępne w menu rozwijane.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, Usuń grupę zasobów, bramę aplikacji i wszystkie powiązane zasoby. Aby to zrobić, wybierz grupę zasobów z bloku bramy aplikacji, a następnie kliknij przycisk **usunąć**.

## <a name="next-steps"></a>Następne kroki

W tym scenariuszu wdrożyć bramę aplikacji i dodać serwer do wewnętrznej bazy danych. Następne kroki są do skonfigurowania bramy aplikacji przez modyfikowanie ustawień i dostosowywanie reguły w bramie. Te kroki można znaleźć odwiedzając następujące artykuły:

Dowiedz się, jak utworzyć sondy kondycji niestandardowych odwiedzając [utworzyć sondy kondycji niestandardowych](application-gateway-create-probe-portal.md)

Dowiedz się, jak skonfigurować odciążanie protokołu SSL i podejmij kosztowne odszyfrowywania SSL poza serwerów sieci web, odwiedzając [skonfigurować odciążanie protokołu SSL](application-gateway-ssl-portal.md)

Dowiedz się, jak chronić swoje aplikacje za pomocą [zapory aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md) funkcji bramy aplikacji.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
