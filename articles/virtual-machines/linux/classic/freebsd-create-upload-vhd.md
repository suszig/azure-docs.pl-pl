---
title: "Utwórz i Przekaż obraz maszyny Wirtualnej FreeBSD | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzenie i przekazywanie wirtualnego dysku twardego (VHD) z systemem operacyjnym FreeBSD, aby utworzyć maszynę wirtualną platformy Azure"
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: huishao
ms.openlocfilehash: 0010e01d4333b96696680ec6fbbeee74b17f46a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Tworzenie i przekazywanie wirtualnego dysku twardego FreeBSD na platformie Azure
W tym artykule opisano tworzenie i przekazywanie wirtualnego dysku twardego (VHD) z systemem operacyjnym FreeBSD. Po wysłaniu, służy ona jako własnego obrazu można utworzyć maszynę wirtualną (VM) na platformie Azure.

> [!IMPORTANT] 
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Aby dowiedzieć się, jak przekazywanie wirtualnego dysku twardego za pomocą modelu Resource Manager, zobacz [tutaj](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule przyjęto założenie, że masz następujące elementy:

* **Subskrypcja platformy Azure**— Jeśli nie masz konta, możesz utworzyć jedną w zaledwie kilka minut. Jeśli masz subskrypcję MSDN, zobacz [Azure miesięczne środki dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). W przeciwnym razie Dowiedz się, jak [utworzyć bezpłatne konto próbne](https://azure.microsoft.com/pricing/free-trial/).  
* **Narzędzia programu PowerShell Azure**--modułu Azure PowerShell musi być zainstalowana i skonfigurowana do pracy z subskrypcją platformy Azure. Aby pobrać module, zobacz [Azure pobiera](https://azure.microsoft.com/downloads/). Samouczek, który opisuje sposób instalowania i konfigurowania modułu jest dostępnych tutaj. Użyj [Azure pobiera](https://azure.microsoft.com/downloads/) polecenia cmdlet w celu przekazania dysku VHD.
* **FreeBSD zainstalowanego systemu operacyjnego w pliku VHD**musi być zainstalowany system operacyjny — obsługiwane FreeBSD do wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD. Na przykład umożliwia rozwiązanie wirtualizacji, takich jak funkcja Hyper-V Utwórz plik VHD i zainstalowania systemu operacyjnego. Aby uzyskać instrukcje dotyczące instalowania i używania funkcji Hyper-V, zobacz [Instalowanie funkcji Hyper-V i tworzenie maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Nowszy format VHDX nie jest obsługiwane na platformie Azure. Dysk można przekonwertować na format wirtualnego dysku twardego za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx). Ponadto istnieje [samouczek w witrynie MSDN dotyczące korzystania z funkcji Hyper-V FreeBSD](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

To zadanie obejmuje następujące kroki 5:

## <a name="step-1-prepare-the-image-for-upload"></a>Krok 1: Przygotowanie obrazu do przekazywania
Na maszynie wirtualnej, w którym zainstalowany jest system operacyjny FreeBSD wykonaj następujące procedury:

1. Włącz protokół DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. Włącz SSH.

    Upewnij się, że serwer SSH jest zainstalowany i skonfigurowany do uruchamiania w czasie rozruchu. Domyślnie jest włączone po instalacji z dysku FreeBSD. 
3. Konfigurowanie konsoli szeregowej.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Zainstaluj plik sudo.

    Konta głównego jest wyłączona w systemie Azure. Oznacza to, że należy korzystać z sudo z nieuprawnionego użytkownika do uruchomienia polecenia z podwyższonym poziomem uprawnień.

        # pkg install sudo
   
5. Wymagania wstępne dotyczące agenta platformy Azure.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Zainstaluj agenta platformy Azure.

    Najnowszą wersję agenta programu Azure zawsze można znaleźć w [github](https://github.com/Azure/WALinuxAgent/releases). Wersja 2.0.10 oficjalnie obsługuje FreeBSD 10 & 10.1 oraz wersji 2.1.4 + (w tym 2.2.x) obsługuje oficjalnie FreeBSD 10.2 i jego nowszych wersjach.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0 Użyjmy 2.0.16 na przykład:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1 możemy użyć 2.1.4 na przykład:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Po zainstalowaniu agenta platformy Azure jest dobrym pomysłem jest sprawdzenie, czy działa:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Anulowanie zastrzeżenia systemu.

    Anulowanie zastrzeżenia systemu do czyszczenia go i była odpowiednia dla reprovisioning. Następujące polecenie spowoduje również usunięcie ostatnie konto użytkownika elastycznie i skojarzone dane:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Teraz można zamknąć maszyny Wirtualnej.

## <a name="step-2-create-a-storage-account-in-azure"></a>Krok 2: Utwórz konto magazynu na platformie Azure
Potrzebujesz konta magazynu na platformie Azure, aby przekazać plik VHD, może służyć do tworzenia maszyny wirtualnej. Klasyczny portal Azure umożliwia tworzenie konta magazynu.

1. Zaloguj się do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. Na pasku poleceń Wybierz **nowy**.
3. Wybierz **usług danych** > **magazynu** > **szybkie tworzenie**.

    ![Szybkie tworzenie konta magazynu](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. Należy wypełnić pola w następujący sposób:

   * W **adres URL** wpisz nazwę domeny podrzędnej do użycia w adresie URL konta magazynu. Wpis może zawierać od 3 do 24 cyfry i małe litery. Ta nazwa jest używana jako nazwa hosta, w ramach adresu URL, która jest wykorzystywana do adresowania magazynu obiektów Blob platformy Azure, magazyn kolejek Azure lub zasobów magazynu tabel Azure dla subskrypcji.
   * W **grupa lokalizacji/koligacji** menu rozwijanego wybierz **lokalizacja lub grupa koligacji** dla konta magazynu. Grupa koligacji pozwala umieścić usługi w chmurze i magazynu w tym samym centrum danych.
   * W **replikacji** Określ, czy używać **geograficznie nadmiarowego** replikacji dla konta magazynu. Replikacja geograficzna jest domyślnie włączona. Ta opcja replikuje dane do lokalizacji dodatkowej, bez ponoszenia kosztów, tak, aby Magazyn awaryjnie przełącza działanie do tej lokalizacji w przypadku poważnej awarii w lokalizacji głównej. Lokalizacja dodatkowa jest przypisywany automatycznie i nie można zmienić. Jeśli potrzebujesz większej kontroli nad lokalizacji magazynu oparte na chmurze ze względu na wymagania prawne i zasady organizacji, można wyłączyć replikacji geograficznej. Należy jednak pamiętać, że jeśli później włączyć replikację geograficzną zostanie naliczona opłaty transfer jednorazowe danych w celu replikowania istniejących danych do lokalizacji dodatkowej. Usługi magazynu bez — replikacja geograficzna jest oferowany z rabatem. Więcej informacji o zarządzaniu — replikacja geograficzna kont magazynu można znaleźć tutaj: [replikacja usługi Azure Storage](../../../storage/common/storage-redundancy.md).

     ![Wprowadź szczegóły konta magazynu](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. Wybierz **utworzyć konto magazynu**. Konto jest teraz wyświetlany w obszarze **magazynu**.

    ![Pomyślnie utworzono konto magazynu](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. Następnie należy utworzyć kontener dla plików VHD przekazane. Wybierz nazwę konta magazynu, a następnie wybierz **kontenery**.

    ![Szczegóły konta magazynu](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. Wybierz **utworzyć kontener**.

    ![Szczegóły konta magazynu](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. W **nazwa** wpisz nazwę użytkownika kontenera. Następnie w **dostępu** menu rozwijanego, wybierz typ zasad dostępu ma.

    ![Nazwa kontenera](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > Domyślnie kontener jest prywatny i jest dostępny tylko przez właściciela konta. Aby zezwolić na publiczny dostęp do odczytu do obiektów blob w kontenerze, ale nie do właściwości kontenera i metadanych, należy użyć **publicznego obiektu Blob** opcji. Aby umożliwić pełnej publiczny dostęp do odczytu do kontenera i obiektów blob, należy użyć **publicznego kontenera** opcji.
   >
   >

## <a name="step-3-prepare-the-connection-to-azure"></a>Krok 3: Przygotowanie połączenia z platformą Azure
Zanim można przekazać pliku VHD, należy ustanowić bezpiecznego połączenia między komputerem i Twojej subskrypcji platformy Azure. Aby wykonać to zadanie, można użyć metody usługi Azure Active Directory (Azure AD) lub certyfikatów.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Przekaż plik VHD przy użyciu metody usługi Azure AD
1. Otwórz konsolę programu Azure PowerShell.
2. Wpisz następujące polecenie:  
    `Add-AzureAccount`

    To polecenie powoduje otwarcie okna logowania której możesz zalogować się przy użyciu konta służbowego.

    ![Okno programu PowerShell](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. Azure uwierzytelnia i zapisanie informacji o poświadczeniach. Następnie zamyka okna.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Przekaż plik VHD przy użyciu metody certyfikatu
1. Otwórz konsolę programu Azure PowerShell.
2. Typ: `Get-AzurePublishSettingsFile`.
3. Oknie przeglądarki zostanie otwarty i wyświetli monit o pobranie pliku .publishsettings. Ten plik zawiera informacje i certyfikatu dla Twojej subskrypcji platformy Azure.

    ![Strona pobierania przeglądarki](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. Zapisz plik .publishsettings.
5. Typ: `Import-AzurePublishSettingsFile <PathToFile>`, gdzie `<PathToFile>` jest pełną ścieżką do pliku .publishsettings.

   Aby uzyskać więcej informacji, zobacz [wprowadzenie do poleceń cmdlet systemu Azure](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Aby uzyskać więcej informacji na temat instalowania i konfigurowania programu PowerShell, zobacz [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

## <a name="step-4-upload-the-vhd-file"></a>Krok 4: Przekaż plik VHD
Podczas przekazywania pliku VHD, należy go umieścić w dowolnym w ramach usługi magazynu obiektów Blob. Poniżej przedstawiono niektóre warunki, które będą używane podczas przekazywania pliku:

* **BlobStorageURL** jest adres URL dla konta magazynu, który został utworzony w kroku 2.
* **YourImagesFolder** jest kontenera w magazynie obiektów Blob, w którym mają być przechowywane obrazy.
* **VHDName** jest etykietę wyświetlaną w klasycznym portalu Azure, aby zidentyfikować wirtualnego dysku twardego.
* **PathToVHDFile** jest pełna ścieżka i nazwa pliku VHD.

W oknie programu Azure PowerShell używanego w poprzednim kroku wpisz:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Krok 5: Tworzenie maszyny Wirtualnej z pliku VHD przekazany
Po przekazaniu pliku VHD, można dodać go jako obraz do listy niestandardowych obrazów, które są skojarzone z subskrypcją i Utwórz maszynę wirtualną z tego obrazu niestandardowego.

1. W oknie programu Azure PowerShell używanego w poprzednim kroku wpisz:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > Użyj systemu Linux jako typ systemu operacyjnego. Bieżąca wersja programu Azure PowerShell akceptuje tylko "Linux" lub "Windows" jako parametru.
   >
   >
2. Po wykonaniu poprzednich kroków nowy obraz znajduje się w przypadku **obrazów** karty w klasycznym portalu Azure.  

    ![Wybierz obraz](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Utwórz maszynę wirtualną z galerii. Ten nowy obraz jest teraz dostępna w obszarze **Moje obrazy**.
4. Wybierz nowy obraz. Następnie należy przejść przez z monitami, aby skonfigurować nazwę hosta, hasła, klucz SSH i tak dalej.

    ![Obraz niestandardowy](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Po ukończeniu inicjowania obsługi administracyjnej, zobaczysz maszyny Wirtualnej FreeBSD działające na platformie Azure.

    ![Obraz FreeBSD na platformie azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
