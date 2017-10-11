

![Maszyny wirtualne w autonomicznej usługi w chmurze](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Jeśli w sieci wirtualnej maszyn wirtualnych, można zdecydować, zestawy dostępności i równoważenia obciążenia ile ma być używany dla usługi w chmurze. Ponadto można organizować maszyn wirtualnych w podsieciach w taki sam sposób jak sieci lokalnej, a także połączyć sieć wirtualną do sieci lokalnej. Oto przykład:

![Maszyny wirtualne w sieci wirtualnej](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Sieci wirtualne są zalecanym sposobem Podłączanie maszyn wirtualnych na platformie Azure. Najlepszym rozwiązaniem jest skonfigurowanie każdej warstwy aplikacji w usłudze w chmurze oddzielne. Jednak może być konieczne łączenie niektórych maszyn wirtualnych z warstwami innej aplikacji w tej samej usługi w chmurze ma pozostać w ciągu maksymalnie 200 usługi w chmurze na subskrypcję. Aby przejrzeć to i inne ograniczenia, zobacz [subskrypcji platformy Azure i ograniczenia usługi, przydziały i ograniczenia](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Połączenie maszyn wirtualnych w sieci wirtualnej
Umożliwia podłączanie maszyn wirtualnych w sieci wirtualnej:

1. Utwórz sieć wirtualną w [portalu Azure](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) i określ "classic deployment".
2. Utwórz zbiór usługi w chmurze dla danego wdrożenia w celu odzwierciedlenia projektu dla zestawów dostępności i równoważenia obciążenia. W portalu Azure, kliknij przycisk **nowe > obliczenia > Usługa w chmurze** dla każdej usługi w chmurze.

  W trakcie wypełniania szczegóły usługi chmury, wybierz taki sam _grupy zasobów_ używane z siecią wirtualną.

3. Aby utworzyć każdej nowej maszyny wirtualnej, kliknij przycisk **nowy > obliczeniowe**, następnie wybierz odpowiedni obraz maszyny Wirtualnej z **wyróżnionych aplikacji**.

  W maszynie Wirtualnej **podstawy** bloku, wybierz taki sam _grupy zasobów_ używane z siecią wirtualną.

  ![Blok podstawowych ustawień maszyny Wirtualnej przy użyciu sieci wirtualnej](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. W trakcie wypełniania maszyny Wirtualnej **ustawienia**, wybierz poprawny _usługi w chmurze_ lub _sieci wirtualnej_ dla maszyny Wirtualnej.

  Azure wybierze inny element zależności.

  ![Blok ustawień maszyny Wirtualnej przy użyciu sieci wirtualnej](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Połączenie maszyn wirtualnych w autonomicznej usługi w chmurze
Umożliwia podłączanie maszyn wirtualnych w autonomicznej usługi w chmurze:

1. Tworzenie usługi w chmurze w [portalu Azure](http://portal.azure.com). Kliknij przycisk **nowe > obliczenia > Usługa w chmurze**. Lub po utworzeniu pierwszej maszyny wirtualnej można utworzyć usługi w chmurze dla danego wdrożenia.
2. Podczas tworzenia maszyn wirtualnych, należy wybrać tej samej grupy zasobów, które są używane z usługą w chmurze.

  ![Dodaj maszynę wirtualną do istniejącej usługi w chmurze](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  W trakcie wypełniania szczegóły maszyny Wirtualnej, wybierz nazwę utworzonego w pierwszym kroku usługi w chmurze.

  ![Wybierając usługę chmury dla maszyny wirtualnej](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
