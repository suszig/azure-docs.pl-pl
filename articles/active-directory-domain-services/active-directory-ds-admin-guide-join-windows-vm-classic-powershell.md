---
title: "Usługami domenowymi Azure Active Directory: Przewodnik administrowania | Dokumentacja firmy Microsoft"
description: "Przyłącz maszynę wirtualną systemu Windows do domeny zarządzanej przy użyciu programu Azure PowerShell i klasycznym modelu wdrażania."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Dołącz maszynę wirtualną systemu Windows Server do domeny zarządzanej przy użyciu programu PowerShell
> [!div class="op_single_selector"]
> * [Klasyczny portal Azure — systemu Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell — systemu Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Azure Resource Manager i model klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). Ten artykuł dotyczy klasycznego modelu wdrożenia. Usługi domenowe Azure AD aktualnie nie obsługuje modelu usługi Resource Manager.
>
>

Te kroki pokazują, jak dostosować zestaw poleceń programu Azure PowerShell, które tworzą i wstępnie skonfigurować maszyny wirtualnej opartych na systemie Windows Azure przy użyciu podejścia bloku konstrukcyjnego. Te kroki ułatwiające tworzenie maszyny wirtualnej opartych na systemie Windows Azure i przyłączyć go do domeny zarządzanej usług domenowych Azure AD.

Te kroki należy wykonać podejście wypełniania-w — przeznaczone do bezpośredniego tworzenia zestawy poleceń programu PowerShell systemu Azure. Ta metoda może być przydatna, jeśli jesteś nowym użytkownikiem programu PowerShell lub chcesz dowiedzieć się, jakie wartości, aby określić, czy konfiguracja zakończyła się pomyślnie. Użytkownicy wersji Advanced programu PowerShell można wykonać polecenia i podstaw wartości zmiennych (wiersze rozpoczynające się od "$").

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

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Krok 3: Przewodnik krok po kroku - aprowizowanie maszyny wirtualnej i przyłączyć go do domeny zarządzanej
Oto odpowiedniego programu Azure PowerShell zestaw poleceń do utworzenia tej maszyny wirtualnej z puste wiersze między każdy blok dla czytelności.

Określ informacje dotyczące maszyny wirtualnej systemu Windows do obsługi administracyjnej.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Dla wartości InstanceSize D, DS lub G serii maszyn wirtualnych, zobacz [maszyny wirtualnej i rozmiary usługi chmury Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Zawierają informacje dotyczące domeny zarządzanej.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Określ nazwę usługi w chmurze.

    $svcname="Contoso100-test"

Określ nazwę sieci wirtualnej, do którego powinny zostać przyłączone maszyny Wirtualnej. Upewnij się, że domeny zarządzanej usługi AAD DS jest dostępny w tej sieci wirtualnej.

    $vnetname="MyPreviewVnet"

Wybierz obraz maszyny Wirtualnej, aby go użyć do udostępnienia maszyny Wirtualnej.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Skonfiguruj maszynę Wirtualną — Ustaw nazwę maszyny Wirtualnej, rozmiar wystąpienia & obraz ma być używany.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Uzyskaj poświadczenia administratora lokalnego dla maszyny Wirtualnej. Wybierz hasło administratora lokalnego silne.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Uzyskaj poświadczenia dla konta użytkownika należącego do grupy "Administratorzy kontrolera domeny usługi AAD" do przyłączenia do domeny zarządzanej maszyny Wirtualnej. Nie określaj na przykład nazwa domeny — w naszym przykładzie, "bob" jest określona jako nazwy użytkownika.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Skonfiguruj maszynę Wirtualną — Określ wymagania sprzężenia domeny & wymagane poświadczenia.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Ustaw podsieci dla maszyny Wirtualnej.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Opcjonalnie: Wskaż adres IP domeny. Jeśli ustawisz adresy IP domeny zarządzanej usług domenowych Azure AD, serwery DNS dla sieci wirtualnej, ten krok nie jest wymagane.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Teraz obsłużyć maszyny Wirtualnej systemu Windows przyłączone do domeny.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skrypt obsługi administracyjnej maszyny Wirtualnej systemu Windows i automatycznie dołączone do domeny zarządzanej usług domenowych w usłudze AAD
Ten zestaw poleceń programu PowerShell tworzy maszynę wirtualną dla serwera z biznesowych, który:

* Użyty obraz systemu Windows Server 2012 R2 Datacenter.
* Jest bardzo małe maszyny wirtualnej.
* Nazwa testu Contoso100 jest.
* Jest automatycznie przyłączone do domeny do domeny zarządzanej contoso100.
* Zostanie dodany do tej samej sieci wirtualnej jako domeny zarządzanej.

W tym miejscu jest pełna przykładowy skrypt, aby utworzyć maszynę wirtualną systemu Windows i automatycznie dołączone do domeny zarządzanej usług domenowych Azure AD.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Powiązana zawartość
* [Usługi domenowe AD Azure - Przewodnik wprowadzający](active-directory-ds-getting-started.md)
* [Administrowanie domeną zarządzaną usług Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
