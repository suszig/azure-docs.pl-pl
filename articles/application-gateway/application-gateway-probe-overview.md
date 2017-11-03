---
title: "Omówienie monitorowania bramy aplikacji Azure kondycji | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat możliwości monitorowania bramy aplikacji Azure"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: davidmu
ms.openlocfilehash: 83a0b1be1aba48146aa1aaedb36ad9d9d23f17d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="application-gateway-health-monitoring-overview"></a>Omówienie monitorowania kondycji bramy aplikacji

Azure Application Gateway domyślnie monitoruje kondycję wszystkich zasobów w jej puli zaplecza i automatycznie usuwa dowolnego zasobu z puli określana jako zła. Brama aplikacji w kontynuuje monitorowanie wystąpień złej kondycji i dodaje je do dobrej kondycji puli zaplecza, gdy staną się dostępne i odpowiadać na zdarzenie sondy kondycji. Brama aplikacji w wysyła sondy kondycji z tego samego portu, który jest zdefiniowany w ustawieniach protokołu HTTP zaplecza. Taka konfiguracja powoduje, że sondy testuje tego samego portu, którego klienci będą stosowane do nawiązania połączenia z zapleczem.

![przykład sondowania bramy aplikacji][1]

Oprócz przy użyciu sondy kondycji domyślnej funkcji monitorowania, można również dostosować sondy kondycji do własnych wymagań aplikacji. W tym artykule omówiono domyślnych i sondy kondycji niestandardowych.

> [!NOTE]
> Jeśli istnieje grupa NSG podsieci bramy aplikacji, zakresy portów 65503 65534 powinien zostać otwarty w podsieci bramy aplikacji dla ruchu przychodzącego. Te porty są wymagane dla kondycji zaplecza interfejsu API do pracy.

## <a name="default-health-probe"></a>Domyślnej funkcji badania kondycji

Bramy aplikacji automatycznie konfiguruje domyślnego badania kondycji, konfigurując nie żadnej konfiguracji niestandardowej sondowania. Zachowanie monitorowania polega na wysłał żądanie HTTP do adresów IP skonfigurowanych dla puli zaplecza. Dla domyślnej sondy Jeśli ustawienia http zaplecza są skonfigurowane dla protokołu HTTPS, sondy używa protokołu HTTPS również do testowania kondycji zapleczy.

Na przykład: Konfigurowanie bramy aplikacji do korzystania z serwerów zaplecza, A, B i C do odbierania ruchu sieciowego HTTP na porcie 80. Monitorowanie kondycji domyślne testy trzy serwery co 30 sekund na dobrej kondycji odpowiedź HTTP. Dobrej odpowiedzi HTTP ma [kod stanu](https://msdn.microsoft.com/library/aa287675.aspx) między 200 i 399.

Jeśli serwer A domyślnym sprawdzaniem sondy nie powiedzie się, bramę aplikacji spowoduje usunięcie jej z puli zaplecza, a ruch sieciowy przestanie przepływać do tego serwera. Domyślnego badania nadal w dalszym ciągu Sprawdź, czy serwer co 30 sekund. Gdy serwer A pomyślnie odpowiada na żądanie jednego z domyślnego badania kondycji, dodaniu wstecz dobrej kondycji do puli zaplecza i ruchu uruchamia ponownie przepływać do serwera.

### <a name="default-health-probe-settings"></a>Domyślne ustawienia sondy kondycji

| Właściwość sondy | Wartość | Opis |
| --- | --- | --- |
| Sonda adresu URL |http://127.0.0.1:\<portu\>/ |Ścieżka adresu URL |
| Interwał |30 |Interwał sondowania w sekundach |
| Limit czasu |30 |Sonda limitu czasu w sekundach |
| Próg złej kondycji |3 |Badania liczby ponownych prób. Po kolejnych sondowania liczby awarii osiągnie próg złej kondycji serwera zaplecza jest oznaczony jako w dół. |

> [!NOTE]
> Port jest tym samym porcie ustawienia HTTP zaplecza.

Domyślnego badania sprawdza tylko http://127.0.0.1:\<portu\> do określenia stanu kondycji. Jeśli trzeba skonfigurować sondy kondycji, aby przejść do niestandardowy adres URL lub zmodyfikuj inne ustawienia, należy użyć niestandardowego sond zgodnie z opisem w poniższych krokach:

## <a name="custom-health-probe"></a>Sondy kondycji niestandardowych

Niestandardowe sond umożliwiają mieć większą kontrolę nad monitorowanie kondycji. Podczas korzystania z niestandardowego sond, można skonfigurować interwał sondowania, adres URL i ścieżki do testowania i zaakceptować przed oznaczeniem wystąpienia puli zaplecza swój stan jako niezdrowy ile odpowiedzi nie powiodło się.

### <a name="custom-health-probe-settings"></a>Ustawienia sondy kondycji niestandardowych

Poniższa tabela zawiera definicje dla właściwości sondy kondycji niestandardowych.

| Właściwość sondy | Opis |
| --- | --- |
| Nazwa |Nazwa sondy. Ta nazwa jest używana do odwoływania się do sondowania w ustawieniach protokołu HTTP zaplecza. |
| Protokół |Protokół używany do wysyłania sondy. Sonda korzysta z protokołu zdefiniowanego w ustawieniach protokołu HTTP zaplecza |
| Host |Nazwa hosta, aby wysłać sondy. Dotyczy tylko wtedy, gdy obejmujący wiele lokacji jest skonfigurowany dla bramy aplikacji, w przeciwnym razie użyj '127.0.0.1'. Ta wartość jest inna niż nazwa hosta maszyny Wirtualnej. |
| Ścieżka |Ścieżka względna sondy. Nieprawidłowa ścieżka rozpoczyna się od '/'. |
| Interwał |Interwał sondy w sekundach. Ta wartość jest odstęp czasu między dwa kolejne sondy. |
| Limit czasu |Badania limitu czasu w sekundach. Jeśli w tym czasie limitu czasu nie odebrano prawidłowej odpowiedzi, sondy jest oznaczony jako nie powiodła się.  |
| Próg złej kondycji |Badania liczby ponownych prób. Po kolejnych sondowania liczby awarii osiągnie próg złej kondycji serwera zaplecza jest oznaczony jako w dół. |

> [!IMPORTANT]
> Jeśli skonfigurowano brama aplikacji w jednej lokacji, domyślnie hosta należy określać nazwy jako "127.0.0.1", chyba że w przeciwnym razie skonfigurowane w niestandardowych sondowania.
> Dla odwołania do badania niestandardowe są wysyłane do \<protokołu\>://\<hosta\>:\<portu\>\<ścieżki\>. Port używany będzie ten sam port, zgodnie z definicją w ustawienia HTTP zaplecza.

## <a name="next-steps"></a>Następne kroki
Po szkoleniowe dotyczące monitorowania kondycji aplikacji bramy, można skonfigurować [sondy kondycji niestandardowych](application-gateway-create-probe-portal.md) w portalu Azure lub [sondy kondycji niestandardowych](application-gateway-create-probe-ps.md) przy użyciu programu PowerShell i modelu wdrażania usługi Azure Resource Manager.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
