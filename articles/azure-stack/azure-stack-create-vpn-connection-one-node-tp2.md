---
title: "Tworzenie połączenia sieci VPN lokacja-lokacja między dwiema sieciami wirtualnymi w różnych środowiskach weryfikacji koncepcji usługi Azure Stack | Microsoft Docs"
description: "Procedura krok po kroku, która umożliwia administratorowi chmury utworzenie połączenia sieci VPN lokacja-lokacja między dwoma środowiskami weryfikacji koncepcji z jednym węzłem w wersji Technical Preview&2;."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: scottnap
translationtype: Human Translation
ms.sourcegitcommit: 5104c7996de9dc0597e65be31c28a9aaa1243a90
ms.openlocfilehash: d324290caf5b5a085a2daf67e541c295dffda732


---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-poc-environments"></a>Tworzenie połączenia sieci VPN lokacja-lokacja między dwiema sieciami wirtualnymi w różnych środowiskach weryfikacji koncepcji usługi Azure Stack
## <a name="overview"></a>Omówienie
W tym artykule przedstawiono sposób tworzenia połączenia sieci VPN lokacja-lokacja między dwiema sieciami wirtualnymi w dwóch oddzielnych środowiskach weryfikacji koncepcji (POC, Proof-of-Concept) usługi Azure Stack. Podczas konfigurowania połączeń dowiesz się, jak działają bramy sieci VPN w usłudze Azure Stack.

> [!NOTE]
> Ten dokument dotyczy w szczególności weryfikacji koncepcji w usłudze Azure Stack Technical Preview&2;.
> 
> 

### <a name="connection-diagram"></a>Diagram połączenia
Na poniższym diagramie przedstawiono, jak powinna wyglądać konfiguracja po wykonaniu tych czynności:

![](media/azure-stack-create-vpn-connection-one-node-tp2/image1.png)

### <a name="before-you-begin"></a>Przed rozpoczęciem
Aby wykonać tę konfigurację, przed rozpoczęciem upewnij się, że są dostępne następujące elementy:

* Dwa serwery, które spełniają wymagania sprzętowe weryfikacji koncepcji usługi Azure Stack zdefiniowane w temacie [Wymagania wstępne dotyczące wdrożenia usługi Azure Stack](azure-stack-deploy.md) i inne wymagania wstępne zdefiniowane w tym dokumencie.
* Pakiet wdrożeniowy usługi Azure Stack Technical Preview 2.

## <a name="deploy-the-poc-environments"></a>Wdrażanie środowisk weryfikacji koncepcji
W celu ukończenia tej konfiguracji wdrożysz dwa środowiska weryfikacji koncepcji usługi Azure Stack.

* Dla każdego wdrażanego środowiska weryfikacji koncepcji możesz postępować zgodnie z instrukcjami wdrażania podanymi w artykule [Wdrażanie usługi Azure Stack POC](azure-stack-run-powershell-script.md).
  Poszczególne środowiska weryfikacji koncepcji w tym dokumencie są ogólnie nazywane *POC1* i *POC2*.

## <a name="configure-quotas-for-compute-network-and-storage"></a>Konfigurowanie limitów przydziału zasobów obliczeniowych, sieciowych i magazynu
Najpierw należy skonfigurować *limity przydziału* zasobów obliczeniowych, sieciowych i magazynu. Te usługi mogą zostać skojarzone z *planem*, a następnie *ofertą*, która może być subskrybowana przez dzierżawy.

> [!NOTE]
> Te kroki należy wykonać dla każdego środowiska weryfikacji koncepcji usługi Azure Stack.
> 
> 

Sposób tworzenia limitów przydziału jest inny niż w wersji Technical Preview&1;. Kroki służące do tworzenia limitów przydziału w wersji Technical Preview&2; możesz znaleźć pod adresem <http://aka.ms/mas-create-quotas>. Na potrzeby tego ćwiczenia możesz zaakceptować wartości domyślne dla wszystkich ustawień limitów przydziału.

## <a name="create-a-plan-and-offer"></a>Tworzenie planu i oferty
[Plany](azure-stack-key-features.md) są grupami obejmującymi co najmniej jedną usługę. Jako dostawca możesz utworzyć plany do zaoferowania dzierżawcom. Z kolei dzierżawcy mogą subskrybować oferty, aby korzystać z planów i objętych nimi usług.

> [!NOTE]
> Te kroki należy wykonać dla każdego środowiska weryfikacji koncepcji usługi Azure Stack.
> 
> 

1. Najpierw utwórz plan. Aby to zrobić, możesz wykonać kroki podane w artykule online [Tworzenie planu](azure-stack-create-plan.md).
2. Utwórz ofertę, wykonując kroki opisane w artykule [Tworzenie oferty w usłudze Azure Stack](azure-stack-create-offer.md).
3. Zaloguj się do portalu jako administrator dzierżawy i [subskrybuj utworzoną ofertę](azure-stack-subscribe-plan-provision-vm.md).

## <a name="create-the-network-resources-in-poc-1"></a>Tworzenie zasobów sieciowych w środowisku POC1
Teraz faktycznie utworzysz zasoby potrzebne do przeprowadzenia konfiguracji. Poniższe kroki ilustrują wykonywane czynności. W ramach tych instrukcji przedstawiono sposób tworzenia zasobów za pomocą portalu, ale ten sam efekt można osiągnąć przy użyciu programu PowerShell.

![](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="log-in-as-a-tenant"></a>Logowanie jako dzierżawca
Administrator usługi może zalogować się jako dzierżawca, aby przetestować plany, oferty i subskrypcje, z których mogą korzystać dzierżawcy. Przed zalogowaniem się należy [utworzyć konto dzierżawcy](azure-stack-add-new-user-aad.md), jeśli jeszcze go nie ma.

### <a name="create-the-virtual-network--vm-subnet"></a>Tworzenie sieci wirtualnej i podsieci maszyny wirtualnej
1. Zaloguj się przy użyciu konta dzierżawcy.
2. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)
3. Wybierz pozycję **Sieć** z menu portalu Marketplace.
4. Kliknij element **Sieć wirtualna** w menu.
5. Kliknij pozycję **Utwórz** u dołu bloku opisu zasobu. Wprowadź następujące wartości w odpowiednich polach zgodnie z poniższą tabelą.
   
   | **Pole** | **Wartość** |
   | --- | --- |
   | Nazwa |vnet-01 |
   | Przestrzeń adresowa |10.0.10.0/23 |
   | Nazwa podsieci |subnet-01 |
   | Zakres adresów podsieci |10.0.10.0/24 |
6. W polu **Subskrypcja** powinna znajdować się subskrypcja utworzona wcześniej.
7. Grupę zasobów można utworzyć nową lub, jeśli grupa już istnieje, wybrać pozycję **Użyj istniejącej**.
8. Sprawdź lokalizację domyślną.
9. Kliknij pozycję **Utwórz**.

### <a name="create-the-gateway-subnet"></a>Tworzenie podsieci bramy
1. Otwórz właśnie utworzony zasób sieci wirtualnej (vnet-01) z poziomu pulpitu nawigacyjnego.
2. W bloku Ustawienia wybierz pozycję **Podsieci**.
3. Kliknij przycisk **Podsieć bramy**, aby dodać podsieć bramy do sieci wirtualnej.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)
4. Domyślna nazwa podsieci to **GatewaySubnet**.
   Podsieci bramy to specjalne podsieci i muszą mieć dokładnie tę nazwę, aby działać prawidłowo.
5. W polu **Zakres adresów** wpisz wartość **10.0.11.0/24**.
6. Kliknij pozycję **Utwórz**, aby utworzyć podsieć bramy.

### <a name="create-the-virtual-network-gateway"></a>Tworzenie bramy sieci wirtualnej
1. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
2. Wybierz pozycję **Sieć** z menu portalu Marketplace.
3. Wybierz pozycję **Brama sieci wirtualnej** z listy zasobów sieciowych.
4. Przejrzyj opis, a następnie kliknij pozycję **Utwórz**.
5. W polu **Nazwa** wpisz wartość **GW1**.
6. Kliknij pozycję **Sieć wirtualna**, aby wybrać sieć wirtualną.
   Z listy wybierz pozycję **vnet-01**.
7. Kliknij element menu **Publiczny adres IP**. Po otworzeniu bloku **Wybór publicznego adresu IP** kliknij pozycję **Utwórz nowy**.
8. W polu **Nazwa** wpisz wartość **GW1-PiP** i kliknij przycisk **OK**.
9. W polu **Typ bramy** powinna być domyślnie wybrana pozycja **VPN**. Zachowaj to ustawienie.
10. W polu **Typ sieci VPN** powinna być domyślnie wybrana pozycja **Oparta na trasach**.
    Zachowaj to ustawienie.
11. Upewnij się, że wartości w polach **Subskrypcja** i **Lokalizacja** są poprawne. Jeśli chcesz, możesz przypiąć zasób do pulpitu nawigacyjnego. Kliknij pozycję **Utwórz**.

### <a name="create-the-local-network-gateway"></a>Tworzenie bramy sieci lokalnej
Implementacja *bramy sieci lokalnej* w tym wdrożeniu na potrzeby oceny usługi Azure Stack jest nieco inna niż w rzeczywistym wdrożeniu platformy Azure.

Dokładnie tak, jak w przypadku platformy Azure, istnieje pojęcie bramy sieci lokalnej. Jednak we wdrożeniu platformy Azure brama sieci lokalnej reprezentuje lokalne (w dzierżawie) urządzenie fizyczne używane do nawiązywania połączeń z bramą sieci wirtualnej na platformie Azure. Natomiast w tym wdrożeniu na potrzeby oceny usługi Azure Stack po obu stronach połączenia znajdują się bramy sieci wirtualnej.

Bardziej ogólnie można przyjąć, że zasób Brama sieci lokalnej ma zawsze wskazywać bramę zdalną po drugiej stronie połączenia. Ze względu na sposób, w jaki zaprojektowano środowisko weryfikacji koncepcji, musisz podać adres IP zewnętrznej karty sieciowej na maszynie wirtualnej translatora adresów sieciowych drugiego środowiska weryfikacji koncepcji jako publiczny adres IP bramy sieci lokalnej. Następnie utworzysz mapowania translatora adresów sieciowych na maszynie wirtualnej translatora adresów sieciowych, aby upewnić się, że oba środowiska zostaną prawidłowo połączone.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Uzyskiwanie adresu IP zewnętrznej karty sieciowej maszyny wirtualnej translatora adresów sieciowych
1. Zaloguj się do maszyny fizycznej usługi Azure Stack używanej dla środowiska POC2.
2. Naciśnij klawisze Windows + R, aby otworzyć menu **Uruchom**, wpisz polecenie **mstsc** i naciśnij klawisz Enter.
3. W polu **Komputer** wpisz nazwę **MAS-BGPNAT01** i kliknij pozycję  **Połącz**.
4. Kliknij menu Start, a następnie kliknij prawym przyciskiem myszy program **Windows PowerShell** i wybierz polecenie **Uruchom jako administrator**.
5. Wpisz polecenie **ipconfig /all**.
6. Znajdź kartę sieciową Ethernet, która jest połączona z siecią lokalną, i zanotuj adres IPv4 powiązany z tą kartą. W naszym środowisku przykładowym ten adres to **10.16.167.195**, ale Twój będzie inny.
7. Zapisz ten adres. Będzie on używany jako publiczny adres IP zasobu Brama sieci lokalnej utworzonego w środowisku POC1.

### <a name="create-the-local-network-gateway-resource"></a>Tworzenie zasobu Brama sieci lokalnej
1. Zaloguj się do maszyny fizycznej usługi Azure Stack używanej dla środowiska POC1.
2. W polu **Komputer** wpisz nazwę **MAS-CON01** i kliknij pozycję **Połącz**.
3. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
4. Wybierz pozycję **Sieć** z menu portalu Marketplace.
5. Wybierz pozycję **brama sieci lokalnej** z listy zasobów.
6. W polu **Nazwa** wpisz wartość **POC2-GW**.
7. Nie znasz jeszcze adresu IP drugiej bramy, ale nie stanowi to problemu, ponieważ później możesz wrócić i go zmienić. Na razie wpisz wartość **10.16.167.195** w polu **Adres IP**.
8. W polu **Przestrzeń adresowa** wpisz przestrzeń adresową sieci wirtualnej, która zostanie utworzona w środowisku POC2. Będzie to **10.0.20.0/23**, więc wpisz tę wartość.
9. Upewnij się, że wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja** są poprawne, a następnie kliknij pozycję **Utwórz**.

### <a name="create-the-connection"></a>Tworzenie połączenia
1. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
2. Wybierz pozycję **Sieć** z menu portalu Marketplace.
3. Wybierz pozycję **Połączenie** z listy zasobów.
4. W bloku ustawień **Podstawy** wybierz pozycję **Lokacja-lokacja (IPsec)** jako **Typ połączenia**.
5. Wybierz wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**, a następnie kliknij przycisk **OK**.
6. W bloku **Ustawienia** wybierz utworzoną wcześniej **bramę sieci wirtualnej** (**GW1**).
7. Wybierz utworzoną wcześniej **bramę sieci lokalnej** (**POC2-GW**).
8. W polu **Nazwa połączenia** wpisz wartość **POC1-POC2**.
9. W polu **Klucz wspólny (PSK)** wpisz wartość **12345** i kliknij przycisk **OK**.

### <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej
Aby walidować dane przesyłane przez połączenie sieci VPN, potrzebne są maszyny wirtualne do wysyłania i odbierania danych w każdym środowisku weryfikacji koncepcji. Utwórz teraz maszynę wirtualną w środowisku POC1 i umieść ją w podsieci maszyny wirtualnej w sieci wirtualnej.

1. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
2. Wybierz pozycję **Maszyny wirtualne** z menu portalu Marketplace.
3. Na liście obrazów maszyn wirtualnych wybierz obraz **Windows Server 2012 R2 Datacenter**.
4. W bloku **Podstawy**, w polu **Nazwa** wpisz wartość **VM01**.
5. Wpisz prawidłową nazwę użytkownika i hasło. To konto będzie używane w celu logowania się do maszyny wirtualnej po jej utworzeniu.
6. Podaj wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**, a następnie kliknij przycisk **OK**.
7. W bloku **Rozmiar** wybierz rozmiar maszyny wirtualnej dla tego wystąpienia, a następnie kliknij pozycję **Wybierz**.
8. W bloku **Ustawienia** możesz zaakceptować wartości domyślne. Upewnij się jedynie, że wybrana sieć wirtualna to **vnet-01**, a w polu Podsieć jest podana wartość **10.0.10.0/24**. Kliknij przycisk **OK**.
9. Sprawdź ustawienia w bloku **Podsumowanie** i kliknij przycisk **OK**.

## <a name="create-the-network-resources-in-poc-2"></a>Tworzenie zasobów sieciowych w środowisku POC2
### <a name="log-in-as-a-tenant"></a>Logowanie jako dzierżawca
Administrator usługi może zalogować się jako dzierżawca, aby przetestować plany, oferty i subskrypcje, z których mogą korzystać dzierżawcy. Przed zalogowaniem się należy [utworzyć konto dzierżawcy](azure-stack-add-new-user-aad.md), jeśli jeszcze go nie ma.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Tworzenie sieci wirtualnej i podsieci maszyny wirtualnej
1. Zaloguj się przy użyciu konta dzierżawcy.
2. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
3. Wybierz pozycję **Sieć** z menu portalu Marketplace.
4. Kliknij element **Sieć wirtualna** w menu.
5. Kliknij pozycję **Utwórz** u dołu bloku opisu zasobu. Wpisz następujące wartości w odpowiednich polach wymienionych w poniższej tabeli.
   
   | **Pole** | **Wartość** |
   | --- | --- |
   | Nazwa |vnet-02 |
   | Przestrzeń adresowa |10.0.20.0/23 |
   | Nazwa podsieci |subnet-02 |
   | Zakres adresów podsieci |10.0.20.0/24 |
6. W polu **Subskrypcja** powinna znajdować się subskrypcja utworzona wcześniej.
7. Grupę zasobów można utworzyć nową lub, jeśli grupa już istnieje, wybrać pozycję **Użyj istniejącej**.
8. Sprawdź lokalizację domyślną w polu **Lokalizacja**. Jeśli chcesz, możesz przypiąć sieć wirtualną do pulpitu nawigacyjnego, aby zapewnić sobie łatwy dostęp.
9. Kliknij pozycję **Utwórz**.

### <a name="create-the-gateway-subnet"></a>Tworzenie podsieci bramy
1. Otwórz właśnie utworzony zasób sieci wirtualnej (**vnet-02**) z poziomu pulpitu nawigacyjnego.
2. W bloku **Ustawienia** wybierz pozycję **Podsieci**.
3. Kliknij przycisk **Podsieć bramy**, aby dodać podsieć bramy do sieci wirtualnej.
   
4. Domyślna nazwa podsieci to **GatewaySubnet**.
   Podsieci bramy to specjalne podsieci i muszą mieć dokładnie tę nazwę, aby działać prawidłowo.
5. W polu **Zakres adresów** wpisz wartość **10.0.20.0/24**.
6. Kliknij pozycję **Utwórz**, aby utworzyć podsieć bramy.

### <a name="create-the-virtual-network-gateway"></a>Tworzenie bramy sieci wirtualnej
1. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
2. Wybierz pozycję **Sieć** z menu portalu Marketplace.
3. Wybierz pozycję **Brama sieci wirtualnej** z listy zasobów sieciowych.
4. Przejrzyj opis, a następnie kliknij pozycję **Utwórz**.
5. W polu **Nazwa** wpisz wartość **GW2**.
6. Kliknij pozycję **Sieć wirtualna**, aby wybrać sieć wirtualną.
   Z listy wybierz pozycję **vnet-02**.
7. Kliknij pozycję **Publiczny adres IP**. Po otworzeniu bloku **Wybór publicznego adresu IP** kliknij pozycję **Utwórz nowy**.
8. W polu **Nazwa** wpisz wartość **GW2-PiP** i kliknij przycisk **OK**.
9. W polu **Typ bramy** powinna być domyślnie wybrana pozycja **VPN**. Zachowaj to ustawienie.
10. W polu **Typ sieci VPN** powinna być domyślnie wybrana pozycja **Oparta na trasach**.
    Zachowaj to ustawienie.
11. Upewnij się, że wartości w polach **Subskrypcja** i **Lokalizacja** są poprawne. Jeśli chcesz, możesz przypiąć zasób do pulpitu nawigacyjnego. Kliknij przycisk **Utwórz**.

### <a name="create-the-local-network-gateway"></a>Tworzenie bramy sieci lokalnej
#### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Uzyskiwanie adresu IP zewnętrznej karty sieciowej maszyny wirtualnej translatora adresów sieciowych
1. Zaloguj się do maszyny fizycznej usługi Azure Stack używanej dla środowiska POC1.
2. Naciśnij i przytrzymaj klawisze Windows + R, aby otworzyć menu **Uruchom**, wpisz polecenie **mstsc** i naciśnij klawisz Enter.
3. W polu **Komputer** wpisz nazwę **MAS-BGPNAT01** i kliknij pozycję  **Połącz**.
4. Kliknij menu Start, kliknij prawym przyciskiem myszy program **Windows PowerShell** i wybierz polecenie **Uruchom jako administrator**.
5. Wpisz polecenie **ipconfig /all**.
6. Znajdź kartę sieciową Ethernet, która jest połączona z siecią lokalną, i zanotuj adres IPv4 powiązany z tą kartą. W środowisku przykładowym ten adres to **10.16.169.131**, ale Twój będzie inny.
7. Zapisz ten adres. Będzie on potrzebny później jako publiczny adres IP zasobu Brama sieci lokalnej utworzonego w środowisku POC1.

#### <a name="create-the-local-network-gateway-resource"></a>Tworzenie zasobu Brama sieci lokalnej
1. Zaloguj się do maszyny fizycznej usługi Azure Stack używanej dla środowiska POC2.
2. W polu **Komputer** wpisz nazwę **MAS-CON01** i kliknij pozycję **Połącz**.
3. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
4. Wybierz pozycję **Sieć** z menu portalu Marketplace.
5. Wybierz pozycję **brama sieci lokalnej** z listy zasobów.
6. W polu **Nazwa** wpisz wartość **POC1-GW**.
7. Teraz będzie potrzebny publiczny adres IP zarejestrowany dla bramy sieci wirtualnej w środowisku POC1. Wpisz wartość **10.16.169.131** w polu **Adres IP**.
8. W polu **Przestrzeń adresowa** wpisz przestrzeń adresową sieci **vnet-01** ze środowiska POC1 — **10.0.0.0/16**.
9. Upewnij się, że wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja** są poprawne, a następnie kliknij pozycję **Utwórz**.

## <a name="create-the-connection"></a>Tworzenie połączenia
1. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
2. Wybierz pozycję **Sieć** z menu portalu Marketplace.
3. Wybierz pozycję **Połączenie** z listy zasobów.
4. W bloku ustawień **Podstawy** wybierz pozycję **Lokacja-lokacja (IPsec)** jako **Typ połączenia**.
5. Wybierz wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**, a następnie kliknij przycisk **OK**.
6. W bloku **Ustawienia** wybierz utworzoną wcześniej **bramę sieci wirtualnej** (**GW1**).
7. Wybierz utworzoną wcześniej **bramę sieci lokalnej** (**POC1-GW**).
8. W polu **Nazwa połączenia** wpisz wartość **POC2-POC1**.
9. W polu **Klucz wspólny (PSK)** wpisz wartość **12345**. W przypadku wybrania innej wartości pamiętaj, że musi ona odpowiadać wartości klucza wspólnego utworzonego w środowisku POC1. Kliknij przycisk **OK**.

## <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej
Utwórz teraz maszynę wirtualną w środowisku POC1 i umieść ją w podsieci maszyny wirtualnej w sieci wirtualnej.

1. W witrynie Azure Portal kliknij pozycję **Nowy**.
   
2. Wybierz pozycję **Maszyny wirtualne** z menu portalu Marketplace.
3. Na liście obrazów maszyn wirtualnych wybierz obraz **Windows Server 2012 R2 Datacenter**.
4. W bloku **Podstawy**, w polu **Nazwa** wpisz wartość **VM02**.
5. Wpisz prawidłową nazwę użytkownika i hasło. To konto będzie używane w celu logowania się do maszyny wirtualnej po jej utworzeniu.
6. Podaj wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**, a następnie kliknij przycisk **OK**.
7. W bloku **Rozmiar** wybierz rozmiar maszyny wirtualnej dla tego wystąpienia, a następnie kliknij pozycję **Wybierz**.
8. W bloku Ustawienia możesz zaakceptować wartości domyślne. Upewnij się jedynie, że wybrana sieć wirtualna to **vnet-02**, a podsieć jest ustawiona na **20.0.0.0/24**. Kliknij przycisk **OK**.
9. Sprawdź ustawienia w bloku **Podsumowanie** i kliknij przycisk **OK**.

## <a name="configure-the-nat-vm-in-each-poc-for-gateway-traversal"></a>Konfigurowanie maszyny wirtualnej translatora adresów sieciowych w każdym środowisku weryfikacji koncepcji na potrzeby przechodzenia przez bramy
Ponieważ środowisko weryfikacji koncepcji zostało zaprojektowane jako autonomiczne i odizolowane od sieci, w której jest wdrożony host fizyczny, „zewnętrzna” sieć adresu VIP łącząca się z bramami nie jest w rzeczywistości zewnętrzna. Jest ona ukryta za routerem działającym jako translator adresów sieciowych. Router jest faktycznie maszyną wirtualną z systemem Windows Server (**MAS-BGPNAT01**) o roli Usługi Routing i dostęp zdalny (RRAS, Routing and Remote Access Services) w infrastrukturze weryfikacji koncepcji. Musisz skonfigurować translatora adresów sieciowych na maszynie wirtualnej MAS-BGPNAT01, aby umożliwić nawiązanie połączenia sieci VPN lokacja-lokacja po obu stronach. Aby to zrobić, musisz utworzyć statyczne mapowanie translatora adresów sieciowych mapujące interfejs zewnętrzny na maszynie wirtualnej BGPNAT na adres VIP puli bramy granicznej dla portów wymaganych w celu nawiązania połączenia sieci VPN.

> [!NOTE]
> Ta konfiguracja jest wymagana tylko w przypadku środowisk weryfikacji koncepcji.
> 
> 

### <a name="configure-nat"></a>Konfiguracja translatora adresów sieciowych
Poniższe kroki musisz wykonać w obydwu środowiskach weryfikacji koncepcji.

1. Zaloguj się do maszyny fizycznej usługi Azure Stack używanej dla środowiska POC1.
2. Naciśnij i przytrzymaj klawisze Windows + R, aby otworzyć menu **Uruchom**, wpisz polecenie **mstsc** i naciśnij klawisz **Enter**.
3. W polu **Komputer** wpisz nazwę **MAS-BGPNAT01** i kliknij pozycję **Połącz**.
4. Kliknij menu Start, a następnie kliknij prawym przyciskiem myszy program **Windows PowerShell** i wybierz polecenie **Uruchom jako administrator**.
5. Wpisz polecenie **ipconfig /all**.
6. Znajdź kartę sieciową Ethernet, która jest połączona z siecią lokalną, i zanotuj adres IPv4 powiązany z tą kartą. W środowisku przykładowym ten adres to **10.16.169.131** (poniżej zakreślony na czerwono), ale Twój będzie inny.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image16.png)
7. Wpisz następujące polecenie programu PowerShell, aby wyznaczyć zewnętrzny adres translatora adresów sieciowych dla portów wymaganych przez uwierzytelnianie IKE. Pamiętaj, aby zmienić adres IP na adres odpowiedni dla Twojego środowiska.
   
       Add-NetNatExternalAddress -NatName BGPNAT -IPAddress 10.16.169.131 -PortStart 499 -PortEnd 501
8. Następnie utworzysz statyczne mapowanie translatora adresów sieciowych adresu zewnętrznego na publiczny adres IP bramy w celu mapowania portu 500 ISAKMP na potrzeby fazy 1 tunelu IPSEC.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 500 -InternalPort 500
> [!NOTE] 
> Użyty tutaj parametr `-InternalAddress` to publiczny adres IP utworzonej wcześniej bramy sieci wirtualnej.  Aby znaleźć ten adres IP, przejrzyj właściwości bloku Brama sieci wirtualnej i wyszukaj wartość Publiczny adres IP.       

9. Na koniec musisz skonfigurować przechodzenie translatora adresów sieciowych, które używa portu 4500 w celu pomyślnego ustanowienia kompletnego tunelu IPEC między urządzeniami korzystającymi z translatora adresów sieciowych.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 4500 -InternalPort 4500
> [!NOTE] 
> Użyty tutaj parametr `-InternalAddress` to publiczny adres IP utworzonej wcześniej bramy sieci wirtualnej.  Aby znaleźć ten adres IP, przejrzyj właściwości bloku Brama sieci wirtualnej i wyszukaj wartość Publiczny adres IP.       

10. Powtórz kroki 1–9 w środowisku POC2.

## <a name="test-the-connection"></a>Testowanie połączenia
Po ustanowieniu połączenia lokacja-lokacja należy zweryfikować, czy można przez niego przesyłać ruch. To zadanie jest proste. Obejmuje ono jedynie zalogowanie się do jednej z maszyn wirtualnych utworzonych w jednym ze środowisk weryfikacji koncepcji i wysłanie polecenia ping do maszyny wirtualnej utworzonej w drugim środowisku. W celu upewnienia się, że przesyłasz ruch przez połączenie lokacja-lokacja, koniecznie wyślij polecenie ping na bezpośredni adres IP maszyny wirtualnej w podsieci zdalnej, a nie na adres VIP. Aby to zrobić, należy znaleźć i zanotować adres po drugiej stronie połączenia.

### <a name="log-in-to-the-tenant-vm-in-poc1"></a>Logowanie się do maszyny wirtualnej dzierżawy w środowisku POC1
1. Zaloguj się do komputera fizycznego usługi Azure Stack używanego dla środowiska POC1 i zaloguj się do witryny Portal przy użyciu konta dzierżawy.
2. Kliknij pozycję **Maszyny wirtualne** na lewym pasku nawigacyjnym.
3. Na liście maszyn wirtualnych znajdź utworzoną wcześniej maszynę **VM01** i kliknij ją.
4. W bloku maszyny wirtualnej kliknij pozycję **Połącz**.
   
     ![](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Otwórz wiersz polecenia z poziomu maszyny wirtualnej i wpisz polecenie **ipconfig /all**.
6. Znajdź **adres IPv4** w danych wyjściowych i zanotuj go. Jest to adres, na który wyślesz polecenie ping ze środowiska POC2. W środowisku przykładowym ten adres to **10.0.10.4**, ale w Twoim środowisku może on być inny. Musi on jednak należeć do utworzonej wcześniej podsieci **10.0.10.0/24**.

### <a name="log-in-to-the-tenant-vm-in-poc2"></a>Logowanie się do maszyny wirtualnej dzierżawy w środowisku POC2
1. Zaloguj się do komputera fizycznego usługi Azure Stack używanego dla środowiska POC2 i zaloguj się do portalu przy użyciu konta dzierżawy.
2. Kliknij pozycję **Maszyny wirtualne** na lewym pasku nawigacyjnym.
3. Na liście maszyn wirtualnych znajdź utworzoną wcześniej maszynę **VM02** i kliknij ją.
4. W bloku maszyny wirtualnej kliknij pozycję **Połącz**.
   
5. Otwórz wiersz polecenia z poziomu maszyny wirtualnej i wpisz polecenie **ipconfig /all**.
6. Powinien zostać wyświetlony adres IPv4 należący do zakresu 10.0.20.0/24. W środowisku przykładowym ten adres to 10.0.20.4, ale w Twoim może on być inny.
7. Teraz z maszyny wirtualnej w środowisku POC2 trzeba wysłać polecenie ping przez tunel do maszyny wirtualnej w środowisku POC1. W tym celu należy wysłać polecenie ping na bezpośredni adres IP zarejestrowany na maszynie VM01.
   W środowisku przykładowym jest to 10.0.10.4, ale pamiętaj, aby wysłać polecenie ping na adres IP zanotowany przez siebie. Powinien zostać wyświetlony wynik podobny do następującego:
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
8. Odpowiedź ze zdalnej maszyny wirtualnej wskazuje na pomyślne wykonanie testu! Możesz zamknąć okno Połączenie maszyny wirtualnej. Alternatywnie możesz spróbować wykonać inny transfer danych, np. skopiować plik, aby przetestować połączenie.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Wyświetlanie statystyk transferu danych za pośrednictwem połączenia bramy
Informacje na temat ilości danych przesyłanych przez połączenie lokacja-lokacja są dostępne w bloku Połączenie. Ten test jest również innym dobrym sposobem na sprawdzenie, czy wysłane polecenie ping zostało rzeczywiście przesłane przez połączenie sieci VPN.

1. Nie wylogowując się z maszyny wirtualnej dzierżawy w środowisku POC2, zaloguj się do **portalu weryfikacji koncepcji usługi Microsoft Azure Stack** przy użyciu konta dzierżawy.
2. Kliknij element menu **Przeglądaj**, a następnie wybierz polecenie **Połączenia**.
3. Na liście kliknij połączenie **POC2-POC1**.
4. W bloku Połączenie będą wyświetlane statystyki **Dane przychodzące** i **Dane wychodzące**. Liczby na poniższym zrzucie ekranu są dużo większe, niż generowane przez polecenie ping. Jest to spowodowane dodatkowymi transferami plików. Te statystyki powinny mieć wartości inne niż zero.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)




<!--HONumber=Feb17_HO2-->


