---
title: "Tworzenie bramy aplikacji przy użyciu reguł routingu adresów URL - Azure CLI 2.0 | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera instrukcje dotyczące sposobu tworzenia i konfigurowania bramy aplikacji przy użyciu reguł routingu adresów URL."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 10d01d5d80e2d111d6b39598eed3612f80162b23
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-path-based-routing-with-azure-cli-20"></a>Utwórz bramę aplikacji przy użyciu routingu opartego na ścieżkę 2.0 interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-create-url-route-cli.md)

Adres URL na podstawie ścieżki routingu, należy skojarzyć tras na podstawie ścieżki adresu URL żądania HTTP. Sprawdza, czy istnieje trasa z pulą serwera zaplecza skonfigurowane dla adresu URL podanego przez bramę aplikacji, a następnie wysyła ruchu sieciowego do określonych puli. Użycia routingu adresów URL na podstawie ścieżki jest w celu zrównoważenia obciążenia żądaniami dla różnych typów zawartości do innego serwera zaplecza pul.

Brama aplikacji w Azure wykorzystuje dwa typy reguł: basic i adres URL reguły ścieżki. Typ podstawowy reguły udostępnia usługę okrężnego dla pul zaplecza. Ścieżka reguły, oprócz rozdzielanie, również umożliwia wzorzec ścieżki adresu URL żądania Wybieranie odpowiedniej puli zaplecza.

## <a name="scenario"></a>Scenariusz

W poniższym przykładzie bramę aplikacji udostępniają ruchu dla domeny contoso.com z dwóch pul serwerów zaplecza: domyślna pula serwera i puli serwerów obrazu.

Żądania dla http://contoso.com/image * są kierowane do puli serwerów obrazu (**imagesBackendPool**). Jeśli wzorzec ścieżki nie jest zgodny, brama aplikacji w wybrana domyślna pula serwera (**appGatewayBackendPool**).

![Trasy adresu URL](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Otwórz **wiersza polecenia usługi Microsoft Azure** i zaloguj się na:

```azurecli
az login -u "username"
```

> [!NOTE]
> Można również użyć `az login` bez przełącznika dla nazwy logowania urządzenia, która wymaga wprowadzenie kodu na aka.ms/devicelogin.

Po wprowadzeniu poprzedniego polecenia, otrzymasz kod. Przejdź do https://aka.ms/devicelogin w przeglądarce, aby kontynuować proces logowania.

![cmd przedstawiający urządzenia logowania][1]

W przeglądarce wprowadź otrzymany kod. To przekieruje Cię do strony logowania.

![przeglądarki, aby wprowadzić kod][2]

Wprowadź kod, aby zarejestrować, a następnie zamknij przeglądarkę, aby kontynuować.

![pomyślnie zalogował się][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Dodawanie reguły na podstawie ścieżki do istniejącej bramy aplikacji

Poniższe kroki pokazują, jak dodać reguły na podstawie ścieżki do istniejącej bramy aplikacji.
### <a name="create-a-new-back-end-pool"></a>Utwórz nową pulę zaplecza

Skonfiguruj ustawienia bramy aplikacji **imagesBackendPool** dla ruchu sieciowego z równoważeniem obciążenia w puli zaplecza. W tym przykładzie możesz skonfigurować ustawienia innej puli zaplecza nowej puli zaplecza. Każda pula zaplecza może mieć własne ustawienia. Reguły ścieżki, użyj ustawienia HTTP zaplecza można kierować ruchem do elementów członkowskich puli zaplecza poprawne. Określa protokół i port, który jest używany podczas wysyłania ruchu do elementów członkowskich puli zaplecza. Ustawienia HTTP zaplecza również określić na podstawie pliku cookie sesji.  Włączenie koligacji na podstawie plików cookie sesji wysyła ruch do tej samej zaplecza w poprzednim żądania dla każdego pakietu.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port-for-an-application-gateway"></a>Utwórz nowy port frontonu bramy aplikacji

Obiekt konfiguracji portów frontonu jest używany przez odbiornik do zdefiniowania, jakie portu bramy aplikacji nasłuchuje ruchu na odbiornika.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Utwórz nowy

W tym kroku dla odbiornika konfigurowany jest publiczny adres IP i port używane do odbierania przychodzącego ruchu sieciowego. W poniższym przykładzie przyjmuje wcześniej skonfigurowane konfiguracji IP frontonu, Konfiguracja portów frontonu i protokołu (http lub https, który jest uwzględniana wielkość liter) i konfiguruje odbiornika. W tym przykładzie odbiornika nasłuchuje ruchu HTTP na porcie 82 publiczny adres IP utworzony wcześniej w tym scenariuszu.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Tworzenie mapy ścieżki adresu URL

Ten krok obejmuje skonfigurowanie względnej ścieżki adresu URL używany przez bramę aplikacji, aby zdefiniować mapowanie między ścieżkę i puli zaplecza przypisane do obsługi ruchu przychodzącego.

> [!IMPORTANT]
> Każda ścieżka musi rozpoczynać się od "/", a miejsce tylko gwiazdkę jest dozwolone, jest na końcu. Nieprawidłowa przykładów /xyz, /xyz* lub/xyz / *. Ciąg przekazywani do dopasowania ścieżki nie zawiera żadnego tekstu po pierwszym "?" lub "#", a te znaki są niedozwolone. 

Poniższy przykład tworzy jedną regułę/obrazów / * ścieżkę routingu ruchu do zaplecza **imagesBackendPool**. Ta zasada zapewnia, że ruchu dla każdego zestawu adresów URL jest kierowany do wewnętrznej. Na przykład http://adatum.com/images/figure1.jpg przechodzi do **imagesBackendPool**. Jeśli ścieżka nie odpowiada żadnemu z reguły ścieżki wstępnie zdefiniowane, Konfiguracja mapowania ścieżki reguły konfiguruje również domyślna pula adresów zaplecza. Na przykład http://adatum.com/shoppingcart/test.html przechodzi do **pool1** , ponieważ jest on zdefiniowany jako domyślna pula niedopasowane ruchu.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Kolejne kroki

Jeśli chcesz dowiedzieć się więcej o odciążania protokołu Secure Sockets Layer (SSL), zobacz [skonfigurować bramę aplikacji dla odciążania SSL](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
