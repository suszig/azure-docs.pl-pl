---
title: "Azure: Szybki start — Tworzenie maszyn wirtualnych za pomocą portalu | Microsoft Docs"
description: "Azure: Szybki start — Tworzenie maszyn wirtualnych za pomocą portalu"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/13/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: ab29f01980bc7c3a8f12aaa55ff35baa3bf3f9fb
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Tworzenie maszyny wirtualnej z systemem Linux za pomocą witryny Azure Portal

Maszyny wirtualne platformy Azure można utworzyć za pomocą witryny Azure Portal. Ta metoda bazuje na opartym na przeglądarce interfejsie użytkownika umożliwiającym tworzenie i konfigurowanie maszyn wirtualnych oraz wszystkich pokrewnych zasobów. Ten przewodnik Szybki start przeprowadza użytkownika przez proces tworzenia maszyny wirtualnej za pomocą witryny Azure Portal.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F).

## <a name="create-ssh-key-pair"></a>Tworzenie pary kluczy SSH

Do wykonania kroków tego przewodnika Szybki start konieczne jest posiadanie pary kluczy SSH. Jeśli masz już parę kluczy SSH, możesz pominąć ten krok. Jeśli korzystasz z komputera z systemem Windows, postępuj zgodnie z instrukcjami przedstawionymi [tutaj](ssh-from-windows.md). 

Korzystając z powłoki Bash, uruchom to polecenie i wykonaj instrukcje wyświetlane na ekranie. Dane wyjściowe polecenia zawierają nazwę pliku klucza publicznego. Zawartość tego pliku jest wymagana podczas tworzenia maszyny wirtualnej.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. W bloku **Nowy** wybierz pozycję **Obliczeniowe**, w bloku **Obliczeniowe** wybierz pozycję **Ubuntu Server 16.04 LTS**, a następnie kliknij przycisk **Utwórz**.

3. Wypełnij formularz **Podstawy** maszyny wirtualnej. W obszarze **Typ uwierzytelniania** wybierz pozycję **SSH**. Podczas wklejania wartości w polu **Klucz publiczny SSH** pamiętaj, aby usunąć wszystkie wiodące i końcowe białe znaki. W obszarze **Grupa zasobów** utwórz nową. Grupa zasobów to logiczny kontener, w którym są tworzone i zbiorczo zarządzane zasoby platformy Azure. Po zakończeniu kliknij przycisk **OK**.

    ![Wprowadzanie podstawowych informacji o maszynie wirtualnej w bloku portalu](./media/quick-create-portal/create-vm-portal-basic-blade.png)  

4. Wybierz rozmiar maszyny wirtualnej. Aby wyświetlić więcej rozmiarów, wybierz pozycje **Wyświetl wszystkie** lub zmień filtr **Obsługiwany typ dysku**. 

    ![Zrzut ekranu przedstawiający rozmiary maszyn wirtualnych](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. W bloku ustawień wybierz opcję **Tak** w obszarze **Używanie zarządzanych dysków**, zachowaj ustawienia domyślne dla pozostałych ustawień i kliknij przycisk **OK**.

6. Na stronie podsumowania kliknij przycisk **OK**, aby rozpocząć wdrażanie maszyny wirtualnej.

7. Aby monitorować stan wdrożenia, kliknij maszynę wirtualną. Maszynę wirtualną można znaleźć na pulpicie nawigacyjnym witryny Azure Portal. Można też wybrać pozycję **Maszyny wirtualne** w menu po lewej stronie. Po utworzeniu maszyny wirtualnej jej stan zmieni się z **Wdrażanie** na **Uruchomiona**.


## <a name="open-port-80-for-web-traffic"></a>Otwieranie portu 80 na potrzeby ruchu w sieci Web 

Domyślnie dozwolone są tylko połączenia SSH z maszynami wirtualnymi z systemem Linux wdrożonymi na platformie Azure. Jeśli ta maszyna wirtualna ma być serwerem sieci Web, należy otworzyć port 80 na potrzeby ruchu sieciowego. Ten krok przeprowadzi Cię przez proces tworzenia reguły sieciowej grupy zabezpieczeń w celu zezwolenia na połączenia przychodzące w porcie 80.

1. W sekcji **Podstawowe elementy** bloku maszyny wirtualnej kliknij nazwę **grupy zasobów**.
2. W bloku grupy zasobów kliknij pozycję **Sieciowa grupa zabezpieczeń** na liście zasobów. Nazwa sieciowej grupy zabezpieczeń powinna być nazwą maszyny wirtualnej z dołączonym na końcu ciągiem -nsg.
3. Kliknij nagłówek **Reguła zabezpieczeń dla ruchu przychodzącego**, aby otworzyć listę reguł ruchu przychodzącego. Na liście powinna już znajdować się reguła protokołu RDP.
4. Kliknij pozycję **+ Dodaj**, aby otworzyć blok **Dodawanie reguły zabezpieczeń dla ruchu przychodzącego**.
5. W polu **Nazwa** wpisz ciąg **nginx**. Upewnij się, że w polu **Zakres portów** ustawiono wartość 80, a w polu **Akcja** — wartość **Zezwalaj**. Kliknij przycisk **OK**.


## <a name="connect-to-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

Po zakończeniu wdrożenia utwórz połączenie SSH z maszyną wirtualną.

1. Kliknij przycisk **Połącz** w bloku maszyny wirtualnej. Na przycisku połączenia są wyświetlane parametry połączenia SSH, których można użyć do nawiązania połączenia z maszyną wirtualną.

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Uruchom następujące polecenie, aby utworzyć sesję SSH. Zastąp parametry połączenia wartościami skopiowanymi z witryny Azure Portal.

```bash 
ssh <replace with IP address>
```

## <a name="install-nginx"></a>Instalowanie serwera NGINX

Użyj poniższego skryptu powłoki systemowej w celu zaktualizowania źródeł pakietów i zainstalowania najnowszego pakietu NGINX. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>Wyświetlanie strony powitalnej serwera NGINX

Po zainstalowaniu serwera NGINX i otwarciu portu 80 na maszynie wirtualnej z Internetu możesz użyć wybranej przeglądarki sieci Web, aby wyświetlić domyślną stronę powitalną przeglądarki serwera NGINX. Upewnij się, że w celu odwiedzenia strony domyślnej używasz udokumentowanego adresu `publicIpAddress`. 

![Domyślna witryna serwera NGINX](./media/quick-create-cli/nginx.png) 
## <a name="delete-virtual-machine"></a>Usuwanie maszyny wirtualnej

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć. W tym celu wybierz grupę zasobów z bloku maszyny wirtualnej, a następnie kliknij przycisk **Usuń**.

## <a name="next-steps"></a>Następne kroki

[Samouczek dotyczący tworzenia maszyn wirtualnych o wysokiej dostępności](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Przegląd przykładowych poleceń interfejsu wiersza polecenia umożliwiających wdrożenie maszyny wirtualnej](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

