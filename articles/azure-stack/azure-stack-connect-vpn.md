---
title: "Stos Azure połączenia z platformą Azure przy użyciu sieci VPN"
description: "Jak nawiązać sieci wirtualnych Azure stosu sieci wirtualnych na platformie Azure przy użyciu sieci VPN."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: 5d963fe8b1b576768156500af39254f45939f90d
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Stos Azure połączenia z platformą Azure przy użyciu sieci VPN

*Dotyczy: Azure stosu zintegrowane systemy*

W tym artykule przedstawiono sposób tworzenia sieci VPN lokacja lokacja nawiązać sieci wirtualnej Azure stos sieci wirtualnej na platformie Azure.

### <a name="connection-diagram"></a>Diagram połączenia
Na poniższym diagramie przedstawiono, jak powinna wyglądać konfiguracji połączenia po zakończeniu:

![Konfiguracja połączenia sieci VPN typu lokacja lokacja](media/azure-stack-connect-vpn/image2.png)

### <a name="before-you-begin"></a>Przed rozpoczęciem
Aby ukończyć konfigurację połączenia, upewnij się, że masz następujące elementy, przed rozpoczęciem:

* Stos Azure zintegrowane wdrożenia systemów (wielowęzłowego), który jest bezpośrednio połączony z Internetem. Oznacza to, że zakres zewnętrznych publiczny adres IP musi być dostępny bezpośrednio z publicznego Internetu.
* Ważnej subskrypcji platformy Azure.  Jeśli nie masz subskrypcji platformy Azure, możesz utworzyć [bezpłatne konto platformy Azure, w tym miejscu](https://azure.microsoft.com/free/?b=17.06).

## <a name="network-example-values-table"></a>Tabela wartości przykład sieci
Tabela wartości przykład sieci zawiera przykładowe wartości, które są używane w tym artykule. Możesz użyć tych wartości, lub można odwołać się do nich, aby lepiej zrozumieć przykłady w tym artykule.

**Tabela wartości przykład sieci**
|   |Azure Stack|Azure|
|---------|---------|---------|
|Nazwa sieci wirtualnej     |Azs-VNet|AzureVNet |
|Przestrzeń adresową sieci wirtualnej |10.1.0.0/16|10.100.0.0/16|
|Nazwa podsieci     |FrontEnd|FrontEnd|
|Zakres adresów podsieci|10.1.0.0/24 |10.100.0.0/24 |
|Podsieć bramy     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>Tworzenie zasobów sieciowych na platformie Azure

Najpierw należy utworzyć zasobów sieciowych dla platformy Azure. Poniższe instrukcje przedstawiają sposób utworzyć zasobów za pomocą [portalu Azure](http://portal.azure.com/).

### <a name="create-the-virtual-network-and-vm-subnet"></a>Tworzenie sieci wirtualnej i podsieci maszyny wirtualnej

1. Zaloguj się do [portalu Azure](http://portal.azure.com/) przy użyciu konta platformy Azure.
2. W portalu użytkowników wybierz **nowy**.
3. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
4. Wybierz **sieci wirtualnej**.
5. Użyj informacji z tabeli konfiguracji sieci, aby zidentyfikować potrzebne wartości dla platformy Azure **nazwa**, **przestrzeni adresów**, **nazwy podsieci**, i **adres podsieci zakres**.
6. Aby uzyskać **grupy zasobów**, Utwórz nową grupę zasobów lub, jeśli masz już konto, wybierz **Użyj istniejącego**.
7. Wybierz **lokalizacji** Twojej sieci wirtualnej.  Jeśli używasz przykładowe wartości, wybierz **wschodnie stany USA** lub użyj innej lokalizacji, jeśli wolisz.
8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.
9. Wybierz pozycję **Utwórz**.

### <a name="create-the-gateway-subnet"></a>Tworzenie podsieci bramy
1. Otwórz utworzony zasób sieci wirtualnej (**AzureVNet**) z poziomu pulpitu nawigacyjnego.
2. Na **ustawienia** zaznacz **podsieci**.
3. Wybierz **podsieci bramy** Aby dodać podsieci bramy sieci wirtualnej.
4. Domyślna nazwa podsieci to **GatewaySubnet**.
   Podsieci bramy to specjalne podsieci i muszą mieć dokładnie tę nazwę, aby działać prawidłowo.
5. W **zakres adresów** pola, sprawdź adres jest **10.100.1.0/24**.
6. Wybierz **OK** można utworzyć podsieci bramy.

### <a name="create-the-virtual-network-gateway"></a>Tworzenie bramy sieci wirtualnej
1. W portalu Azure wybierz **nowy**.  
2. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
3. Wybierz z listy zasobów sieciowych, **Brama sieci wirtualnej**.
4. W **nazwa**, typ **Azure-GW**.
5. Aby wybrać sieć wirtualną, wybierz **sieci wirtualnej**. Następnie wybierz **AzureVnet** z listy.
6. Wybierz **publicznego adresu IP**. Gdy **wybierz publiczny adres IP** zostanie otwarta sekcja, wybierz opcję **Utwórz nowy**.
7. W **nazwa**, typ **Azure-GW — PiP**, a następnie wybierz **OK**.
8. Domyślnie dla **typ sieci VPN**, **opartej na trasach** jest zaznaczone.
    Zachowaj **opartej na trasach** typ sieci VPN.
9. Upewnij się, że wartości w polach **Subskrypcja** i **Lokalizacja** są poprawne. Można przypiąć do pulpitu nawigacyjnego zasobu. Wybierz pozycję **Utwórz**.

### <a name="create-the-local-network-gateway-resource"></a>Utwórz zasób bramy sieci lokalnej

1. W portalu Azure wybierz **nowy**. 
4. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
5. Wybierz z listy zasobów **bramy sieci lokalnej**.
6. W **nazwa**, typ **Azs-GW**.
7. W **adres IP**, typ publiczny adres IP dla sieci Azure stosu Brama sieci wirtualnej wymieniony w tabeli konfiguracji sieci.
8. W **przestrzeni adresowej**, ze stosu Azure, wpisz **10.1.0.0/24** i **10.1.1.0/24** przestrzeni dla adresów **AzureVNet**.
9. Upewnij się, że Twoje **subskrypcji**, **grupy zasobów**, i **lokalizacji** są poprawne, a następnie wybierz **Utwórz**.

## <a name="create-the-connection"></a>Tworzenie połączenia
1. W portalu użytkowników wybierz **nowy**. 
2. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
3. Wybierz z listy zasobów **połączenia**.
4. Na **podstawowe** w sekcji Ustawienia dla **typ połączenia**, wybierz **lokacja lokacja (IPSec)**.
5. Wybierz **subskrypcji**, **grupy zasobów**, i **lokalizacji**, a następnie wybierz **OK**.
6. Na **ustawienia** zaznacz **Brama sieci wirtualnej**, a następnie wybierz **Azure-GW**.
7. Wybierz **bramy sieci lokalnej**, a następnie wybierz **Azs-GW**.
8. W **nazwa połączenia**, typ **Azure Azs**.
9. W **klucz udostępniony (PSK)**, typ **12345**. Jeśli wybierzesz inną wartość, należy pamiętać, że *musi* pasuje do wartości dla klucza udostępnionego utworzonego na drugim końcu połączenia. Kliknij przycisk **OK**.
10. Przegląd **Podsumowanie** , a następnie wybierz **OK**.

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej
Teraz Utwórz maszynę wirtualną na platformie Azure i umieszcza je w podsieci sieci maszyny Wirtualnej w Twojej sieci wirtualnej.

1. W portalu Azure wybierz **nowy**.
2. Przejdź do **Marketplace**, a następnie wybierz **obliczeniowe**.
3. Na liście obrazów maszyny wirtualnej, wybierz **Eval centrum danych systemu Windows Server 2016** obrazu.
4. Na **podstawy** sekcji dla **nazwa**, typ **AzureVM**.
5. Wpisz prawidłową nazwę użytkownika i hasło. Służy do logowania się na maszynie wirtualnej, po jego utworzeniu.
6. Podaj **subskrypcji**, **grupy zasobów**, i **lokalizacji**, a następnie wybierz **OK**.
7. Na **rozmiar** wybierz rozmiar maszyny wirtualnej dla tego wystąpienia, a następnie wybierz **wybierz**.
8. Na **ustawienia** sekcji można zaakceptować wartości domyślne. Upewnij się, że **AzureVnet** sieci wirtualnej jest zaznaczone, a następnie sprawdź, czy podsieci ma ustawioną **10.100.0.0/24**. Kliknij przycisk **OK**.
9. Sprawdź ustawienia na **Podsumowanie** , a następnie wybierz **OK**.

## <a name="create-the-network-resources-in-azure-stack"></a>Tworzenie zasobów sieciowych w stosie Azure
Następnie należy utworzyć zasobów sieciowych w stosie Azure.

### <a name="sign-in-as-a-user"></a>Zaloguj się jako użytkownik
Administrator usługi można zalogować się jako użytkownik do testowania planów, ofertami i subskrypcje, którzy mogą korzystać z własnych użytkowników. Jeśli nie masz już konto, [Utwórz konto użytkownika](azure-stack-add-new-user-aad.md) przed zalogowaniem.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Tworzenie sieci wirtualnej i podsieci maszyny wirtualnej
1. Użyj konta użytkownika do logowania do portalu użytkowników.
2. W portalu użytkowników wybierz **nowy**.

    ![Utwórz nową sieć wirtualną](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
4. Wybierz **sieci wirtualnej**.
5. Dla **nazwa**, **przestrzeni adresów**, **nazwy podsieci**, i **zakres adresów podsieci**, użyj wartości z tabeli konfiguracji sieci.
6. W **subskrypcji**, pojawi się subskrypcji, który został utworzony wcześniej.
7. Aby uzyskać **grupy zasobów**, można utworzyć grupę zasobów lub jeśli już istnieje, wybierz **Użyj istniejącego**.
8. Sprawdź lokalizację domyślną.
9. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.
10. Wybierz pozycję **Utwórz**.

### <a name="create-the-gateway-subnet"></a>Tworzenie podsieci bramy
1. Na pulpicie nawigacyjnym otwórz zasobów sieci wirtualnej sieci wirtualnej Azs utworzony.
2. Na **ustawienia** zaznacz **podsieci**.
3. Aby dodać podsieci bramy sieci wirtualnej, wybierz **podsieci bramy**.
   
    ![Dodaj podsieć bramy](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Domyślnie nazwa podsieci ma ustawioną **GatewaySubnet**.
   Specjalne są podsieciami bramy. Aby działać poprawnie, musisz użyć *GatewaySubnet* nazwy.
5. W **zakres adresów**, sprawdź, czy adres jest **10.1.1.0/24**.
6. Wybierz **OK** można utworzyć podsieci bramy.

### <a name="create-the-virtual-network-gateway"></a>Tworzenie bramy sieci wirtualnej
1. W portalu Azure stosu wybierz **nowy**. 
2. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
3. Wybierz z listy zasobów sieciowych, **Brama sieci wirtualnej**.
4. W **nazwa**, typ **Azs-GW**.
5. Wybierz **sieci wirtualnej** element, aby wybrać sieci wirtualnej.
   Wybierz **Azs-VNet** z listy.
6. Wybierz **publicznego adresu IP** elementu menu. Gdy **wybierz publiczny adres IP** zostanie otwarta sekcja, wybierz opcję **Utwórz nowy**.
7. W **nazwa**, typ **Azs-GW — PiP**, a następnie wybierz **OK**.
8.  Domyślnie dla **typ sieci VPN**, **opartej na trasach** jest zaznaczone.
    Zachowaj **opartej na trasach** typ sieci VPN.
9. Upewnij się, że wartości w polach **Subskrypcja** i **Lokalizacja** są poprawne. Można przypiąć do pulpitu nawigacyjnego zasobu. Wybierz pozycję **Utwórz**.

### <a name="create-the-local-network-gateway"></a>Tworzenie bramy sieci lokalnej
Pojęcia *bramy sieci lokalnej* w stosie Azure jest nieco inne niż w przypadku wdrożenia usługi Azure.

We wdrożeniu Azure bramy sieci lokalnej reprezentuje urządzenie fizyczne lokalnego (w lokalizacji użytkownika), używanej do nawiązania połączenia bramy sieci wirtualnej na platformie Azure. W stosie Azure obu końców połączenia są bram sieci wirtualnej!

Sposób można traktować to bardziej ogólnie jest zasobu bramy sieci lokalnej będzie zawsze informuje bramy zdalnego na drugim końcu połączenia. 

### <a name="create-the-local-network-gateway-resource"></a>Utwórz zasób bramy sieci lokalnej
1. Zaloguj się do portalu Azure stosu.
2. W portalu użytkowników wybierz **nowy**.
3. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
4. Wybierz z listy zasobów **bramy sieci lokalnej**.
5. W **nazwa**, typ **Azure-GW**.
6. W **adres IP**, wpisz publiczny adres IP dla bramy sieci wirtualnej na platformie Azure **Azure-GW — PiP**. Ten adres występuje wcześniej w tabeli konfiguracji sieci.
7. W **przestrzeni adresowej**, do przestrzeni adresowej sieci wirtualnej Azure, który został utworzony, wpisz **10.100.0.0/24** i **10.100.1.0/24**.
8. Upewnij się, że Twoje **subskrypcji**, **grupy zasobów**, i **lokalizacji** są poprawne, a następnie wybierz **Utwórz**.

### <a name="create-the-connection"></a>Tworzenie połączenia
1. W portalu użytkowników wybierz **nowy**.
2. Przejdź do **Marketplace**, a następnie wybierz **sieci**.
3. Wybierz z listy zasobów **połączenia**.
4. Na **podstawy** w sekcji Ustawienia dla **typ połączenia**, wybierz pozycję **lokacja lokacja (IPSec)**.
5. Wybierz **subskrypcji**, **grupy zasobów**, i **lokalizacji**, a następnie wybierz **OK**.
6. Na **ustawienia** zaznacz **Brama sieci wirtualnej**, a następnie wybierz **Azs-GW**.
7. Wybierz **bramy sieci lokalnej**, a następnie wybierz **Azure-GW**.
8. W **nazwa połączenia**, typ **Azs Azure**.
9. W **klucz udostępniony (PSK)**, typ **12345**, a następnie wybierz **OK**.
10. Na **Podsumowanie** zaznacz **OK**.

### <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej
Sprawdzania poprawności danych, który przechodzi przez połączenie sieci VPN, należy utworzyć maszyny wirtualnej na obu końcach do wysyłania i odbierania danych za pośrednictwem tunelu VPN. 

1. W portalu Azure wybierz **nowy**.
2. Przejdź do **Marketplace**, a następnie wybierz **obliczeniowe**.
3. Na liście obrazów maszyny wirtualnej, wybierz **Eval centrum danych systemu Windows Server 2016** obrazu.
4. Na **podstawy** sekcji w **nazwa**, typ **Azs-VM**.
5. Wpisz prawidłową nazwę użytkownika i hasło. Służy do logowania się do maszyny Wirtualnej, po jego utworzeniu.
6. Podaj **subskrypcji**, **grupy zasobów**, i **lokalizacji**, a następnie wybierz **OK**.
7. Na **rozmiar** , dla tego wystąpienia, wybierz rozmiar maszyny wirtualnej, a następnie wybierz **wybierz**.
8. Na **ustawienia** pozycję Zaakceptuj ustawienia domyślne. Upewnij się, że **Azs-VNet** sieci wirtualnej jest zaznaczone. Sprawdź, czy podsieci ma ustawioną **10.1.0.0/24**. Następnie wybierz **OK**.
9. Na **Podsumowanie** , przejrzyj ustawienia, a następnie wybierz **OK**.


## <a name="test-the-connection"></a>Testowanie połączenia
Teraz, gdy zostanie nawiązane połączenie lokacja lokacja, należy sprawdzić, czy uzyskanie przez niego przepływające ruchu. Aby sprawdzić, zaloguj się do jednej z maszyn wirtualnych, które zostały utworzone w stosie Azure. Następnie należy wysyłać polecenia ping maszynę wirtualną, która zostanie utworzona na platformie Azure. 

Aby upewnić się, że wysyłać ruch przez połączenie lokacja lokacja, zbadaj adres bezpośredni adres IP (DIP) maszyny wirtualnej w zdalnym podsieci, a nie adres VIP. Aby to zrobić, należy znaleźć adres DIP na drugim końcu połączenia. Zapisz adres do późniejszego użycia.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Zaloguj się do maszyny Wirtualnej Azure stosu użytkownika
1. Zaloguj się do portalu Azure stosu.
2. Na pasku nawigacyjnym po lewej stronie wybierz **maszyn wirtualnych**.
3. Na liście maszyn wirtualnych, należy znaleźć **Azs-VM** utworzonego wcześniej, i wybierz go.
4. W sekcji dla maszyny wirtualnej, kliknij polecenie **Connect**, a następnie otwórz plik Azs VM.rdp.
   
     ![Przycisk Połącz](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Zaloguj się przy użyciu konta, które zostało skonfigurowane podczas tworzenia maszyny wirtualnej.
6. Otwórz podwyższonym **programu Windows PowerShell** okna.
7. Wpisz polecenie **ipconfig /all**.
8. W danych wyjściowych, Znajdź **adres IPv4**, a następnie Zapisz adres do późniejszego użycia. Jest to adres, który będzie ping z platformy Azure. W środowisku przykładzie adres jest **10.1.0.4**, ale w danym środowisku mogą się różnić. Należy włączyć w **10.1.0.0/24** podsieci utworzonego wcześniej.
9. Aby utworzyć regułę zapory zezwalającą maszyny wirtualnej, które odpowiadają na polecenia ping, uruchom następujące polecenie programu PowerShell:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Zaloguj się do dzierżawy maszyny Wirtualnej na platformie Azure
1. Zaloguj się do Portalu Azure.
2. Na pasku nawigacyjnym po lewej stronie kliknij **maszyn wirtualnych**.
3. Z listy maszyn wirtualnych, należy znaleźć **maszyny Wirtualnej Azure** utworzonego wcześniej, i wybierz go.
4. W sekcji dla maszyny wirtualnej, kliknij polecenie **Connect**.
5. Zaloguj się przy użyciu konta, które zostało skonfigurowane podczas tworzenia maszyny wirtualnej.
6. Otwórz podwyższonym **programu Windows PowerShell** okna.
7. Wpisz polecenie **ipconfig /all**.
8. Powinien zostać wyświetlony adres IPv4, który mieści się w **10.100.0.0/24**. W środowisku przykładzie adres jest **10.100.0.4**, ale adres mogą się różnić.
9. Aby utworzyć regułę zapory zezwalającą maszyny wirtualnej, które odpowiadają na polecenia ping, uruchom następujące polecenie programu PowerShell:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Z maszyny wirtualnej na platformie Azure Zbadaj maszyny wirtualnej w stosie Azure za pośrednictwem tunelu. Aby to zrobić, możesz wykonać polecenie ping zapisane z maszyny Wirtualnej Azs DIP.
   W środowisku przykładzie jest to **10.1.0.4**, ale należy wysyłać polecenia ping adres zanotowaną w laboratorium. Powinny pojawić się wynik, który wygląda jak poniższy zrzut ekranu:
   
    ![Pomyślne polecenie ping](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Odpowiedź od zdalnego maszyny wirtualnej wskazuje testów powiodło się! Możesz zamknąć okno maszyny wirtualnej. Aby przetestować połączenie, możesz wypróbować inne rodzaje transferów danych, takich jak kopiowania plików.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Wyświetlanie statystyk transferu danych za pośrednictwem połączenia bramy
Jeśli chcesz wiedzieć, ile dane są przesyłane za pośrednictwem połączenia lokacja lokacja, te informacje są dostępne na **połączenia** sekcji. Ten test jest również inny sposób, aby sprawdzić, czy polecenie ping, które zostały wysłane faktycznie nawiązaniem połączenia z siecią VPN.

1. Po zarejestrowaniu maszynie wirtualnej użytkownika w stosie Azure, należy używać konta użytkownika logować się do portalu użytkowników.
2. Przejdź do **wszystkie zasoby**, a następnie wybierz **Azs Azure** połączenia. **Połączenia** pojawi się.
4. Na **połączenia** sekcji Statystyki dotyczące **danych w** i **dla danych wychodzących** są wyświetlane. Na poniższym zrzucie ekranu dużą liczbą są przypisane do transferu dodatkowych plików. Niektóre niezerowe wartości powinny być widoczne.
   
    ![Dane wejściowe i wyjściowe](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Kolejne kroki

[Wdrażanie aplikacji na platformie Azure oraz Azure stosu](azure-stack-solution-pipeline.md)
