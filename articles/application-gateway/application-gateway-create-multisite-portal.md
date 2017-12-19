---
title: Kilka witryn z bramy aplikacji Azure | Dokumentacja firmy Microsoft
description: "Ta strona zawiera instrukcje dotyczące konfigurowania istniejącą bramę aplikacji Azure do obsługi wielu aplikacji sieci web na tej samej bramy z portalu Azure."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 28a7fcb3e08a9c4b6a27e9fbc8d3ebae309adc62
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Skonfigurować istniejącą bramę aplikacji do obsługi wielu aplikacji sieci web

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Obsługujący wiele lokacji umożliwia wdrożenie więcej niż jednej aplikacji sieci web na tej samej bramy aplikacji. Opiera się na obecność nagłówek hosta w przychodzące żądanie HTTP, aby określić, które odbiornika będzie odbierać dane. Odbiornik następnie kieruje ruch do puli zaplecza odpowiednie zgodnie z konfiguracją w definicji reguły bramy. Brama aplikacji w aplikacji sieci web z włączonym protokołem SSL, opiera się rozszerzenia oznaczenia nazwy serwera (SNI), aby wybrać poprawny odbiornika dla ruchu w sieci web. Zazwyczaj do obsługi wielu lokacji jest używane w celu zrównoważenia obciążenia żądaniami dla domen z innej witryny sieci web do innego serwera zaplecza pul. Podobnie wielu domen podrzędnych tej samej domeny katalogu głównego może być hostowana na tę samą bramę aplikacji.

## <a name="scenario"></a>Scenariusz

W poniższym przykładzie brama aplikacji jest obsługę ruchu dla domeny contoso.com i fabrikam.com z dwóch pul serwerów zaplecza: contoso puli serwerów i puli serwerów firmy fabrikam. Podobnych konfiguracji może posłużyć do hosta poddomen, takich jak app.contoso.com i blog.contoso.com.

![Scenariusz w wielu lokacjach][multisite]

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym scenariuszu dodaje obsługę wielu lokacji do istniejącej bramy aplikacji. Aby ukończyć ten scenariusz, istniejącą bramę aplikacji musi być dostępna do skonfigurowania. Odwiedź stronę [Utwórz bramę aplikacji przy użyciu portalu](application-gateway-create-gateway-portal.md) informacje na temat tworzenia aplikacji w warstwie podstawowa bramy w portalu.

Poniżej przedstawiono kroki niezbędne do zaktualizuj bramę aplikacji:

1. Tworzenie pul zaplecza dla każdej witryny.
2. Utwórz odbiornik dla każdej lokacji obsługuje bramy aplikacji.
3. Utwórz reguły mapowania każdego odbiornik mający odpowiednie zaplecza.

## <a name="requirements"></a>Wymagania

* **Pula serwerów zaplecza:** lista adresów IP serwerów zaplecza. Adresy IP na liście powinny należeć do podsieci sieci wirtualnej lub być publicznymi bądź wirtualnymi adresami IP. Można także nazwę FQDN.
* **Ustawienia puli serwerów zaplecza:** każda pula ma ustawienia, takie jak port, protokół i koligacja oparta na plikach cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu:** port publiczny, który jest otwierany w bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik:** odbiornik ma port frontonu, protokół (Http lub Https, z uwzględnieniem wielkości liter) oraz nazwę certyfikatu SSL (w przypadku konfigurowania odciążania protokołu SSL). Dla bramy aplikacji obsługującej obejmujący wiele lokacji nazwy hosta i wskaźniki SNI są również został dodany.
* **Reguła:** reguły wiąże odbiornika puli serwera zaplecza i definiuje puli serwera zaplecza, których ruch powinny być kierowane do, gdy trafienia w szczególności odbiornika. Reguły są przetwarzane w kolejności, w jakiej występują, a ruch zostanie skierowany przez pierwszą regułę odpowiadającą niezależnie od szczegółowością. Na przykład jeśli utworzono regułę przy użyciu odbiornika podstawowe i regułę przy użyciu odbiornika obejmujący wiele lokacji zarówno w tym samym porcie, reguły z wieloma lokacjami odbiornika musi być wymieniona przed regułę przy użyciu podstawowego odbiornika w kolejności reguły obejmujący wiele lokacji, aby działać zgodnie z oczekiwaniami. 
* **Certyfikaty:** każdego odbiornika wymaga unikatowego certyfikatu, w tym przykładzie 2 odbiorników są tworzone dla wielu witryn. Dwa certyfikaty PFX oraz hasła dla nich muszą zostać utworzone.

## <a name="create-back-end-pools-for-each-site"></a>Tworzenie pul zaplecza dla każdej lokacji

Dla każdej lokacji puli zaplecza, że aplikacja obsługuje bramy jest potrzebne, w tym przypadku 2 są można utworzyć, jeden dla contoso11.com i jeden dla fabrikam11.com.

### <a name="step-1"></a>Krok 1

Przejdź do istniejącej bramy aplikacji w portalu Azure (https://portal.azure.com). Wybierz **pul zaplecza** i kliknij przycisk **Dodaj**

![Dodawanie pul zaplecza][7]

### <a name="step-2"></a>Krok 2

Wprowadź informacje dla tej puli zaplecza **pool1**, dodawanie adresów ip lub nazwy FQDN dla serwerów zaplecza i kliknij przycisk **OK**

![Ustawienia pool1 puli wewnętrznej bazy danych][8]

### <a name="step-3"></a>Krok 3

W bloku pul zaplecza kliknij **Dodaj** można dodać dodatkowe puli zaplecza **pool2**, dodawanie adresów ip lub nazwy FQDN dla serwerów zaplecza i kliknij przycisk **OK**

![Ustawienia pool2 puli wewnętrznej bazy danych][9]

## <a name="create-listeners-for-each-back-end"></a>Tworzenie odbiorników dla każdego zaplecza

Usługa Application Gateway bazuje na nagłówkach hosta HTTP 1.1 w celu hostowania więcej niż jednej witryny sieci Web na tym samym publicznym adresie IP i porcie. Podstawowe odbiornika utworzone w portalu nie zawiera tej właściwości.

### <a name="step-1"></a>Krok 1

Kliknij przycisk **odbiorników** na istniejące bramy aplikacji i kliknij przycisk **obejmujący wiele lokacji** do dodania pierwszego odbiornika.

![obiekty nasłuchujące bloku — omówienie][1]

### <a name="step-2"></a>Krok 2

Wprowadź informacje dla odbiornika. W tym przykładzie protokół SSL jest skonfigurowany zakończenia, Utwórz nowy port serwera sieci Web. Przekaż certyfikat PFX, który ma być używany dla zakończenia połączenia SSL. Jedyną różnicą w tym bloku w porównaniu do bloku standardowe odbiornika podstawowe jest nazwą hosta.

![odbiornik bloku właściwości][2]

### <a name="step-3"></a>Krok 3

Kliknij przycisk **obejmujący wiele lokacji** i utwórz inny odbiornik, zgodnie z opisem w poprzednim kroku dla drugiej witryny. Upewnij się używała innego certyfikatu dla drugiego odbiornika. Jedyną różnicą w tym bloku w porównaniu do bloku standardowe odbiornika podstawowe jest nazwą hosta. Wypełnij informacje odbiornika i kliknij **OK**.

![odbiornik bloku właściwości][3]

> [!NOTE]
> Tworzenie odbiorników w portalu Azure dla aplikacji bramy jest długotrwałych zadań, może upłynąć trochę czasu, aby utworzyć dwa odbiorniki w tym scenariuszu. Po zakończeniu Pokaż odbiorników w portalu, jak pokazano na poniższej ilustracji:

![odbiornik — omówienie][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a>Tworzenie reguł do mapowania odbiorników pul zaplecza

### <a name="step-1"></a>Krok 1

Przejdź do istniejącej bramy aplikacji w portalu Azure (https://portal.azure.com). Wybierz **reguły** i wybrać istniejącą regułę domyślną **rule1** i kliknij przycisk **Edytuj**.

### <a name="step-2"></a>Krok 2

Wypełnij bloku reguł, jak pokazano na poniższej ilustracji. Wybieranie pierwszy odbiornika i pierwszej puli, a następnie klikając polecenie **zapisać** po zakończeniu.

![Edytuj istniejącą regułę][6]

### <a name="step-3"></a>Krok 3

Kliknij przycisk **podstawowe reguły** utworzyć drugą regułę. Wypełnij formularz drugi odbiornika, a drugie puli wewnętrznej bazy danych, a następnie kliknij przycisk **OK** do zapisania.

![Dodawanie bloku podstawowe reguły][10]

W tym scenariuszu kończy się konfigurowanie istniejącą bramę aplikacji z obsługą wielu lokacji za pośrednictwem portalu Azure.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak chronić witryny sieci Web z [Application Gateway - zapory aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
