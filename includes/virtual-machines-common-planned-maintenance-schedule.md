

## <a name="multi-and-single-instance-vms"></a>Obsługa wielu i jednego wystąpienia maszyny wirtualne
Wielu klientów systemem Azure liczba go krytycznych, czy można zaplanować podczas ich maszyny wirtualne przechodzą zaplanowanej konserwacji z powodu przestoju — około 15 minut — występuje podczas konserwacji. Zestawy dostępności można użyć ułatwiające kontrolę podczas zaplanowanej konserwacji odbierania elastycznie maszyn wirtualnych.

Istnieją dwie możliwe konfiguracje dla maszyn wirtualnych działających na platformie Azure. Maszyny wirtualne albo są skonfigurowane jako jednym czy wielu wystąpieniach. W przypadku maszyn wirtualnych w zestawie dostępności, następnie są skonfigurowane z opcją wielu wystąpień. Należy pamiętać, nawet pojedynczy maszyn wirtualnych można wdrożyć w zestawie dostępności, dzięki czemu są one traktowane jako mająca wiele wystąpień. Jeśli maszyny wirtualne nie są dostępne w zestawie dostępności, następnie są skonfigurowane z opcją jednego wystąpienia.  Aby uzyskać szczegółowe informacje na zestawów dostępności, zobacz [Zarządzaj dostępnością maszyn wirtualnych z systemem Windows](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lub [Zarządzaj dostępnością maszyn wirtualnych z systemem Linux](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aktualizacje zaplanowanej konserwacji do maszyn wirtualnych z jednego wystąpienia i wielu wystąpień nastąpić oddzielnie. Ponowne skonfigurowanie maszyn wirtualnych można jednego wystąpienia (jeśli są one wielu wystąpień) lub wielu wystąpień (jeśli są one jednego wystąpienia), można kontrolować, po ich maszyn wirtualnych komunikatu zaplanowanej konserwacji. Zobacz [zaplanowanej konserwacji dla maszyn wirtualnych systemu Linux Azure](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub [zaplanowanej konserwacji dla maszyn wirtualnych Azure Windows](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) szczegółowe informacje o zaplanowanej konserwacji dla maszyn wirtualnych platformy Azure.

## <a name="for-multi-instance-configuration"></a>Mająca wiele wystąpień konfiguracji
Można wybrać podczas zaplanowanej konserwacji ma wpływ na maszyn wirtualnych wdrożonych w konfiguracji zestawu dostępności przez usunięcie tych maszyn wirtualnych z zestawów dostępności.

1. Zostanie wysłana wiadomość e-mail do Ciebie siedmiu dni przed zaplanowanej konserwacji do maszyn wirtualnych w konfiguracji wielu wystąpień. Identyfikatory subskrypcji oraz nazwy odpowiednich maszyn wirtualnych w wielu wystąpieniach znajdują się w treści wiadomości e-mail.
2. Podczas tych siedem dni można wybrać podczas swoich wystąpień są aktualizowane przez usunięcie wielu wystąpień maszyn wirtualnych w tym regionie z ich zestawu dostępności. Ta zmiana w konfiguracji powoduje ponowne uruchomienie komputera, jako maszyna wirtualna jest przenoszona z jednego hosta fizycznego, przeznaczony dla konserwacji do innego hosta fizycznego, który nie jest przeznaczone do obsługi.
3. Należy usunąć maszynę Wirtualną z jego zestawem dostępności w portalu Azure.

   1. W portalu wybierz maszynę Wirtualną, aby usunąć z zestawu dostępności.  

   2. W obszarze **ustawienia**, kliknij przycisk **zestawu dostępności**.

      ![Wybór zestawu dostępności](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. W zestawie dostępności menu rozwijane, wybierz opcję "Nie jest częścią zestawu dostępności."

      ![Usuń ze zbioru](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. U góry, kliknij przycisk **zapisać**. Kliknij przycisk **tak** potwierdzić, że ta akcja ponownego uruchomienia maszyny Wirtualnej.

   >[!TIP]
   >Do wielu wystąpień maszyny Wirtualnej można zmienić później, wybierając jedną z zestawów dostępności wymienionych.

4. Usuwane z zestawów dostępności maszyn wirtualnych zostaną przeniesione do jednego wystąpienia hostów i nie są aktualizowane podczas zaplanowanej konserwacji do konfiguracji ustawić dostępności.
5. Po zakończeniu aktualizacji do maszyn wirtualnych dostępności Ustaw (zgodnie z harmonogramem opisane w oryginalnej wiadomości e-mail), należy dodać maszyn wirtualnych do ich zestawów dostępności. Staje się częścią zestawu dostępności jako występująca ponownie konfiguruje maszyn wirtualnych i powoduje ponowne uruchomienie komputera. Zwykle po wykonaniu wszystkich aktualizacji wielu wystąpień w całego środowiska Azure następuje konserwacji jednego wystąpienia.

Usuwanie maszyny Wirtualnej z zestawu dostępności może być również uzyskany przy użyciu programu Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Dla jednego wystąpienia konfiguracji
Można wybrać podczas zaplanowanej konserwacji ma wpływ na możesz maszyn wirtualnych w konfiguracji jednego wystąpienia przez dodanie tych maszyn wirtualnych do zestawów dostępności.

Krok po kroku

1. Zostanie wysłana wiadomość e-mail do Ciebie siedmiu dni przed zaplanowanej konserwacji do maszyn wirtualnych w konfiguracji jednego wystąpienia. Identyfikatory subskrypcji oraz nazwy odpowiednich maszyn wirtualnych w jednym wystąpieniu znajdują się w treści wiadomości e-mail.
2. Podczas tych siedem dni można wybrać podczas rozruchu wystąpienia przez dodanie jednego wystąpienia maszyn wirtualnych do zestawem dostępności w tym samym regionie. Ta zmiana w konfiguracji powoduje ponowne uruchomienie komputera, jako maszyna wirtualna jest przenoszona z jednego hosta fizycznego, przeznaczony dla konserwacji do innego hosta fizycznego, który nie jest przeznaczone do obsługi.
3. Wykonaj instrukcje tutaj, aby dodać istniejące maszyny wirtualne do zestawów dostępności za pomocą portalu Azure i programu Azure PowerShell. (Zobacz przykład programu Azure PowerShell, który obejmuje następujące kroki).
4. Gdy te maszyny wirtualne są konfigurowane jako mająca wiele wystąpień, do maszyn wirtualnych z jednego wystąpienia są wyłączone z zaplanowanej konserwacji.
5. Po zakończeniu aktualizacji wirtualna jednego wystąpienia (zgodnie z harmonogramem w oryginalnej wiadomości e-mail), można powrócić do jednego wystąpienia maszyn wirtualnych i przez usunięcie maszyn wirtualnych z ich zestawów dostępności.

Dodawanie maszyny Wirtualnej do także zestaw danych o dostępności można osiągnąć za pomocą programu Azure PowerShell:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
