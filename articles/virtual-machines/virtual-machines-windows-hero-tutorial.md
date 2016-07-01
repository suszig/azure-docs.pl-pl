<properties
    pageTitle="Tworzenie pierwszej maszyny wirtualnej z systemem Windows | Microsoft Azure"
    description="Dowiedz się, jak utworzyć pierwszą maszynę wirtualną z systemem Windows przy użyciu Portalu Azure."
    keywords="Windows virtual machine,create a virtual machine,virtual computer,setting up a virtual machine"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/05/2016"
    ms.author="cynthn"/>

# Tworzenie pierwszej maszyny wirtualnej z systemem Windows w Portalu Azure

Ten samouczek pokazuje, jak łatwo można w ciągu kilku minut utworzyć maszynę wirtualną z systemem Windows przy użyciu Portalu Azure.  

Jeśli nie masz subskrypcji Azure, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) w zaledwie kilka minut.

Oto [przewodnik wideo](https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Create-A-Virtual-Machine-Running-Windows-In-The-Azure-Preview-Portal) tego samouczka. 


## Wybieranie obrazu maszyny wirtualnej z portalu Marketplace

Jako przykładu używamy obrazu systemu Windows Server 2012 R2 Datacenter, ale jest to tylko jeden z wielu obrazów oferowanych na platformie Azure. Obrazy dostępne do wyboru zależą od subskrypcji. Przykładowo obrazy komputerowe mogą być dostępne dla [subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Zaloguj się do [Portalu Azure](https://portal.azure.com).

2. W menu Centrum kliknij pozycję **Nowy** > **Virtual Machines** > **Windows Server 2012 R2 Datacenter**.

    ![Zrzut ekranu pokazujący dostępne w portalu obrazy maszyn wirtualnych Azure](./media/virtual-machines-windows-hero-tutorial/marketplace-new.png)


3. Na stronie **Windows Server 2012 R2 Datacenter** w sekcji **Wybierz model wdrożenia** sprawdź, czy wybrano pozycję **Resource Manager**. Kliknij pozycję **Utwórz**.

    ![Zrzut ekranu pokazujący model wdrażania do wybrania dla maszyny wirtualnej](./media/virtual-machines-windows-hero-tutorial/deployment-model.png)

## Tworzenie maszyny wirtualnej z systemem Windows

Po wybraniu obrazu możesz użyć domyślnych ustawień platformy Azure dla większości elementów konfiguracji i szybko utworzyć maszynę wirtualną.

1. W bloku **Podstawowe** wprowadź nazwę maszyny wirtualnej w polu **Nazwa**. Nazwa musi składać się z 1–15 znaków i nie może zawierać znaków specjalnych.

2. Podaj nazwę użytkownika w polu **Nazwa użytkownika** i silne hasło w polu **Hasło**. Zostaną one użyte do utworzenia konta lokalnego na maszynie wirtualnej. Konto lokalne jest używane do logowania się do maszyny wirtualnej i zarządzania nią. 

    Hasło musi mieć długość od 8 do 123 znaków i musi zawierać co najmniej 3 z następujących typów znaków: mała litera, wielka litera, cyfra i znak specjalny. 


3. Wybierz istniejącą [grupę zasobów](../resource-group-overview.md#resource-groups) lub wprowadź nazwę nowej grupy zasobów. W polu **Lokalizacja** wprowadź lokalizację centrum danych Azure, np. **Zachodnie stany USA**. 

4. Gdy skończysz, kliknij przycisk **OK**, aby przejść do następnej sekcji. 

    ![Zrzut ekranu pokazujący ustawienia w bloku Podstawowe służące do konfigurowania maszyny wirtualnej Azure](./media/virtual-machines-windows-hero-tutorial/basics-blade.png)

    
5. Wybierz [rozmiar](virtual-machines-windows-sizes.md) maszyny wirtualnej, a następnie kliknij przycisk **Wybierz**, aby kontynuować. 

    ![Zrzut ekranu bloku Rozmiar pokazujący dostępne do wyboru rozmiary maszyn wirtualnych Azure](./media/virtual-machines-windows-hero-tutorial/size-blade.png)

6. W bloku **Ustawienia** możesz zmienić opcje magazynu i sieci. W przypadku pierwszej maszyny wirtualnej zazwyczaj można zaakceptować ustawienia domyślne. Jeśli wybrano rozmiar maszyny wirtualnej, który obsługuję tę funkcję, możesz wypróbować usługę Premium Storage, wybierając opcję **Premium (SSD)** w obszarze **Typ dysku**. Po zakończeniu wprowadzania zmian kliknij przycisk **OK**.

    ![Zrzut ekranu bloku Ustawienia, w którym możesz skonfigurować opcjonalne funkcje maszyny wirtualnej Azure](./media/virtual-machines-windows-hero-tutorial/settings-blade.png)

7. Kliknij pozycję **Podsumowanie**, aby przejrzeć wybrane opcje. Gdy skończysz, kliknij przycisk **OK**.

    ![Zrzut ekranu strony Podsumowanie, która zawiera opcje konfiguracji wybrane dla maszyny wirtualnej Azure](./media/virtual-machines-windows-hero-tutorial/summary-blade.png)

8. Podczas tworzenia maszyny wirtualnej przez platformę Azure, możesz śledzić postęp w obszarze **Virtual Machines** w menu Centrum. 


## Nawiązywanie połączenia z maszyną wirtualną i logowanie

1.  W menu Centrum kliknij pozycję **Virtual Machines**.

2.  Wybierz maszynę wirtualną z listy.

3. W bloku maszyny wirtualnej kliknij pozycję **Połącz**. Operacja spowoduje utworzenie i pobranie pliku Remote Desktop Protocol (rdp), który jest swego rodzaju skrótem do nawiązywania połączenia z maszyną wirtualną. Możesz zapisać plik na pulpicie, aby zapewnić sobie łatwy dostęp. **Otwórz** ten plik, aby nawiązać połączenie z maszyną wirtualną.

    ![Zrzut ekranu Portalu Azure pokazujący sposób nawiązywania połączenia z maszyną wirtualną.](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Zostanie wyświetlone ostrzeżenie, że plik rdp pochodzi od nieznanego wydawcy. Jest to normalne. W oknie pulpitu zdalnego kliknij przycisk **Połącz**, aby kontynuować.

    ![Zrzut ekranu przedstawiający ostrzeżenie o nieznanym wydawcy.](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. W oknie Zabezpieczenia systemu Windows wpisz nazwę użytkownika i hasło konta lokalnego, które zostało utworzone podczas tworzenia maszyny wirtualnej. Wprowadź nazwę użytkownika w formacie *nazwa_maszyny_wirtualnej*&#92;*nazwa_użytkownika*, a następnie kliknij przycisk **OK**.

    ![Zrzut ekranu przedstawiający wprowadzanie nazwy maszyny wirtualnej, nazwy użytkownika i hasła.](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Zostanie wyświetlone ostrzeżenie, że nie można zweryfikować certyfikatu. Jest to normalne. Kliknij przycisk **Tak**, aby potwierdzić tożsamość maszyny wirtualnej i zakończyć logowanie.

    ![Zrzut ekranu przedstawiający komunikat dotyczący potwierdzenia tożsamości maszyny wirtualnej.](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Jeśli podczas próby połączenia wystąpiły problemy, zobacz [Troubleshoot Remote Desktop connections to a Windows-based Azure Virtual Machine](virtual-machines-windows-troubleshoot-rdp-connection.md) (Rozwiązywanie problemów z połączeniami pulpitu zdalnego z maszyną wirtualną Azure z systemem Windows).

Teraz możesz pracować z maszyną wirtualną tak samo, jak z każdym innym serwerem.

## Zatrzymywanie maszyny wirtualnej

Dobrym rozwiązaniem jest zatrzymanie maszyny wirtualnej w celu uniknięcia opłat, gdy maszyna nie jest używana. Po prostu kliknij przycisk **Zatrzymaj**, a następnie przycisk **Tak**.

![Zrzut ekranu przedstawiający przycisk zatrzymywania maszyny wirtualnej.](./media/virtual-machines-windows-hero-tutorial/stop-vm.png)
    
Kliknij przycisk **Uruchom**, aby ponownie uruchomić maszynę wirtualną, gdy wszystko będzie gotowe do jej ponownego użycia.


## Następne kroki

* Poeksperymentuj z [dodawaniem dysku danych](virtual-machines-windows-attach-disk-portal.md) do maszyny wirtualnej. Dyski danych zapewniają więcej pamięci masowej dla maszyny wirtualnej.

* [Utwórz maszynę wirtualną z systemem Windows przy użyciu programu PowerShell](virtual-machines-windows-ps-create.md) lub [utwórz maszynę wirtualną z systemem Linux](virtual-machines-linux-quick-create-cli.md) przy użyciu interfejsu wiersza polecenia platformy Azure.



<!--HONumber=Jun16_HO2-->


