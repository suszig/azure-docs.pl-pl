---
title: "Utwórz lub zaktualizuj bramę aplikacji za pomocą zapory aplikacji sieci web | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć bramę aplikacji za pomocą zapory aplikacji sieci web przy użyciu portalu"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: bfc06c1b44974fd17a3794654503d21d6407a917
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-by-using-the-portal"></a>Utwórz bramę aplikacji za pomocą zapory aplikacji sieci web przy użyciu portalu

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-web-application-firewall-cli.md)

Dowiedz się, jak utworzyć zapory aplikacji sieci web (WAF)-włączone bramy aplikacji.

Zapory aplikacji sieci Web w brama aplikacji w usłudze Azure chroni aplikacje sieci web przed wspólnej ataków opartych na sieci web takich jak iniekcja kodu SQL, ataki skryptów między witrynami i hijacks sesji. Zapory aplikacji sieci Web chroni przed wiele OWASP top 10 wspólnej sieci web luk w zabezpieczeniach.

## <a name="scenarios"></a>Scenariusze

W tym artykule przedstawiono dwa scenariusze. W przypadku pierwszego scenariusza, możesz dowiedzieć się, jak [Utwórz bramę aplikacji z zapory aplikacji sieci Web](#create-an-application-gateway-with-web-application-firewall). W tym scenariuszu drugiego, możesz dowiedzieć się, jak [Dodawanie zapory aplikacji sieci Web do istniejącej bramy aplikacji](#add-web-application-firewall-to-an-existing-application-gateway).

![Przykładowy scenariusz][scenario]

> [!NOTE]
> Można dodać sondy kondycji niestandardowych, adresy w puli zaplecza i dodatkowe reguły na bramie aplikacji. Te aplikacje są skonfigurowane, po skonfigurowaniu bramy aplikacji i nie podczas pierwszego wdrożenia.

## <a name="before-you-begin"></a>Przed rozpoczęciem

 Brama aplikacji wymaga jego własnej podsieci. Po utworzeniu sieci wirtualnej, upewnij się, pozostawienie wystarczającej przestrzeni adresowej do mają wiele podsieci. Po wdrożeniu bramę aplikacji do podsieci bramy tylko dodatkowych aplikacji można dodać do podsieci.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Dodawanie zapory aplikacji sieci web do istniejącej bramy aplikacji

W tym przykładzie aktualizuje istniejącą bramę aplikacji do obsługi zapory aplikacji sieci Web w **zapobiegania** tryb.

1. W portalu Azure **ulubione** okienku wybierz **wszystkie zasoby**. Na **wszystkie zasoby** bloku, wybierz istniejącą bramę aplikacji. Jeśli subskrypcja już ma kilka zasobów, wprowadź nazwę w **Filtruj według nazwy** pole, aby łatwo uzyskać dostęp do strefy DNS.

   ![Wybrane bramy aplikacji][1]

2. Wybierz **zapory aplikacji sieci Web**i zaktualizuj ustawienia bramy aplikacji. Po zakończeniu aktualizacji, zaznacz **zapisać**. 

3. Aby zaktualizować istniejącą bramę aplikacji do obsługi zapory aplikacji sieci Web, użyj następujących ustawień:

   | **Ustawienie** | **Wartość** | **Szczegóły**
   |---|---|---|
   |**Uaktualnij do planu zapory aplikacji sieci Web**| Zaznaczone | Ta opcja umożliwia ustawienie warstwy brama aplikacji w warstwie zapory aplikacji sieci Web.|
   |**Stan zapory**| Enabled (Włączony) | To ustawienie umożliwia włączenie zapory na zapory aplikacji sieci Web.|
   |**Trybie zapory** | Zapobieganie | To ustawienie jest sposób obsługi zapory aplikacji sieci Web z szkodliwy ruch. **Wykrywanie** tryb tylko dzienniki zdarzeń. **Zapobieganie** tryb dzienniki zdarzeń i zatrzymuje szkodliwy ruch.|
   |**Zestaw reguł**|3.0|To ustawienie określa [podstawowego zestawu reguł](application-gateway-web-application-firewall-overview.md#core-rule-sets) używany do ochrony członków puli zaplecza.|
   |**Konfigurowanie reguł wyłączone**|Zmienia się|Aby uniknąć możliwych fałszywych alarmów, można to ustawienie wyłączyć niektórych [reguł i grup reguł](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Po uaktualnieniu istniejącą bramę aplikacji do jednostki SKU zapory aplikacji sieci Web, rozmiar jednostki SKU zmienia się na **średni**. Po zakończeniu konfiguracji, można ponownie skonfigurować to ustawienie.

    ![Ustawienia podstawowe][2-1]

    > [!NOTE]
    > Aby wyświetlić dzienniki zapory aplikacji sieci Web, Włącz diagnostykę i wybierz **ApplicationGatewayFirewallLog**. Wybierz wystąpienia **1** tylko do celów testowych. Nie zaleca się liczba wystąpień, w obszarze **2** , ponieważ nie jest objęta umowy SLA. Mała bram nie są dostępne, gdy używasz zapory aplikacji sieci Web.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Utwórz bramę aplikacji za pomocą zapory aplikacji sieci web

W tym scenariuszu obejmują:

* Utwórz bramę aplikacji średnia zapory aplikacji sieci Web z dwoma wystąpieniami.
* Tworzenie sieci wirtualnej o nazwie AdatumAppGatewayVNET z zarezerwowanym blokiem CIDR 10.0.0.0/16.
* Tworzenie podsieci o nazwie Appgatewaysubnet używającej bloku 10.0.0.0/28 jako bloku CIDR.
* Konfigurowanie certyfikatu dla odciążania protokołu SSL.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). Jeśli nie masz już konto, możesz zarejestrować się w celu [bezpłatna miesięczna wersja próbna](https://azure.microsoft.com/free).

2. W **ulubione** okienku w portalu, wybierz opcję **nowy**.

3. Na **nowy** bloku, wybierz opcję **sieci**. Na **sieci** bloku, wybierz opcję **brama aplikacji w**, jak pokazano na poniższej ilustracji:

    ![Tworzenie bramy aplikacji][1]

4. Na **podstawy** bloku, zostanie wyświetlone, wprowadź następujące wartości, a następnie wybierz **OK**:

   | **Ustawienie** | **Wartość** | **Szczegóły**
   |---|---|---|
   |**Nazwa**|AdatumAppGateway|Nazwa bramy aplikacji.|
   |**Warstwy**|Zapora aplikacji sieci Web|Dostępne wartości to Standard i zapory aplikacji sieci Web. Aby dowiedzieć się więcej na temat zapory aplikacji sieci Web, zobacz [zapory aplikacji sieci Web](application-gateway-web-application-firewall-overview.md).|
   |**Rozmiar jednostki SKU**|Medium|Opcje warstwy standardowa **małych**, **średni**, i **duży**. Opcje warstwy zapory aplikacji sieci Web są **średni** i **duży** tylko.|
   |**Liczba wystąpień**|2|Liczba wystąpień brama aplikacji w celu zapewnienia wysokiej dostępności. Użyj liczby wystąpień 1 tylko do celów testowych.|
   |**Subskrypcja**|[Twoja subskrypcja]|Wybierz subskrypcję na potrzeby utworzenia bramy aplikacji.|
   |**Grupa zasobów**|**Utwórz nowy:** AdatumAppGatewayRG|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Lokalizacja**|Zachodnie stany USA||

   ![Podstawowe ustawienia konfiguracji][2-2]

5. Na **ustawienia** bloku, który jest wyświetlany w obszarze **sieci wirtualnej**, wybierz pozycję **wybierz sieć wirtualną**. Na **sieci wirtualnej wybierz** bloku, wybierz opcję **Utwórz nowy**.

   ![Wybór sieci wirtualnej][2]

6. Na **bloku sieci wirtualnej Utwórz**, wprowadź następujące wartości, a następnie wybierz **OK**. **Podsieci** na **ustawienia** bloku jest wypełniane przy użyciu wybranej podsieci.

   |**Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|AdatumAppGatewayVNET|Nazwa bramy aplikacji.|
   |**Przestrzeń adresowa**|10.0.0.0/16| Ta wartość jest przestrzeni adresowej dla sieci wirtualnej.|
   |**Nazwa podsieci**|AppGatewaySubnet|Nazwa podsieci bramy aplikacji.|
   |**Zakres adresów podsieci**|10.0.0.0/28 | Ta podsieć umożliwia więcej dodatkowe podsieci w sieci wirtualnej dla członków puli zaplecza.|

7. Na **ustawienia** bloku w obszarze **konfiguracji IP frontonu**, wybierz pozycję **publicznego** jako **typ adresu IP**.

8. Na **ustawienia** bloku w obszarze **publicznego adresu IP**, wybierz pozycję **wybierz publiczny adres IP**. Na **wybierz publiczny adres IP** bloku, wybierz opcję **Utwórz nowy**.

   ![Publiczny Wybór adresu IP][3]

9. Na **tworzenie publicznego adresu IP** bloku, zaakceptuj wartość domyślną, a następnie wybierz **OK**. **Publicznego adresu IP** pole jest wypełniane przy użyciu publicznego adresu IP wybrana.

10. Na **ustawienia** bloku w obszarze **konfiguracji odbiornika**, wybierz pozycję **HTTP** w obszarze **protokołu**. Certyfikat jest wymagany do użycia **HTTPS**. Klucz prywatny certyfikatu jest wymagana. Podaj do eksportu pliku PFX certyfikatu, a następnie wprowadź hasło dla pliku.

11. Skonfigurować ustawienia specyficzne dla **WAF**.

   |**Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Stan zapory**| Enabled (Włączony)| To ustawienie włącza zapory aplikacji sieci Web lub wyłącz.|
   |**Trybie zapory** | Zapobieganie| To ustawienie określa akcje, które przyjmuje zapory aplikacji sieci Web w szkodliwy ruch. **Wykrywanie** tryb tylko dzienników ruchu. **Zapobieganie** tryb dzienniki i zatrzymuje ruchu z nieautoryzowanego odpowiedź 403.|


12. Przegląd **Podsumowanie** i wybrać opcję **OK**. Brama aplikacji jest teraz w kolejce, a utworzona.

13. Po zastosowaniu utworzono bramę, przejdź do niego w portalu, aby kontynuować konfigurację bramy aplikacji.

    ![Widok zasobów bramy aplikacji][10]

Te kroki Utwórz bramę aplikacji w warstwie podstawowa z domyślnych ustawień odbiornika, puli zaplecza, ustawienia HTTP zaplecza i zasady. Pomyślnie, po zakończeniu inicjowania obsługi administracyjnej można zmodyfikować te ustawienia do potrzeb wdrożenia.

> [!NOTE]
> Bramy aplikacji utworzonych za pomocą podstawową konfigurację zapory aplikacji sieci Web są skonfigurowane z CRS 3.0 do ochrony.

## <a name="next-steps"></a>Następne kroki

Aby skonfigurować alias domeny niestandardowej dla [publicznego adresu IP](../dns/dns-custom-domain.md#public-ip-address), możesz użyć usługi Azure DNS lub innego dostawcy usługi DNS.

Aby skonfigurować rejestrowanie diagnostyczne do dziennika zdarzeń, które wykryte lub uniemożliwił z zapory aplikacji sieci Web, zobacz [diagnostyki bramy aplikacji](application-gateway-diagnostics.md).

Aby utworzyć sondy kondycji niestandardowych, zobacz [utworzyć sondy kondycji niestandardowych](application-gateway-create-probe-portal.md).

Aby skonfigurować odciążanie protokołu SSL, a następnie kosztowne subskrypcji SSL poza serwerów sieci web, zobacz [odciążania skonfigurować protokół SSL](application-gateway-ssl-portal.md).

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
