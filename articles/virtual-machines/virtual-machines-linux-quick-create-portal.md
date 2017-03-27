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
ms.date: 03/21/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: bcfd830a5e2f39f36460990cae7e84b04d9a5fbb
ms.lasthandoff: 03/21/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-portal"></a>Tworzenie maszyny wirtualnej z systemem Linux za pomocą witryny Azure Portal

Maszyny wirtualne platformy Azure można utworzyć za pomocą witryny Azure Portal. Ta metoda bazuje na opartym na przeglądarce interfejsie użytkownika umożliwiającym tworzenie i konfigurowanie maszyn wirtualnych oraz wszystkich pokrewnych zasobów. Ten przewodnik Szybki start przeprowadza użytkownika przez proces tworzenia maszyny wirtualnej za pomocą witryny Azure Portal. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="create-ssh-key-pair"></a>Tworzenie pary kluczy SSH

Do wykonania kroków tego przewodnika Szybki start konieczne jest posiadanie pary kluczy SSH. Jeśli masz już parę kluczy SSH, możesz pominąć ten krok. Jeśli korzystasz z komputera z systemem Windows, postępuj zgodnie z instrukcjami przedstawionymi [tutaj](./virtual-machines-linux-ssh-from-windows.md). 

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

    ![Wprowadzanie podstawowych informacji o maszynie wirtualnej w bloku portalu](./media/virtual-machine-quick-start/create-vm-portal-basic-blade.png)  

4. Wybierz rozmiar maszyny wirtualnej i kliknij przycisk **Wybierz**. 

    ![Wybieranie rozmiaru maszyny wirtualnej w bloku portalu](./media/virtual-machine-quick-start/create-vm-portal-size-blade.png)

5. W bloku ustawień wybierz opcję **Tak** w obszarze **Używanie zarządzanych dysków**, zachowaj ustawienia domyślne dla pozostałych ustawień i kliknij przycisk **OK**.

6. Na stronie podsumowania kliknij przycisk **OK**, aby rozpocząć wdrażanie maszyny wirtualnej.

7. Aby monitorować stan wdrożenia, kliknij maszynę wirtualną. Maszynę wirtualną można znaleźć na pulpicie nawigacyjnym witryny Azure Portal. Można też wybrać pozycję **Maszyny wirtualne** w menu po lewej stronie. Po utworzeniu maszyny wirtualnej jej stan zmieni się z **Wdrażanie** na **Uruchomiona**.

## <a name="connect-to-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

Po zakończeniu wdrożenia utwórz połączenie SSH z maszyną wirtualną.

1. Kliknij przycisk **Połącz** w bloku maszyny wirtualnej. Na przycisku połączenia są wyświetlane parametry połączenia SSH, których można użyć do nawiązania połączenia z maszyną wirtualną.

    ![Portal 9](./media/virtual-machine-quick-start/portal-quick-start-9.png) 

2. Uruchom następujące polecenie, aby utworzyć sesję SSH. Zastąp parametry połączenia wartościami skopiowanymi z witryny Azure Portal.

```bash 
ssh <replace with IP address>
```
## <a name="delete-virtual-machine"></a>Usuwanie maszyny wirtualnej

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć. W tym celu wybierz grupę zasobów z bloku maszyny wirtualnej, a następnie kliknij przycisk **Usuń**.

## <a name="next-steps"></a>Następne kroki

[Samouczek dotyczący tworzenia maszyn wirtualnych o wysokiej dostępności](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Przegląd przykładowych poleceń interfejsu wiersza polecenia umożliwiających wdrożenie maszyny wirtualnej](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

