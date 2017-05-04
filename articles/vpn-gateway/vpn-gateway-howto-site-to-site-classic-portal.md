---
title: "Łączenie sieci lokalnej z siecią wirtualną platformy Azure: sieć VPN typu lokacja-lokacja (model klasyczny): portal | Microsoft Docs"
description: "Kroki tworzenia połączenia IPsec z sieci lokalnej do sieci wirtualnej platformy Azure za pośrednictwem publicznego Internetu. Kroki te są pomocne podczas tworzenia obejmującego wiele lokalizacji połączenia bramy sieci VPN typu lokacja-lokacja za pomocą portalu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: e5dcf957ea88175be02bce21929c43151417d0e3
ms.lasthandoff: 05/02/2017


---
# <a name="create-a-site-to-site-connection-using-the-azure-portal-classic"></a>Tworzenie sieci wirtualnej za pomocą połączenia typu lokacja-lokacja przy użyciu witryny Azure Portal (model klasyczny)

Ten artykuł pokazuje, jak używać witryny Azure Portal do tworzenia połączenia bramy sieci VPN lokacja-lokacja z sieci lokalnej do sieci wirtualnej. Kroki podane w tym artykule mają zastosowanie do klasycznego modelu wdrażania. Tę konfigurację możesz również utworzyć przy użyciu innego narzędzia wdrażania lub modelu wdrażania, wybierając inną opcję z następującej listy:

> [!div class="op_single_selector"]
> * [Resource Manager — witryna Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager — program PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager — interfejs wiersza polecenia](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasyczny — witryna Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasyczny — klasyczny portal](vpn-gateway-site-to-site-create.md)
> 
>

![Diagram połączenia bramy VPN Gateway typu lokacja-lokacja obejmującego wiele lokalizacji](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)


Połączenie bramy sieci VPN typu lokacja-lokacja umożliwia łączenie sieci lokalnej z siecią wirtualną platformy Azure za pośrednictwem tunelu sieci VPN IPsec/IKE (IKEv1 lub IKEv2). Ten typ połączenia wymaga lokalnego urządzenia sieci VPN z przypisanym publicznym adresem IP dostępnym z zewnątrz. Więcej informacji o bramach sieci VPN można znaleźć w artykule [Informacje dotyczące bram sieci VPN](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy są spełnione następujące kryteria:

* Upewnij się, że chcesz pracować z klasycznym modelem wdrażania. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Aby uzyskać więcej informacji o zgodnych urządzeniach sieci VPN i konfiguracji urządzeń, zobacz artykuł [Informacje o urządzeniach sieci VPN](vpn-gateway-about-vpn-devices.md).
* Dostępny zewnętrznie publiczny adres IPv4 urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
* Jeśli nie znasz zakresów adresów IP w konfiguracji swojej sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane. Tworząc tę konfigurację, musisz określić prefiksy zakresu adresów IP, które platforma Azure będzie kierować do Twojej lokalizacji lokalnej. Żadna z podsieci sieci lokalnej nie może się nakładać na podsieci sieci wirtualnej, z którymi chcesz nawiązać połączenie.
* Obecnie w celu określenia klucza współużytkowanego i utworzenia połączenia bramy sieci VPN jest wymagany program PowerShell. Zainstaluj najnowszą wersję poleceń cmdlet programu PowerShell usługi Azure Service Management (SM). Aby uzyskać więcej informacji, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview). Podczas pracy z programem PowerShell dla tej konfiguracji upewnij się, że działasz jako administrator. 

### <a name="values"></a>Przykładowe wartości konfiguracji dla tego ćwiczenia

W przykładach w tym artykule są stosowane następujące wartości. Tych wartości możesz użyć do tworzenia środowiska testowego lub odwoływać się do nich, aby lepiej zrozumieć przykłady w niniejszym artykule.

* **Nazwa sieci wirtualnej:** TestVNet1
* **Przestrzeń adresowa:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (opcjonalnie na potrzeby tego ćwiczenia)
* **Podsieci:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (opcjonalnie na potrzeby tego ćwiczenia)
* **GatewaySubnet:** 10.11.255.0/27
* **Grupa zasobów:** TestRG1
* **Lokalizacja:** Wschodnie stany USA
* **Serwer DNS:** 8.8.8.8 (opcjonalnie na potrzeby tego ćwiczenia)
* **Nazwa lokacji lokalnej:** Lokacja2

## <a name="CreatVNet"></a>1. Tworzenie sieci wirtualnej

Podczas tworzenia sieci wirtualnej na potrzeby połączenia typu lokacja-lokacja upewnij się, że określone przestrzenie adresowe nie nakładają się na żadne przestrzenie adresowe klientów dla lokacji lokalnych, z którymi chcesz nawiązywać połączenia. Obecność nakładających się podsieci spowoduje, że połączenie nie będzie działać prawidłowo.

* Jeśli masz już sieć wirtualną, sprawdź, czy ustawienia są zgodne z projektem bramy sieci VPN. Zwróć szczególną uwagę na wszelkie podsieci, które mogą pokrywać się z innymi sieciami. 

* Jeśli nie masz jeszcze sieci wirtualnej, utwórz ją. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi.

### <a name="to-create-a-virtual-network"></a>Aby utworzyć sieć wirtualną

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.

    ![Wyszukiwanie bloku Sieć wirtualna](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Klasyczny**, a następnie kliknij przycisk **Utwórz**.

    ![Wybieranie modelu wdrożenia](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. W tym bloku dodaj pierwszą przestrzeń adresową i zakres adresów pojedynczej podsieci. Po zakończeniu tworzenia sieci wirtualnej możesz wrócić i dodać dodatkowe podsieci oraz przestrzenie adresowe.

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Tworzenie bloku sieci wirtualnej")
5. Sprawdź, czy pole **Subskrypcja** zawiera prawidłową wartość. Subskrypcje można zmieniać, korzystając z listy rozwijanej.
6. Kliknij przycisk **Grupy zasobów** i wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Następnie w polu **Lokalizacja** wybierz ustawienia sieci wirtualnej. Lokalizacja określa miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.
8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.

    ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "Przypnij do pulpitu nawigacyjnego")
9. Po kliknięciu przycisku „Utwórz” na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

    ![Kafelek Tworzenie sieci wirtualnej](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Tworzenie sieci wirtualnej")

Po utworzeniu sieci wirtualnej w obszarze **Stan** na stronie sieci w klasycznej witrynie Azure Portal pojawi się wartość **Utworzona**.

## <a name="additionaladdress"></a>2. Dodawanie kolejnej przestrzeni adresowej

Po utworzeniu sieci wirtualnej możesz dodać kolejną przestrzeń adresową. Dodanie kolejnej przestrzeni adresowej nie jest wymagane w przypadku konfiguracji typu lokacja-lokacja, ale jeśli potrzebujesz wielu przestrzeni adresowych, wykonaj następujące kroki:

1. Znajdź sieci wirtualne w portalu.
2. W bloku Twojej sieci wirtualnej, w sekcji **Ustawienia** kliknij pozycję **Przestrzeń adresowa**.
3. W bloku Przestrzeń adresowa kliknij pozycję **+Dodaj** i wprowadź dodatkową przestrzeń adresową.
 
## <a name="dns"></a>3. Określanie serwera DNS
Ustawienia DNS nie są wymagane w przypadku konfiguracji typu lokacja-lokacja, ale serwer DNS jest konieczny, aby korzystać z rozpoznawania nazw.

Po utworzeniu sieci wirtualnej możesz dodać adres IP serwera DNS, aby umożliwić obsługę rozpoznawania nazw. Otwórz ustawienia dla sieci wirtualnej, kliknij pozycję Serwery DNS i dodaj adres IP serwera DNS, który ma być używany do rozpoznawania nazw. To ustawienie nie powoduje jednak utworzenia serwera DNS. W ustawieniach przykładowych używamy publicznego serwera DNS. Zazwyczaj zalecane jest użycie prywatnego serwera DNS. Należy pamiętać o dodaniu serwera DNS, z którym zasoby mogą się komunikować.

1. Znajdź sieci wirtualne w portalu.
2. W bloku Twojej sieci wirtualnej, w sekcji **Ustawienia** kliknij pozycję **Serwery DNS**.
3. Dodaj serwer DNS.
4. Aby zapisać ustawienia, kliknij przycisk **Zapisz** znajdujący się u góry strony.
 
## <a name="localsite"></a>4. Konfigurowanie lokacji lokalnej

Lokacja lokalna zazwyczaj oznacza lokalizację lokalną. Zawiera ona adres IP urządzenia sieci VPN, z którym będzie tworzone połączenie, oraz zakresy adresów IP, które będą kierowane za pośrednictwem bramy sieci VPN do tego urządzenia sieci VPN.

1. W witrynie portalu przejdź do sieci wirtualnej, dla której chcesz utworzyć bramę.
2. W bloku **Przegląd** Twojej sieci wirtualnej, w sekcji Połączenia VPN kliknij pozycję **Brama**, aby otworzyć blok **Nowe połączenie VPN**.

    ![Kliknij, aby skonfigurować ustawienia bramy](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "Kliknij, aby skonfigurować ustawienia bramy")
3. W bloku **Nowe połączenie VPN** wybierz pozycję **Lokacja-lokacja**.

    ![Kliknij pozycję Lokacja-lokacja](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site.png "Lokacja-lokacja")
4. Kliknij pozycję **Lokacja lokalna — Skonfiguruj wymagane ustawienia**, aby otworzyć blok **Lokacja lokalna**. Skonfiguruj ustawienia, a następnie kliknij przycisk **OK**, aby je zapisać.
    - **Nazwa:** Utwórz nazwę lokacji lokalnej, aby ułatwić jej identyfikację.
    - **Adres IP bramy sieci VPN:** Publiczny adres IP urządzenia sieci VPN w sieci lokalnej. Urządzenie sieci VPN wymaga publicznego adresu IPv4. Określ prawidłowy publiczny adres IP dla urządzenia sieci VPN, z którym chcesz się połączyć. Nie może ono znajdować się za translatorem adresów sieciowych i musi być dostępne za pomocą usługi Azure.
    - **Przestrzeń adresowa klienta:** Podaj listę zakresów adresów IP, które mają być kierowane do sieci lokalnej za pośrednictwem tej bramy. Można dodać wiele zakresów przestrzeni adresów. Upewnij się, że określone w tym miejscu zakresy nie pokrywają się z zakresami innych sieci, z którymi łączy się Twoja sieć wirtualna, ani z zakresami adresów samej sieci wirtualnej.

    ![Lokacja lokalna](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Konfigurowanie lokacji lokalnej")

## <a name="gatewaysubnet"></a>5. Konfigurowanie podsieci bramy

Musisz utworzyć podsieć dla bramy sieci VPN. Podsieć bramy zawiera adresy IP, z których korzystają usługi bramy sieci VPN.

1. W bloku **Nowe połączenie VPN** zaznacz pole wyboru **Utwórz bramę natychmiast**. Zostanie wyświetlony blok „Opcjonalna konfiguracja bramy”. Jeśli nie zaznaczysz pola wyboru, blok umożliwiający skonfigurowanie podsieci bramy nie zostanie wyświetlony.

    ![Konfiguracja bramy — Podsieć, rozmiar, typ routingu](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Konfiguracja bramy — Podsieć, rozmiar, typ routingu")
2. Kliknij pozycję **Opcjonalna konfiguracja bramy — Podsieć, rozmiar i typ routingu**, aby otworzyć blok **Konfiguracja bramy**.
3. W bloku **Konfiguracja bramy** kliknij pozycję **Podsieć — Skonfiguruj wymagane ustawienia**, aby otworzyć blok **Dodawanie podsieci**.

    ![Konfiguracja bramy — Podsieć bramy](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Konfiguracja bramy — Podsieć bramy")
4. W bloku **Dodawanie podsieci** dodaj podsieć bramy. Rozmiar określanej podsieci bramy zależy od konfiguracji bramy sieci VPN, którą chcesz utworzyć. Chociaż możliwe jest utworzenie podsieci bramy tak małej, jak /29, zaleca się wybranie większej podsieci /27 lub /28, aby zawierała więcej adresów. Zastosowanie większej podsieci bramy daje wystarczającą liczbę adresów IP, aby uwzględnić możliwe przyszłe konfiguracje.

    ![Dodawanie podsieci bramy](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Dodawanie podsieci bramy")

## <a name="sku"></a>6. Określ typ jednostki SKU i sieci VPN
1. Wybierz **rozmiar** bramy. To jest jednostka SKU bramy używana do tworzenia bramy sieci wirtualnej. W portalu domyślna jednostka SKU to **Podstawowa**. Więcej informacji o jednostkach SKU bramy zawiera artykuł [Informacje o ustawieniach bramy VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Wybieranie typu jednostki SKU i sieci VPN](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Wybieranie typu jednostki SKU i sieci VPN")
2. Wybierz **Typ routingu** dla bramy. Jest on również nazywany typem sieci VPN. Wybranie poprawnego typu bramy jest bardzo ważne, ponieważ nie można konwertować bramy na inny typ. Urządzenie sieci VPN musi być zgodne z wybranym typem routingu. Więcej informacji o typie sieci VPN można znaleźć w artykule [Informacje na temat ustawień usługi VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype). W niektórych artykułach mogą znajdować się odwołania do typów sieci VPN „RouteBased” i „PolicyBased”. Typ „Dynamiczny” odpowiada typowi „RouteBased”, a „Statyczny” — typowi „PolicyBased”.
3. Kliknij pozycję **OK**, aby zapisać ustawienia.
4. W bloku **Nowe połączenie VPN** kliknij przycisk **OK** u dołu bloku, aby rozpocząć tworzenie bramy sieci wirtualnej. Może to potrwać do 45 minut.

## <a name="vpndevice"></a>7. Konfiguracja urządzenia sieci VPN

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Tworzenie połączenia
W tym kroku należy ustawić klucz współużytkowany i utworzyć połączenie. Ustawiony klucz musi być tym samym kluczem, którego użyto do skonfigurowania urządzenia sieci VPN.

> [!NOTE]
> Obecnie tego kroku nie można wykonać w witrynie Azure Portal. Musisz użyć wersji poleceń cmdlet programu Azure PowerShell pochodzącej z usługi Service Management (SM).
>

### <a name="step-1-connect-to-your-azure-account"></a>Krok 1. Nawiąż połączenie z kontem platformy Azure

1. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

  ```powershell
  Login-AzureRmAccount
  ```
2. Sprawdź subskrypcje dostępne na koncie.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Jeśli masz więcej niż jedną subskrypcję, wybierz tę, której chcesz użyć.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
4. Dodaj wersję poleceń cmdlet programu PowerShell pochodzącą z usługi SM.

  ```powershell
  Add-AzureAccount
  ```

### <a name="step-2-set-the-shared-key-and-create-the-connection"></a>Krok 2. Ustaw klucz współużytkowany i utwórz połączenie

Podczas pracy z programem PowerShell w klasycznym modelu wdrażania czasami nazwy zasobów w portalu nie są nazwami, których platforma Azure oczekuje przy korzystaniu z programu PowerShell. Poniższe kroki pomogą Ci wyeksportować plik konfiguracji sieci, aby uzyskać dokładne wartości nazw.

1. Utwórz katalog na komputerze, a następnie wyeksportuj plik konfiguracji sieci do tego katalogu. W tym przykładzie plik konfiguracji sieci zostanie wyeksportowany do katalogu C:\AzureNet.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Otwórz plik konfiguracji sieci przy użyciu edytora xml, a następnie sprawdź wartości właściwości „LocalNetworkSite name” i „VirtualNetworkSite name”. Zmodyfikuj przykład tak, aby odzwierciedlał te wartości. W przypadku podawania nazwy, która zawiera spacje, umieść wartość w apostrofach.

3. Ustaw klucz współużytkowany i utwórz połączenie. Wartość „-SharedKey” jest generowana i określana przez Ciebie. W tym przykładzie użyliśmy wartości „abc123”, ale można (i należy) wygenerować bardziej złożoną wartość. Pamiętaj o tym, że wartość podana w tym miejscu musi być taka sama, jak wartość podana podczas konfigurowania urządzenia sieci VPN.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
  -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
  ```
Wynik po utworzeniu połączenia: **Stan: Powodzenie**.

## <a name="verify"></a>9. Weryfikowanie połączenia

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="next-steps"></a>Następne kroki
Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Aby uzyskać więcej informacji, zobacz [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) (Maszyny wirtualne).


