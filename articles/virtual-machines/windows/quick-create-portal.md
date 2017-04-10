---
title: "Azure: Szybki start — Tworzenie maszyn wirtualnych z systemem Windows za pomocą portalu | Microsoft Docs"
description: "Azure: Szybki start — Tworzenie maszyn wirtualnych z systemem Windows za pomocą portalu"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 3cb94144addabf2371c8a058cebff25def4b6338
ms.lasthandoff: 04/04/2017

---

# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a>Tworzenie maszyny wirtualnej z systemem Windows za pomocą witryny Azure Portal

Maszyny wirtualne platformy Azure można utworzyć za pomocą witryny Azure Portal. Ta metoda bazuje na opartym na przeglądarce interfejsie użytkownika umożliwiającym tworzenie i konfigurowanie maszyn wirtualnych oraz wszystkich pokrewnych zasobów. Ten przewodnik Szybki start przeprowadza użytkownika przez proces tworzenia maszyny wirtualnej za pomocą witryny Azure Portal. Po ukończeniu wdrożenia nawiążemy połączenie z serwerem i zainstalujemy usługi IIS.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

2. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

3. W bloku **Nowy** wybierz pozycję **Obliczeniowe**, w bloku **Obliczeniowe** wybierz pozycję **Windows Server 2016 Datacenter**, a następnie kliknij przycisk **Utwórz**.

4. Wypełnij formularz **Podstawy** maszyny wirtualnej. Nazwa użytkownika i hasło wprowadzone w tym miejscu są używane na potrzeby logowania się do maszyny wirtualnej. W obszarze **Grupa zasobów** utwórz nową. Grupa zasobów to logiczny kontener, w którym są tworzone i zbiorczo zarządzane zasoby platformy Azure. Po zakończeniu kliknij przycisk **OK**.

    ![Wprowadzanie podstawowych informacji o maszynie wirtualnej w bloku portalu](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

5. Wybierz rozmiar maszyny wirtualnej i kliknij przycisk **Wybierz**.

6. W bloku ustawień wybierz opcję **Tak** w obszarze **Używanie zarządzanych dysków**, zachowaj ustawienia domyślne dla pozostałych ustawień i kliknij przycisk **OK**.

7. Na stronie podsumowania kliknij przycisk **OK**, aby rozpocząć wdrażanie maszyny wirtualnej.

8. Aby monitorować stan wdrożenia, kliknij maszynę wirtualną. Maszynę wirtualną można znaleźć na pulpicie nawigacyjnym witryny Azure Portal. Można też wybrać pozycję **Maszyny wirtualne** w menu po lewej stronie. Po utworzeniu maszyny wirtualnej jej stan zmieni się z **Wdrażanie** na **Uruchomiona**.

## <a name="open-port-80-for-web-traffic"></a>Otwieranie portu 80 na potrzeby ruchu w sieci Web 

Aby zezwolić na ruch dla usług IIS, musisz otworzyć port 80 dla ruchu w sieci Web. Ten krok przeprowadzi Cię przez proces tworzenia reguły sieciowej grupy zabezpieczeń w celu zezwolenia na połączenia przychodzące w porcie 80.

1. W sekcji **Podstawowe elementy** bloku maszyny wirtualnej kliknij nazwę **grupy zasobów**.
2. W bloku grupy zasobów kliknij pozycję **Sieciowa grupa zabezpieczeń** na liście zasobów. Nazwa sieciowej grupy zabezpieczeń powinna być nazwą maszyny wirtualnej z dołączonym na końcu ciągiem -nsg.
3. Kliknij nagłówek **Reguła zabezpieczeń dla ruchu przychodzącego**, aby otworzyć listę reguł ruchu przychodzącego. Na liście powinna już znajdować się reguła protokołu RDP.
4. Kliknij pozycję **+ Dodaj**, aby otworzyć blok **Dodawanie reguły zabezpieczeń dla ruchu przychodzącego**.
5. W polu **Nazwa** wpisz ciąg **IIS** i upewnij się, że w polu **Zakres portów** ustawiono wartość 80, a w polu **Akcja** — wartość **Zezwalaj**, a następnie kliknij przycisk **OK**.


## <a name="connect-to-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

Po zakończeniu wdrożenia utwórz połączenie pulpitu zdalnego z maszyną wirtualną.

1. Kliknij przycisk **Połącz** w bloku maszyny wirtualnej. Zostanie utworzony i pobrany plik Remote Desktop Protocol (rdp).

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. Aby połączyć się z maszyną wirtualną, otwórz pobrany plik RDP. Jeśli zostanie wyświetlony monit, kliknij przycisk **Połącz**. Na komputerze Mac należy skorzystać z klienta RDP, takiego jak ten [klient pulpitu zdalnego](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) ze sklepu Mac App Store.

3. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej, a następnie kliknij przycisk **OK**.

4. Podczas logowania może się pojawić ostrzeżenie o certyfikacie. Kliknij przycisk **Tak** lub **Kontynuuj**, aby kontynuować nawiązywanie połączenia.


## <a name="install-iis-using-powershell"></a>Instalowanie usług IIS przy użyciu programu PowerShell

Teraz po zalogowaniu do maszyny wirtualnej platformy Azure możesz użyć jednego wiersza w programie PowerShell, aby zainstalować usługi IIS i włączyć lokalną regułę zapory, która zezwala na ruch w sieci Web.  Otwórz wiersz polecenia programu PowerShell i uruchom następujące polecenie:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Wyświetlanie strony powitalnej usług IIS

Po zainstalowaniu usług IIS i otwarciu portu 80 na maszynie wirtualnej z Internetu możesz użyć wybranej przeglądarki sieci Web, aby wyświetlić domyślną stronę powitalną przeglądarki usług IIS. Pobierz **publiczny adres IP** z bloku maszyny wirtualnej i użyj go w celu odwiedzenia domyślnej strony sieci Web. 

![Domyślna witryna usług IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="delete-virtual-machine"></a>Usuwanie maszyny wirtualnej

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć za pomocą następującego polecenia.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="delete-virtual-machine"></a>Usuwanie maszyny wirtualnej

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć. W tym celu wybierz grupę zasobów z bloku maszyny wirtualnej, a następnie kliknij przycisk **Usuń**.

## <a name="next-steps"></a>Następne kroki

[Samouczek dotyczący instalowania roli i konfigurowania zapory](hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Przegląd przykładowych poleceń interfejsu wiersza polecenia umożliwiających wdrożenie maszyny wirtualnej](cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

