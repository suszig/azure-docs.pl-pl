


Zbiór dostępności pomaga zachować maszyn wirtualnych dostępnych w trakcie przestoju, takich jak podczas konserwacji. Wprowadzenie do dwóch lub więcej podobnie skonfigurowane maszyny wirtualne w zestawie dostępności tworzy nadmiarowość potrzebne do zapewnienia dostępności aplikacji lub usług, które są wykonywane na komputerze wirtualnym. Aby uzyskać szczegółowe informacje dotyczące sposobu działania, zobacz [Zarządzaj dostępnością maszyn wirtualnych][Manage the availability of virtual machines].

Jest najlepszym rozwiązaniem będzie używać zestawów dostępności i równoważenia obciążenia punktów końcowych do zapewnienia, że aplikacja jest zawsze dostępny i działa wydajnie. Aby uzyskać więcej informacji o punktach końcowych z równoważeniem obciążenia, zobacz [równoważenia obciążenia dla usług infrastruktury platformy Azure][Load balancing for Azure infrastructure services].

Klasyczne maszyny wirtualne można dodać do zestawu przy użyciu jednej z dwóch opcji dostępności:

* [Opcja 1: Tworzenie maszyny wirtualnej i dostępności na tym samym czasie][Option 1: Create a virtual machine and an availability set at the same time]. Następnie należy dodać nowych maszyn wirtualnych w zestawie, podczas tworzenia tych maszyn wirtualnych.
* [Opcja 2: Dodaj istniejącą maszynę wirtualną do zestawu dostępności][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> W klasycznym modelu maszyn wirtualnych, które chcesz umieścić w tym samym zestawie dostępności muszą należeć do tej samej usługi w chmurze.
> 
> 

## <a id="createset"></a>— Opcja 1: tworzenie maszyny wirtualnej i dostępności na tym samym czasie
W tym celu można użyć portalu Azure lub poleceń programu PowerShell systemu Azure.

Aby korzystać z portalu Azure:

1. Jeśli jeszcze tego nie zrobiono, zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu centralnym kliknij **+ nowy**, a następnie kliknij przycisk **maszyny wirtualnej**.
   
    ![Tekst alternatywny obrazu](./media/virtual-machines-common-classic-configure-availability/ChooseVMImage.png)
3. Wybierz obraz maszyny wirtualnej Marketplace, którego chcesz użyć. Można utworzyć maszyny wirtualnej systemu Linux lub Windows.
4. Dla wybranej maszyny wirtualnej, sprawdź, czy model wdrażania jest ustawiony na **klasycznego** , a następnie kliknij przycisk **Utwórz**
   
    ![Tekst alternatywny obrazu](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Wprowadź nazwę maszyny wirtualnej, nazwę użytkownika i hasło (dla komputerów z systemem Windows) lub klucz publiczny SSH (w przypadku maszyn z systemem Linux). 
6. Wybierz rozmiar maszyny Wirtualnej, a następnie kliknij przycisk **wybierz** aby kontynuować.
7. Wybierz **konfiguracji opcjonalnej > zestawu dostępności**, i wybierz zestawu dostępności chcesz dodać maszyny wirtualnej.
   
    ![Tekst alternatywny obrazu](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Przejrzyj ustawienia konfiguracji. Gdy wszystko będzie gotowe, kliknij przycisk **Utwórz**.
9. Podczas gdy platforma Azure tworzy maszynę wirtualną, możesz śledzić postępy w obszarze **maszyn wirtualnych** w menu centralnym.

Aby użyć poleceń programu Azure PowerShell do tworzenia maszyny wirtualnej platformy Azure i dodaj go do zestawu dostępności nowych lub istniejących, zobacz [użycia programu Azure PowerShell do tworzenia i wstępnie skonfigurować maszyn wirtualnych z systemem Windows](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"></a>Opcja 2: Dodaj istniejącą maszynę wirtualną do zestawu dostępności
W portalu Azure można dodać istniejącego klasycznych maszyn wirtualnych do istniejących dostępności ustawić lub Utwórz nową dla nich. (Należy pamiętać, że maszyny wirtualne w tym samym zestawie dostępności muszą należeć do tej samej usługi w chmurze). Te kroki są prawie takie same. Przy użyciu programu Azure PowerShell można dodać maszyny wirtualnej do istniejącego zestawu dostępności.

1. Jeśli nie zostało to jeszcze zrobione, zaloguj się do [portalu Azure](https://portal.azure.com).
2. W menu centralnym kliknij **maszyn wirtualnych (klasyczne)**.
   
    ![Tekst alternatywny obrazu](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Z listy maszyn wirtualnych wybierz nazwę maszyny wirtualnej, który chcesz dodać do zestawu.
4. Wybierz **zestawu dostępności** z maszyny wirtualnej **ustawienia**.
   
    ![Tekst alternatywny obrazu](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Wybierz zestaw dostępności, które chcesz dodać do maszyny wirtualnej. Maszyna wirtualna musi należeć do tej samej usługi w chmurze jako zestawu dostępności.
   
    ![Tekst alternatywny obrazu](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Kliknij pozycję **Zapisz**.

Użycie poleceń programu Azure PowerShell, otwórz sesję programu PowerShell Azure poziomie administratora i uruchom następujące polecenie. Dla symboli zastępczych (takich jak &lt;VmCloudServiceName&gt;), Zastąp wszystko w obrębie oferty, w tym < i > znaków o poprawne nazwy.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> Maszyna wirtualna może dysponować ponownego uruchomienia, aby zakończyć dodawanie go do zestawu dostępności.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

