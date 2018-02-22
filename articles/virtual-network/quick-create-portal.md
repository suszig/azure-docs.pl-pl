---
title: Tworzenie sieci wirtualnej na platformie Azure - Portal | Dokumentacja firmy Microsoft
description: "Dowiedz się szybko utworzyć sieć wirtualną przy użyciu portalu Azure. Sieć wirtualna umożliwia wiele typów zasobów platformy Azure do prywatnie komunikują się ze sobą."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 61100b9786245204502686a47e5aae2a6d210259
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Tworzenie sieci wirtualnej przy użyciu witryny Azure Portal

W tym artykule należy Dowiedz się, jak utworzyć sieć wirtualną. Po utworzeniu sieci wirtualnej, wdrożenie dwóch maszyn wirtualnych w sieci wirtualnej i prywatnie komunikacji między nimi.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

1. Kliknij przycisk **+ nowy** w lewym górnym rogu portalu Azure.

2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.

3. Jak pokazano na poniższej ilustracji, wprowadź *myVirtualNetwork* dla **nazwa**, *myResourceGroup* dla **grupy zasobów**, wybierz  **Lokalizacja** i **subskrypcji**Zaakceptuj pozostałe wartości domyślne, a następnie kliknij przycisk **Utwórz**. 

    ![Podaj podstawowe informacje o sieci wirtualnej](./media/quick-create-portal/virtual-network.png)

    **Przestrzeni adresów** jest określone w notacji CIDR. Sieć wirtualna zawiera zero lub więcej podsieci. Podsieci domyślnej **zakres adresów** 10.0.0.0/24 używa cały zakres adresów sieci wirtualnej, więc nie można utworzyć innej podsieci w sieci wirtualnej przy użyciu domyślnej przestrzeni adresowej i zakresu. Określony zakres adresów zawiera 10.0.0.0-10.0.0.254 adresów IP. Tylko 10.0.0.4-10.0.0.254 są dostępne, ponieważ Azure rezerwuje pierwsze cztery adresów (0-3) oraz ostatni adres w każdej podsieci. Dostępne adresy IP są przypisywane z zasobami wdrożonymi w sieci wirtualnej.

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Sieć wirtualna umożliwia kilka typów zasobów platformy Azure do prywatnie komunikują się ze sobą. Jeden typ zasobów, które można wdrożyć w sieci wirtualnej jest maszyną wirtualną. Utwórz dwie maszyny wirtualne w sieci wirtualnej, aby można było zweryfikować i zrozumieć sposób działania komunikacji między maszynami wirtualnymi w sieci wirtualnej w kolejnym kroku.

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**.

3. Wprowadź informacje o maszynie wirtualnej pokazany na rysunku poniżej. **Nazwy użytkownika** i **hasło** wprowadzeniu są używane do logowania do maszyny wirtualnej w kolejnym kroku. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). Wybierz użytkownika **subskrypcji**, wybierz opcję wykorzystania istniejącego *myResourceGroup* zasobów, a następnie upewnij się, że **lokalizacji** jest tej samej lokalizacji został utworzony sieć wirtualną w. Po zakończeniu kliknij przycisk **OK**.

    ![Podaj podstawowe informacje o maszynie wirtualnej](./media/quick-create-portal/virtual-machine-basics.png)

4. Wybierz rozmiar maszyny wirtualnej, a następnie kliknij przycisk **wybierz**. Aby wyświetlić więcej rozmiarów, wybierz pozycje **Wyświetl wszystkie** lub zmień filtr **Obsługiwany typ dysku**. Rozmiary, które pojawiają się automatycznie mogą być inne niż w poniższym przykładzie: 

    ![Wybierz rozmiar maszyny wirtualnej](./media/quick-create-portal/virtual-machine-size.png)

5. W obszarze **ustawienia**, *myVirtualNetwork* powinna już być zaznaczona dla **sieci wirtualnej**, ale jeśli nie, kliknij przycisk **sieci wirtualnej**, następnie wybierz *myVirtualNetwork*. Pozostaw *domyślne* wybranego dla **podsieci**, a następnie kliknij przycisk **OK**.

    ![Wybierz sieć wirtualną](./media/quick-create-portal/virtual-machine-network-settings.png)

6. Na **Podsumowanie** kliknij przycisk **Utwórz** rozpocząć wdrażanie maszyny wirtualnej. 

7. Maszyna wirtualna ma kilka minut na utworzenie. Po utworzeniu maszyna wirtualna jest przypięta do pulpitu nawigacyjnego portalu Azure i automatycznie otwiera podsumowanie maszyny wirtualnej. Kliknij przycisk **sieci**.

    ![Informacje o sieci maszyny wirtualnej](./media/quick-create-portal/virtual-machine-networking.png)

    Zostanie wyświetlony **prywatnego adresu IP** adres jest *10.0.0.4*. W kroku 5, w obszarze **ustawienia**, wybrano *myVirtualNetwork* sieci wirtualnej i akceptowanie podsieci o nazwie *domyślne* dla **podsieci**. Gdy możesz [utworzona sieć wirtualna](#create-a-virtual-network), zaakceptowane domyślna wartość 10.0.0.0/24 dla podsieci **zakres adresów**. Serwer DHCP systemu Azure przypisuje pierwszy dostępny adres dla wybranej podsieci maszyny wirtualnej. Ponieważ Azure rezerwuje pierwsze cztery adresów (0-3) z każdej podsieci, 10.0.0.4 jest pierwszy dostępny adres IP dostępne dla tej podsieci.

    **Publicznego adresu IP** adres przypisany jest inny niż adres przypisany do maszyny wirtualnej. Domyślnie Azure przypisuje publiczny, adres IP routingu internetowego każdej maszyny wirtualnej. Publiczny adres IP jest przypisany do maszyny wirtualnej z [puli adresów przypisanych do każdego regionu Azure](https://www.microsoft.com/download/details.aspx?id=41653). Azure wie, który publiczny adres IP jest przypisany do maszyny wirtualnej, system operacyjny działający na maszynie wirtualnej nie ma informacji o żadnych publicznego adresu IP, które są przypisane do niej.

8. Wykonaj kroki 1-7 ponownie, ale w kroku 3, Nazwa maszyny wirtualnej *myVm2*. 

9. Po utworzeniu maszyny wirtualnej, kliknij przycisk **sieci**, ponieważ użytkownik w kroku nr 7. Zostanie wyświetlony **prywatnego adresu IP** adres jest *10.0.0.5*. Ponieważ Azure wcześniej przypisywany pierwszy adres można używać *10.0.0.4* w podsieci, aby *myVm1* maszyny wirtualnej, przypisane *10.0.0.5* do  *myVm2* maszyny wirtualnej, ponieważ był następnego dostępnego adresu w podsieci.

## <a name="connect-to-a-virtual-machine"></a>Połącz z maszyną wirtualną

1. Zdalnie łącz się z *myVm1* maszyny wirtualnej. W górnej części portalu Azure, wprowadź *myVm1*. Gdy **myVm1** pojawia się w wynikach wyszukiwania kliknij ją. Kliknij przycisk **Connect** przycisku.

    ![Omówienie maszyny wirtualnej](./media/quick-create-portal/virtual-machine-overview.png)


2. Po kliknięciu przycisku **Connect** przycisku pliku Remote Desktop Protocol (RDP) jest tworzony i pobrana na komputer.  
3. Otwórz plik rdp pobranego. Jeśli zostanie wyświetlony monit, kliknij przycisk **Połącz**. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej, a następnie kliknij przycisk **OK**. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Kliknij przycisk **Tak** lub **Kontynuuj**, aby kontynuować nawiązywanie połączenia.

## <a name="validate-communication"></a>Sprawdź poprawność komunikacji

Próba ping systemu Windows maszyny wirtualnej nie powiedzie się, ponieważ domyślnie ping nie jest dozwolone przez zaporę systemu Windows. Aby umożliwić polecenia ping, aby *myVm1*, wprowadź następujące polecenie w wierszu polecenia:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Aby sprawdzić poprawność komunikację z *myVm2*, wprowadź następujące polecenie w wierszu polecenia *myVm1* maszyny wirtualnej. Podaj poświadczenia, które zostały użyte podczas tworzenia maszyny wirtualnej, a następnie ukończ połączenia:

```
mstsc /v:myVm2
```

Podłączanie pulpitu zdalnego zakończy się pomyślnie, ponieważ obie maszyny wirtualne mają przypisane z prywatnych adresów IP *domyślne* podsieci i dlatego pulpitu zdalnego jest otwarty przez zaporę systemu Windows, domyślnie. Możesz połączyć się z *myVm2* przez hosta o nazwie, ponieważ platforma Azure automatycznie udostępnia rozpoznawanie nazw DNS dla wszystkich hostów w sieci wirtualnej. W wierszu polecenia polecenie ping *myVm1*, z *myVm2*.

```
ping myvm1
```

Polecenie ping zakończy się pomyślnie, ponieważ jego dozwolone przez zaporę systemu Windows na *myVm1* maszyny wirtualnej w poprzednim kroku. Aby potwierdzić wychodzącego komunikację z Internetem, wprowadź następujące polecenie:

```
ping bing.com
```

Otrzymasz cztery odpowiedzi z bing.com. Domyślnie ruch wychodzący do Internetu może komunikować się żadnej maszyny wirtualnej w sieci wirtualnej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, Usuń grupę zasobów i całą jego zawartość. W górnej części portalu Azure, wprowadź *myResourceGroup*. Gdy **myResourceGroup** pojawia się w wynikach wyszukiwania kliknij ją. Kliknij polecenie **Usuń**.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule została wdrożona domyślna sieci wirtualnej z jedną podsiecią i dwie maszyny wirtualne. Aby dowiedzieć się, jak utworzyć sieć wirtualną niestandardowe z wieloma podsieciami i wykonywać zadania zarządzania podstawowe, nadal samouczek dotyczący tworzenia niestandardowych sieci wirtualnej i zarządzanie nią.


> [!div class="nextstepaction"]
> [Tworzenie niestandardowych sieci wirtualnej i zarządzanie nim](virtual-networks-create-vnet-arm-pportal.md#portal)
