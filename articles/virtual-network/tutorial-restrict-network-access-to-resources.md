---
title: Ograniczenie dostępu do sieci do zasobów PaaS - portalu Azure | Dokumentacja firmy Microsoft
description: Dowiedz się, jak ograniczyć i ograniczenie dostępu do sieci do zasobów platformy Azure, takich jak usługi Azure Storage i bazy danych SQL Azure z punktów końcowych usługi sieci wirtualnej przy użyciu portalu Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 9a64a5c1f63dc05cba6fdfa310b694e34bdba7d1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Ograniczenie dostępu do sieci do PaaS zasobów z punktów końcowych usługi sieci wirtualnej przy użyciu portalu Azure

Punktów końcowych usługi sieci wirtualnej umożliwiają ograniczenie dostępu do sieci do niektórych zasobów usługi Azure z podsiecią sieci wirtualnej. Można również usunąć dostęp do zasobów Internetu. Punkty końcowe usługi zapewniają bezpośredniego połączenia z Twojej sieci wirtualnej do obsługiwanych usług platformy Azure, co umożliwia wykorzystanie wirtualnej sieci prywatnej przestrzeni adresowej do uzyskiwania dostępu do usług Azure. Ruch kierowany do zasobów platformy Azure za pomocą punktów końcowych usługi zawsze pozostaje w sieci Microsoft Azure w sieci szkieletowej. W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz sieć wirtualną z jedną podsiecią
> * Dodaj podsieć i włączyć punkt końcowy usługi
> * Tworzenie zasobów platformy Azure i zezwolić na dostęp do sieci z go z tylko podsieci
> * Wdróż maszynę wirtualną (VM) do każdej podsieci
> * Potwierdź dostęp do zasobu z podsieci
> * Upewnij się, że odmowa dostępu do zasobu z podsieci i z Internetu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do portalu Azure pod adresem http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. Wprowadź, lub wybierz następujące informacje, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |----|----|
    |Name (Nazwa)| myVirtualNetwork |
    |Przestrzeń adresowa| 10.0.0.0/16|
    |Subskrypcja| Wybierz subskrypcję|
    |Grupa zasobów | Wybierz **Utwórz nowy** , a następnie wprowadź *myResourceGroup*.|
    |Lokalizacja| Wybierz **wschodnie stany USA** |
    |Nazwa podsieci| Publiczne|
    |Zakres adresów podsieci| 10.0.0.0/24|
    |Punkty końcowe usługi| Disabled (Wyłączony)|

    ![Podaj podstawowe informacje o sieci wirtualnej](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)


## <a name="enable-a-service-endpoint"></a>Włącz punkt końcowy usługi

1. W **wyszukiwania zasobów, usługi i dokumenty** polu w górnej części portalu, wprowadź *myVirtualNetwork.* Gdy **myVirtualNetwork** pojawia się w wynikach wyszukiwania, wybierz go.
2. Dodaj podsieć do sieci wirtualnej. W obszarze **ustawienia**, wybierz pozycję **podsieci**, a następnie wybierz **+ podsieci**, jak pokazano na poniższej ilustracji:

    ![Dodawanie podsieci](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. W obszarze **Dodaj podsieć**, wybierz lub wprowadź następujące informacje, a następnie wybierz **OK**:

    |Ustawienie|Wartość|
    |----|----|
    |Name (Nazwa)| Prywatne |
    |Zakres adresów| 10.0.1.0/24|
    |Punkty końcowe usługi| Wybierz **Microsoft.Storage** w obszarze **usług**|

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Ograniczenie dostępu do sieci do i z podsiecią

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieciowej grupy zabezpieczeń**.
W obszarze **Utwórz grupę zabezpieczeń sieci**, wprowadź, lub wybierz następujące informacje, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |----|----|
    |Name (Nazwa)| myNsgPrivate |
    |Subskrypcja| Wybierz subskrypcję|
    |Grupa zasobów | Wybierz **Użyj istniejącego** i wybierz *myResourceGroup*.|
    |Lokalizacja| Wybierz **wschodnie stany USA** |

4. Po utworzeniu grupy zabezpieczeń sieci, wprowadź *myNsgPrivate*w **wyszukiwania zasobów, usługi i dokumenty** pole w górnej części portalu. Gdy **myNsgPrivate** pojawia się w wynikach wyszukiwania, wybierz go.
5. W obszarze **ustawienia**, wybierz pozycję **reguł zabezpieczeń dla ruchu wychodzącego**.
6. Wybierz **+ Dodaj**.
7. Utworzyć regułę, która umożliwia dostęp ruchu wychodzącego na publiczne adresy IP przypisane do usługi Magazyn Azure. Wprowadź, lub wybierz następujące informacje, a następnie wybierz **OK**:

    |Ustawienie|Wartość|
    |----|----|
    |Element źródłowy| Wybierz **sieci wirtualnej** |
    |Zakresy portów źródła| * |
    |Element docelowy | Wybierz **usługi Tag**|
    |Numer seryjny docelowego | Wybierz **magazynu**|
    |Zakresy portów docelowego| * |
    |Protokół|Dowolne|
    |Akcja|Zezwalaj|
    |Priorytet|100|
    |Name (Nazwa)|Allow-Storage-All|
8. Utworzyć regułę, która zastępuje domyślną regułę zabezpieczeń, który umożliwia wychodzący dostęp do wszystkich publicznych adresów IP. Wykonaj kroki 6 i 7 ponownie, używając następujących wartości:

    |Ustawienie|Wartość|
    |----|----|
    |Element źródłowy| Wybierz **sieci wirtualnej** |
    |Zakresy portów źródła| * |
    |Element docelowy | Wybierz **usługi Tag**|
    |Numer seryjny docelowego| Wybierz **Internet**|
    |Zakresy portów docelowego| * |
    |Protokół|Dowolne|
    |Akcja|Zablokuj|
    |Priorytet|110|
    |Name (Nazwa)|Deny-Internet-All|

9. W obszarze **ustawienia**, wybierz pozycję **reguły zabezpieczeń dla ruchu przychodzącego**.
10. Wybierz **+ Dodaj**.
11. Utworzyć regułę, która zezwala na ruch protokołu RDP (Remote Desktop) dla ruchu przychodzącego podsieci z dowolnego miejsca. Reguła zastępuje domyślną regułę zabezpieczeń, która nie zezwala na cały ruch przychodzący z Internetu. Połączenia pulpitu zdalnego są dozwolone do podsieci, aby później można przetestować połączenia. Wykonaj kroki 6 i 7 ponownie, używając następujących wartości:

    |Ustawienie|Wartość|
    |----|----|
    |Element źródłowy| Dowolne |
    |Zakresy portów źródła| * |
    |Element docelowy | Wybierz **usługi Tag**|
    |Numer seryjny docelowego| Wybierz **sieci wirtualnej**|
    |Zakresy portów docelowego| 3389 |
    |Protokół|Dowolne|
    |Akcja|Zezwalaj|
    |Priorytet|120|
    |Name (Nazwa)|Zezwalaj na RDP-wszystkie|

12. W obszarze **ustawienia**, wybierz pozycję **podsieci**.
13. Wybierz **+ Skojarz**
14. W obszarze **skojarzyć podsieci**, wybierz pozycję **sieci wirtualnej** , a następnie wybierz **myVirtualNetwork** w obszarze **wybierz sieć wirtualną**.
15. W obszarze **Wybierz podsieć**, wybierz pozycję **prywatnej**, a następnie wybierz **OK**.

## <a name="restrict-network-access-to-a-resource"></a>Ograniczenie dostępu do sieci do zasobu

Kroki niezbędne do ograniczania dostępu do sieci do zasobów został utworzony za pomocą usług Azure włączone dla punktów końcowych usługi różni się w usługach. W dokumentacji dla poszczególnych usług, aby poznać konkretne kroki dla każdej usługi. W dalszej części tego artykułu zawiera kroki, aby ograniczyć dostęp do konta usługi Azure Storage, na przykład.

### <a name="create-a-storage-account"></a>Tworzenie konta magazynu

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **magazynu**, a następnie wybierz **konta magazynu — obiekt blob, plików, tabeli, kolejki**.
3. Wprowadź, lub wybierz poniższe informacje, Zaakceptuj pozostałe wartości domyślne, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |----|----|
    |Name (Nazwa)| Wprowadź nazwę, która jest unikatowa dla wszystkich lokalizacji platformy Azure, w zakresie od 3 do 24 znaków długości, przy użyciu tylko cyfry i małe litery.|
    |Rodzaj konta|StorageV2 (ogólnego przeznaczenia v2)|
    |Replikacja| Magazyn lokalnie nadmiarowy (LRS)|
    |Subskrypcja| Wybierz subskrypcję|
    |Grupa zasobów | Wybierz **Użyj istniejącego** i wybierz *myResourceGroup*.|
    |Lokalizacja| Wybierz **wschodnie stany USA** |

### <a name="create-a-file-share-in-the-storage-account"></a>Tworzenie udziału plików w ramach konta magazynu

1. Po utworzeniu konta magazynu, wprowadź nazwę konta magazynu w **wyszukiwania zasobów, usługi i dokumenty** pole w górnej części portalu. Gdy nazwa konta magazynu jest wyświetlana w wynikach wyszukiwania, zaznacz go.
2. Wybierz **pliki**, jak pokazano na poniższej ilustracji:

    ![Konto magazynu](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 
3. Wybierz **+ udział pliku**w obszarze **usługa plików**.
4. Wprowadź *Moje udziału plików* w obszarze **nazwa**, a następnie wybierz **OK**.
5. Zamknij **usługa plików** pole.

### <a name="enable-network-access-from-a-subnet"></a>Włączanie dostępu do sieci z podsiecią

Domyślnie kont magazynu akceptować połączenia od klientów w dowolnej sieci. Aby zezwolić na dostęp z określonej podsieci i odmawiania dostępu do sieci z innymi sieciami, wykonaj następujące czynności:

1. W obszarze **ustawienia** dla konta magazynu, wybierz **zapory i sieci wirtualne**.
2. W obszarze **sieci wirtualnych**, wybierz pozycję **wybrane sieci**.
3. Wybierz **Dodaj istniejącej sieci wirtualnej**.
4. W obszarze **dodawanie sieci**, wybierz następujące wartości, a następnie wybierz **Dodaj**:

    |Ustawienie|Wartość|
    |----|----|
    |Subskrypcja| Wybierz subskrypcję.|
    |Sieci wirtualne|Wybierz **myVirtualNetwork**w obszarze **sieci wirtualnej**|
    |Podsieci| Wybierz **prywatnej**w obszarze **podsieci**|

    ![Zapory i sieci wirtualnych](./media/tutorial-restrict-network-access-to-resources/storage-firewalls-and-virtual-networks.png) 

5. Wybierz pozycję **Zapisz**.
6. Zamknij **zapory i sieci wirtualne** pole.
7. W obszarze **ustawienia** dla konta magazynu, wybierz **klucze dostępu**, jak pokazano na poniższej ilustracji:

      ![Zapory i sieci wirtualnych](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

8. Uwaga **klucza** wartość, jak należy ręcznie wprowadzić go w kolejnym kroku podczas mapowania udziału pliku z literą dysku na maszynie wirtualnej.

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Aby przetestować dostępu do sieci na konto magazynu, należy wdrożyć maszynę Wirtualną do każdej podsieci.

### <a name="create-the-first-virtual-machine"></a>Tworzenie pierwszej maszyny wirtualnej

1. Wybierz **+ Utwórz zasób** na górnego, lewego rogu portalu Azure.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**.
3. Wprowadź, lub wybierz następujące informacje, a następnie wybierz **OK**:

    |Ustawienie|Wartość|
    |----|----|
    |Name (Nazwa)| myVmPublic|
    |Nazwa użytkownika|Wprowadź nazwę użytkownika wybrane.|
    |Hasło| Wprowadź hasło wybrane. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subskrypcja| Wybierz subskrypcję.|
    |Grupa zasobów| Wybierz **Użyj istniejącego** i wybierz **myResourceGroup**.|
    |Lokalizacja| Wybierz **wschodnie stany USA**.|

    ![Podaj podstawowe informacje o maszynie wirtualnej](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)
4. Wybierz rozmiar maszyny wirtualnej, a następnie wybierz **wybierz**.
5. W obszarze **ustawienia**, wybierz pozycję **sieci** , a następnie wybierz **myVirtualNetwork**. Następnie wybierz **podsieci**i wybierz **publicznych**, jak pokazano na poniższej ilustracji:

    ![Wybierz sieć wirtualną](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)
6. Na **Podsumowanie** wybierz pozycję **Utwórz** rozpocząć wdrażanie maszyny wirtualnej. Maszyna wirtualna zajmuje kilka minut do wdrożenia, ale można kontynuować do następnego kroku, podczas tworzenia maszyny Wirtualnej.

### <a name="create-the-second-virtual-machine"></a>Tworzenie drugiej maszyny wirtualnej

Wykonaj kroki 1 – 6 ponownie, ale w kroku 3, Nazwa maszyny wirtualnej *myVmPrivate* i wybierz w kroku 5, **prywatnej** podsieci.

Maszyna wirtualna ma kilka minut, aby wdrożyć. Nie należy kontynuować do następnego kroku, dopiero po zakończeniu tworzenia i otwórz jej ustawienia w portalu.

## <a name="confirm-access-to-storage-account"></a>Potwierdź dostęp do konta magazynu

1. Raz *myVmPrivate* wirtualna zakończy tworzenie, Azure otwiera ustawienia dla niego. Połącz się z maszyną Wirtualną, wybierając **Connect** przycisku, jak pokazano na poniższej ilustracji:

    ![Połącz z maszyną wirtualną](./media/tutorial-restrict-network-access-to-resources/connect-to-virtual-machine.png)

2. Po wybraniu **Connect** przycisku pliku Remote Desktop Protocol (RDP) jest tworzony i pobrana na komputer.  
3. Otwórz plik rdp pobranego. Po wyświetleniu monitu wybierz **Connect**. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny Wirtualnej. Musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić poświadczenia zostały wprowadzone podczas tworzenia maszyny Wirtualnej. 
4. Kliknij przycisk **OK**.
5. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Jeśli zostanie wyświetlone ostrzeżenie, wybierz **tak** lub **Kontynuuj**, aby nawiązać połączenie.
6. Na *myVmPrivate* maszyny Wirtualnej, mapowanie dysku przy użyciu programu PowerShell Z udziału plików na platformę Azure. Przed uruchomieniem polecenia, które należy wykonać, Zastąp `<storage-account-key>` i `<storage-account-name>` z wartościami dostarczone i pobierane w [Utwórz konto magazynu](#create-a-storage-account).

    ```powershell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
    ```
    
    PowerShell zwraca dane wyjściowe podobne do następujących przykładowe dane wyjściowe:

    ```powershell
    Name           Used (GB)     Free (GB) Provider      Root
    ----           ---------     --------- --------      ----
    Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
    ```

    Pomyślnie zamapowany na dysk Z udziału plików na platformę Azure.

7. Upewnij się, że maszyna wirtualna nie ma wychodzącego łączności innych publicznych adresów IP z wiersza polecenia:

    ```
    ping bing.com
    ```
    
    Nie odpowiedzi jest wyświetlany, ponieważ grupa zabezpieczeń sieci skojarzonych z *prywatnej* podsieci nie zezwala na dostęp ruchu wychodzącego do publicznych adresów IP innego niż adres przypisany do usługi Magazyn Azure.

8. Zamykanie sesji usług pulpitu zdalnego do *myVmPrivate* maszyny Wirtualnej.

## <a name="confirm-access-is-denied-to-storage-account"></a>Upewnij się, że odmowa dostępu do konta magazynu

1. Wprowadź *myVmPublic* w **wyszukiwania zasobów, usługi i dokumenty** pole w górnej części portalu.
2. Gdy **myVmPublic** pojawia się w wynikach wyszukiwania, wybierz go.
3. Wykonać kroki od 1 do 6 w [Potwierdź dostęp do konta magazynu](#confirm-access-to-storage-account) dla *myVmPublic* maszyny Wirtualnej.

    Odmowa dostępu i pojawi się `New-PSDrive : Access is denied` błędu. Odmowa dostępu, ponieważ *myVmPublic* maszyna wirtualna jest wdrożona w *publicznego* podsieci. *Publicznego* podsieci nie ma punktu końcowego usługi włączone dla usługi Azure Storage i konta magazynu tylko zezwala na dostęp do sieci z *prywatnej* podsieci, nie *publicznego*podsieci.

4. Zamykanie sesji usług pulpitu zdalnego do *myVmPublic* maszyny Wirtualnej.

5. Z komputera, przejdź do platformy Azure [portal](https://portal.azure.com).
6. Wprowadź nazwę konta magazynu utworzone w **wyszukiwania zasobów, usługi i dokumenty** pole. Gdy nazwa konta magazynu jest wyświetlana w wynikach wyszukiwania, zaznacz go.
7. Wybierz pozycję **Pliki**.
8. Zostanie wyświetlony błąd pokazano na poniższej ilustracji:

    ![Błąd odmowy dostępu](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)

    Odmowa dostępu, ponieważ komputer nie znajduje się w *prywatnej* podsieć *MyVirtualNetwork* sieci wirtualnej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupy zasobów i wszystkie zasoby, które zawiera:

1. Wprowadź *myResourceGroup* w **wyszukiwania** pole w górnej części portalu. Po wyświetleniu **myResourceGroup** w wynikach wyszukiwania wybierz go.
2. Wybierz pozycję **Usuń grupę zasobów**.
3. Wprowadź *myResourceGroup* dla **typu nazwa grupy zasobów:** i wybierz **usunąć**.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku możesz włączyć punkt końcowy usługi dla podsieci sieci wirtualnej. Wiesz, że punkty końcowe usługi można włączyć dla zasobów z wielu usług Azure. Utworzono konto magazynu Azure i ograniczania dostępu do sieci do konta magazynu, aby tylko zasoby w podsieci sieci wirtualnej. Przed utworzeniem punktów końcowych usług w środowisku produkcyjnym sieci wirtualnych, zalecane jest, że należy dokładnie zapoznać się z [punkty końcowe usługi](virtual-network-service-endpoints-overview.md).

Jeśli masz wiele sieci wirtualnych w ramach Twojego konta, możesz połączyć ze sobą dwie sieci wirtualne, więc zasoby w każdej sieci wirtualnej mogą komunikować się ze sobą. Przejdź do następnego samouczkiem, aby dowiedzieć się, jak połączyć sieci wirtualnych.

> [!div class="nextstepaction"]
> [Łączenie sieci wirtualnej](./tutorial-connect-virtual-networks-portal.md)
