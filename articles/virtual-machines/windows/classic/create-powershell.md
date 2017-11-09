---
title: "Tworzenie maszyny Wirtualnej systemu Windows przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Tworzenie maszyn wirtualnych z systemem Windows przy użyciu programu Azure PowerShell i klasycznym modelu wdrażania."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: af672a873b33cbd0b6151fd564e84c96f0b6e1e3
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a>Utwórz maszynę wirtualną systemu Windows PowerShell i klasycznym modelu wdrażania
> [!div class="op_single_selector"]
> * [Portal Azure — systemu Windows](tutorial.md)
> * [PowerShell — systemu Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Dowiedz się, jak [wykonać te kroki przy użyciu modelu usługi Resource Manager](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Te kroki pokazują, jak dostosować zestaw poleceń programu Azure PowerShell, które tworzą i wstępnie skonfigurować maszyny wirtualnej opartych na systemie Windows Azure przy użyciu podejścia bloku konstrukcyjnego. Można użyć tego procesu, aby szybko utworzyć zestaw poleceń dla nowej maszyny wirtualnej z systemem Windows i rozwiń istniejącego wdrożenia lub utworzyć wiele zestawów poleceń, które szybko tworzyć niestandardowe i testowania lub środowisku specjalista IT.

Te kroki należy wykonać podejście wypełniania-w — przeznaczone do bezpośredniego tworzenia zestawy poleceń programu PowerShell systemu Azure. Ta metoda może być przydatna, jeśli jesteś nowym użytkownikiem programu PowerShell lub po prostu chcesz wiedzieć, jakie wartości, aby określić, czy konfiguracja zakończyła się pomyślnie. Użytkownicy wersji Advanced programu PowerShell można wykonać polecenia i podstaw wartości zmiennych (wiersze rozpoczynające się od "$").

Jeśli jeszcze tego nie zrobiono tego wcześniej, postępuj zgodnie z instrukcjami w [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview) do zainstalowania programu Azure PowerShell na komputerze lokalnym. Następnie należy otworzyć wiersz polecenia programu Windows PowerShell.

## <a name="step-1-add-your-account"></a>Krok 1: Dodaj swoje konto
1. W wierszu polecenia programu PowerShell wpisz **Add-AzureAccount** i kliknij przycisk **Enter**. 
2. Wpisz adres e-mail skojarzony z subskrypcją platformy Azure i kliknij przycisk **Kontynuuj**. 
3. Wpisz hasło dla swojego konta. 
4. Kliknij przycisk **Zaloguj**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Krok 2: Ustaw subskrypcji i konto magazynu
Ustaw Twojej subskrypcji platformy Azure i konto magazynu, uruchamiając następujące polecenia w wierszu polecenia programu Windows PowerShell. Zastąp wszystko w obrębie oferty, w tym < i > znaków o poprawne nazwy.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Nazwa subskrypcji poprawne można pobrać z właściwości Nazwa subskrypcji dane wyjściowe **Get-AzureSubscription** polecenia. Nazwa konta magazynu poprawne można pobrać z właściwości Label dane wyjściowe **Get-AzureStorageAccount** polecenia po uruchomieniu **AzureSubscription wybierz** polecenia.

## <a name="step-3-determine-the-imagefamily"></a>Krok 3: Określanie ImageFamily
Następnie należy określić ImageFamily lub etykieta wartości dla określonego obrazu odpowiadającej maszynie wirtualnej Azure, który chcesz utworzyć. Można uzyskać listę dostępnych wartości ImageFamily za pomocą tego polecenia.

    Get-AzureVMImage | select ImageFamily -Unique

Poniżej przedstawiono przykładowe wartości ImageFamily dla komputerów z systemem Windows:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* Windows Server 2016 Technical Preview 4
* SQL Server 2012 z dodatkiem SP1 Enterprise w systemie Windows Server 2012

Jeśli okaże się obraz, którego szukasz, otwórz wystąpienie świeże edytora tekstu, wybór lub programu PowerShell Integrated Scripting Environment (ISE). Skopiuj następujące do nowego pliku tekstowego lub programu PowerShell ISE, zastępując wartości ImageFamily.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

W niektórych przypadkach nazwa obrazu jest zamiast wartości ImageFamily właściwości etykiety. Jeśli nie możesz znaleźć obrazu, który chcesz uzyskać za pomocą właściwości ImageFamily, listy obrazów za ich właściwości Label, przy użyciu tego polecenia.

    Get-AzureVMImage | select Label -Unique

Jeśli okaże się prawy obraz za pomocą tego polecenia, otwórz wystąpienie świeże edytora tekstu, wybór lub programu PowerShell ISE. Skopiuj następujące do nowego pliku tekstowego lub programu PowerShell ISE, zastępując wartość etykiety.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>Krok 4: Utwórz zestaw poleceń
Tworzenie pozostałe polecenia ustawione przez kopiowanie odpowiedni zestaw bloków poniżej do nowego pliku tekstowego lub ISE, a następnie wypełnianie wartości zmiennych i usuwanie < i > znaków. Zobacz dwa [przykłady](#examples) na końcu tego artykułu pomysł końcowego wyniku.

Uruchom polecenie ustawić, wybierając jedną z tych bloków dwa polecenia (wymagane).

Opcja 1: Określ nazwę maszyny wirtualnej i rozmiarze.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Opcja 2: Określ nazwę, rozmiar i nazwa zbioru dostępności.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Dla wartości InstanceSize D, DS lub G serii maszyn wirtualnych, zobacz [maszyny wirtualnej i rozmiary usługi chmury Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Jeśli masz umowy Enterprise Agreement z Software Assurance i zamierzasz korzystać z systemu Windows Server [korzyści Użyj hybrydowego](https://azure.microsoft.com/pricing/hybrid-use-benefit/), Dodaj **- LicenseType** parametr  **Nowy AzureVMConfig** polecenia cmdlet, przekazując wartość **Windows_Server** dla typowy przypadek użycia.  Upewnij się, że używasz obrazu, które zostały przekazane; Nie można używać standardowy obraz w galerii korzyści użyć hybrydowego.
> 
> 

Opcjonalnie na autonomicznym komputerze z systemem Windows Określ konto administratora lokalnego i hasło.

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Wybierz silne hasło. Aby sprawdzić jego siły, zobacz [narzędzie do sprawdzania haseł: używanie silnych haseł](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Opcjonalnie Aby dodać komputer z systemem Windows do istniejącej domeny usługi Active Directory, określić konto administratora lokalnego i hasło, domeny oraz nazwę i hasło konta domeny.

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Dla dodatkowych opcji konfiguracji wstępnej dla maszyn wirtualnych z systemem Windows, zobacz składnię **Windows** i **WindowsDomain** zestawy parametrów [AzureProvisioningConfig Dodaj ](/powershell/module/azure/add-azureprovisioningconfig).

Określony adres IP, znany jako statyczne DIP opcjonalnie przypisać maszynę wirtualną.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Można sprawdzić, czy określony adres IP jest dostępna w programie:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Opcjonalnie przypisać maszynę wirtualną do określonej podsieci w sieci wirtualnej platformy Azure.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

Opcjonalnie Dodaj dysk pojedynczy danych do maszyny wirtualnej.

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Kontroler domeny usługi Active Directory należy ustawić $hcaching "None".

Opcjonalnie dodaj maszynę wirtualną do istniejącego zestawu z równoważeniem obciążenia dla ruchu zewnętrznego.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Na koniec wybierz jedną z tych bloków polecenia wymagane do utworzenia maszyny wirtualnej.

Opcja 1: Utwórz maszynę wirtualną w istniejącej usługi w chmurze.

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

Krótka nazwa usługi w chmurze jest nazwa wyświetlana na liście usługi w chmurze w portalu Azure lub na liście grup zasobów w portalu Azure.

Opcja 2: Utwórz maszynę wirtualną w istniejącej usługi chmury i sieci wirtualnej.

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>Krok 5: Uruchom polecenie zestawu
Przejrzyj zestawu poleceń programu PowerShell systemu Azure, tworzone w edytorze tekstu lub programu PowerShell ISE składające się z wielu bloków poleceń z kroku 4. Upewnij się określono wszystkie wymagane zmienne i mają poprawne wartości. Upewnij się, że po usunięciu wszystkich również < i > znaków.

Korzystając z edytora tekstów, skopiuj zestaw poleceń do Schowka, a następnie kliknij prawym przyciskiem myszy Otwórz wiersz polecenia programu Windows PowerShell. Spowoduje to wystawiać zestawu poleceń w postaci serii poleceń programu PowerShell i utworzenie maszyny wirtualnej platformy Azure. Alternatywnie Uruchom polecenie w programie PowerShell ISE.

Jeśli zostanie utworzony ponownie tę maszynę wirtualną lub podobne jeden, możesz:

* Zapisz to polecenie Ustaw jako plik skryptu programu PowerShell (*.ps1).
* Zapisz to polecenie Ustaw jako element runbook usługi Automatyzacja Azure w **kont automatyzacji** części portalu Azure.

## <a id="examples"></a>Przykłady
Poniżej przedstawiono dwa przykłady tworzenia zestawy poleceń programu PowerShell usługi Azure utworzonych opartych na systemie Windows Azure maszyny wirtualnej przy użyciu kroków opisanych powyżej.

### <a name="example-1"></a>Przykład 1
Potrzebuję programu PowerShell zestawu poleceń, utworzyć maszynę wirtualną początkowy dla kontrolera domeny usługi Active Directory, który:

* Użyty obraz systemu Windows Server 2012 R2 Datacenter.
* Ma nazwę AZDC1.
* Jest komputerem autonomicznym.
* Ma dysk danych dodatkowych 20 GB.
* Ma statyczny adres IP 192.168.244.4.
* Znajduje się w podsieci wewnętrznej bazy danych AZDatacenter sieci wirtualnej.
* Jest usługą w chmurze Azure TailspinToys.

Oto odpowiedniego programu Azure PowerShell zestaw poleceń do utworzenia tej maszyny wirtualnej z puste wiersze między każdy blok dla czytelności.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>Przykład 2
Potrzebuję programu PowerShell zestawu poleceń, utworzyć maszynę wirtualną dla serwera z biznesowych, który:

* Użyty obraz systemu Windows Server 2012 R2 Datacenter.
* Ma nazwę LOB1.
* Jest elementem członkowskim domeny corp.contoso.com.
* Ma dysk danych dodatkowych 200 GB.
* Znajduje się w podsieci frontonu AZDatacenter sieci wirtualnej.
* Jest usługą w chmurze Azure TailspinToys.

Oto odpowiedniego programu Azure PowerShell zestaw poleceń do utworzenia tej maszyny wirtualnej.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Następne kroki
Jeśli potrzebujesz dysk systemu operacyjnego, który jest większy niż 127 GB, możesz [rozwiń dysk systemu operacyjnego](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

