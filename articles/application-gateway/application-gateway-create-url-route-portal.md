---
title: "Tworzenie reguły na podstawie ścieżki dla bramy aplikacji - portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć regułę na podstawie ścieżki dla bramy aplikacji przy użyciu portalu Azure."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.openlocfilehash: b207e7e7bd83e56db68288190c7bedafa8b5b7fa
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-azure-portal"></a>Tworzenie reguły na podstawie ścieżki dla bramy aplikacji przy użyciu portalu Azure

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-create-url-route-cli.md)

Adres URL na podstawie ścieżki routingu, należy skojarzyć tras na podstawie ścieżki adresu URL żądania HTTP. Sprawdza, czy istnieje trasa z pulą serwera zaplecza skonfigurowane dla adresu URL podanego przez bramę aplikacji, a następnie wysyła ruchu sieciowego do określonych puli. Użycia routingu adresów URL na podstawie ścieżki jest w celu zrównoważenia obciążenia żądaniami dla różnych typów zawartości do innego serwera zaplecza pul.

Bramy aplikacji ma dwa typy zasad: basic i adres URL reguły ścieżki. Typ podstawowy reguły udostępnia usługę okrężnego dla pul zaplecza. Ścieżka reguły, oprócz rozdzielanie, także używać wzorzec ścieżki adresu URL żądania, gdy Wybieranie odpowiedniej puli zaplecza.

## <a name="scenario"></a>Scenariusz

Poniższy scenariusz tworzy regułę na podstawie ścieżki w istniejącą bramę aplikacji.
W tym scenariuszu przyjęto założenie, że już zostały wykonane kroki przedstawione w [utworzyć bramę aplikacji za pomocą portalu](application-gateway-create-gateway-portal.md).

![Trasy adresu URL][scenario]

## <a name="createrule"></a>Tworzenie reguły na podstawie ścieżki

Reguły na podstawie ścieżki wymaga własnego odbiornika. Przed utworzeniem reguły, należy sprawdzić, czy odbiornik dostępne do użycia.

### <a name="step-1"></a>Krok 1

Przejdź do [portalu Azure](http://portal.azure.com) i wybierz istniejącą bramę aplikacji. Kliknij przycisk **reguły**.

![Application Gateway — omówienie][1]

### <a name="step-2"></a>Krok 2

Kliknij przycisk **na podstawie ścieżki** przycisk, aby dodać nową regułę na podstawie ścieżki.

### <a name="step-3"></a>Krok 3

**Reguły na podstawie ścieżki Dodaj** bloku ma dwie sekcje. Pierwsza sekcja jest, gdzie są zdefiniowane odbiornika, nazwę reguły i ustawienia ścieżki domyślnej. Domyślne ustawienia ścieżki są dla tras, które nie są objęte na podstawie ścieżki tras niestandardowych. Druga sekcja **reguły na podstawie ścieżki Dodaj** bloku jest, gdzie został zdefiniowany samych zasad na podstawie ścieżki.

**Ustawienia podstawowe**

* **Nazwa**: przyjazną nazwę reguły, który jest dostępny w portalu.
* **Odbiornik**: odbiornik, który jest używany dla reguły.
* **Domyślna pula zaplecza**: zaplecza do użycia dla reguły domyślnej.
* **Domyślne ustawienia HTTP**: ustawienia protokołu HTTP do użycia dla reguły domyślnej.

**Ustawienia reguły na podstawie ścieżki**

* **Nazwa**: przyjazną nazwę reguły na podstawie ścieżki.
* **Ścieżki**: ścieżka reguły szuka podczas przesyłania ruchu.
* **Puli zaplecza**: zaplecza do użycia dla reguły.
* **Ustawienie HTTP**: ustawienia protokołu HTTP do użycia dla reguły.

> [!IMPORTANT]
> **Ścieżki** ustawienie znajduje się lista wzorców ścieżka do dopasowania. Każdy wzorzec musi rozpoczynać się kreską ukośną i gwiazdkę jest dozwolona tylko na końcu. Prawidłowe przykłady: /xyz, /xyz*i /xyz/*.  

![Dodawanie bloku reguły na podstawie ścieżki z informacjami o wypełnione][2]

Dodanie reguły na podstawie ścieżki do istniejącej bramy aplikacji jest łatwy za pośrednictwem portalu Azure. Po utworzeniu reguły na podstawie ścieżki, można edytować o dodatkowe reguły. 

![Dodaj dodatkowe reguły ścieżki][3]

Ten krok obejmuje skonfigurowanie trasę na podstawie ścieżki. Należy zrozumieć, że żądania nie są ponownie zapisać. Żądania są dostępne w, bramy aplikacji sprawdza żądania, a oparte na wzorcu adres URL, wysyła żądanie do odpowiedniej puli zaplecza.

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się, jak skonfigurować odciążanie protokołu SSL z bramy aplikacji Azure, zobacz [skonfigurować bramę aplikacji dla odciążania protokołu SSL przy użyciu portalu Azure](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
