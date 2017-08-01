---
title: "Tworzenie pierwszego wzorca usługi Jenkins na maszynie wirtualnej z systemem Linux (Ubuntu) na platformie Azure"
description: "Skorzystaj z szablonu rozwiązania umożliwiającego wdrożenie usługi Jenkins."
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: multiple
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 748f1994d0ee5b6c6a988bca8e5b694b29c0b8c5
ms.contentlocale: pl-pl
ms.lasthandoff: 06/30/2017

---

# <a name="create-your-first-jenkins-master-on-a-linux-ubuntu-vm-on-azure"></a>Tworzenie pierwszego wzorca usługi Jenkins na maszynie wirtualnej z systemem Linux (Ubuntu) na platformie Azure

W tym przewodniku Szybki start pokazano, jak zainstalować najnowszą stabilną wersję usługi Jenkins razem z narzędziami i wtyczkami skonfigurowanymi do współdziałania z platformą Azure na maszynie wirtualnej z systemem Linux (Ubuntu 14.04 LTS). Narzędzia te zawierają następujące składniki:
<ul>
<li>Narzędzie Git do kontroli źródła</li>
<li>Wtyczka do obsługi poświadczeń do platformy Azure umożliwiająca nawiązywanie bezpiecznych połączeń</li>
<li>Wtyczka agentów maszyn wirtualnych platformy Azure umożliwiająca kompilację elastyczną, przeprowadzanie testów oraz ciągłą integrację</li>
<li>Wtyczka usługi Microsoft Azure Storage do przechowywania artefaktów</li>
<li>Interfejs wiersza polecenia platformy Azure do wdrażania aplikacji za pomocą skryptów</li>
</ul>

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie bezpłatnego konta platformy Azure.
> * Tworzenie wzorca usługi Jenkins na maszynie wirtualnej platformy Azure przy użyciu szablonu rozwiązania. 
> * Przeprowadzanie początkowej konfiguracji usługi Jenkins.
> * Instalowanie sugerowanych wtyczek.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-the-vm-in-azure-by-deploying-the-solution-template-for-jenkins"></a>Tworzenie maszyny wirtualnej na platformie Azure przez wdrożenie szablonu rozwiązania dla usługi Jenkins

Szablony Szybki Start platformy Azure umożliwiają szybkie i niezawodne wdrażanie złożonych technologii na platformie Azure.  Usługa Azure Resource Manager pozwala aprowizować aplikacje za pomocą [deklaratywnych szablonów](https://azure.microsoft.com/en-us/resources/templates/?term=jenkins). Pojedynczy szablon umożliwia wdrożenie wielu usług wraz z ich zależnościami. Za pomocą tego samego szablonu możesz wdrażać aplikację na każdym etapie jej cyklu życia.

Aby zapoznać się z opcjami dotyczącymi opłat za korzystanie z tego szablonu, zobacz informacje o [planach i cenniku](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.jenkins?tab=PlansAndPrice).

Przejdź do strony [z obrazem usługi Jenkins w portalu Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview) i kliknij przycisk **POBIERZ TERAZ**.  

W witrynie Azure Portal kliknij pozycję **Utwórz**.  Szablon ten wymaga użycia usługi Resource Manager, dlatego lista rozwijana modelu szablonu jest wyłączona.
   
![Okno dialogowe witryny Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

Na karcie **Konfigurowanie ustawień podstawowych**:

![Konfigurowanie ustawień podstawowych](./media/install-jenkins-solution-template/ap-basic.png)

* Podaj nazwę wystąpienia usługi Jenkins.
* Wybierz typ dysku maszyny wirtualnej.  W przypadku obciążeń produkcyjnych wybierz większy rozmiar maszyny wirtualnej. Wybranie dysku SSD zapewnia lepszą wydajność.  Więcej informacji na temat typów dysków platformy Azure jest dostępnych [tutaj](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage).
* Nazwa użytkownika: musi spełniać wymagania dotyczące długości i nie może zawierać wyrazów zastrzeżonych ani nieobsługiwanych znaków. Takie nazwy, jak „admin”, są niedozwolone.  Więcej informacji o wymaganiach dotyczących nazwy użytkownika i hasła jest dostępnych [tutaj](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq).
* Typ uwierzytelniania: utwórz wystąpienie zabezpieczone hasłem lub [kluczem publicznym SSH](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows). Używane hasło musi zawierać trzy z następujących znaków: mała litera, wielka litera, cyfra i znak specjalny.
* Pozostaw wartość **LTS** jako typ wersji usługi Jenkins.
* Wybierz subskrypcję.
* Utwórz grupę zasobów lub wybierz istniejącą.
* Wybierz lokalizację.

Na karcie **Konfigurowanie opcji dodatkowych**:

![Konfigurowanie opcji dodatkowych](./media/install-jenkins-solution-template/ap-addtional.png)

* Podaj etykietę nazwy domeny, która jednoznacznie identyfikuje wzorzec usługi Jenkins.

Kliknij przycisk **OK**, aby przejść do następnego kroku. 

Po pomyślnej weryfikacji kliknij przycisk **OK**, aby pobrać szablon i parametry. 

Następnie wybierz pozycję **Kup**, aby umożliwić aprowizację wszystkich zasobów.

## <a name="setup-ssh-port-forwarding"></a>Konfigurowanie przekierowania portów SSH

Domyślnie wystąpienie usługi Jenkins używa protokołu http i nasłuchuje na porcie 8080. Użytkownicy nie powinni przeprowadzać uwierzytelniania za pośrednictwem niezabezpieczonych protokołów.
    
Skonfigurowanie przekierowania portów umożliwia wyświetlenie interfejsu użytkownika usługi Jenkins na maszynie lokalnej.

### <a name="if-you-are-using-windows"></a>Jeśli używasz systemu Windows:

Zainstaluj program PuTTY i uruchom to polecenie, jeśli do zabezpieczenia usługi Jenkins jest używane hasło:
```
putty.exe -ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Aby się zalogować, wprowadź hasło.

![Aby się zalogować, wprowadź hasło.](./media/install-jenkins-solution-template/jenkins-pwd.png)

Jeśli używasz protokołu SSH, uruchom następujące polecenie:
```
putty -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

### <a name="if-you-are-using-linux-or-mac"></a>Jeśli używasz systemu Linux lub komputera Mac:

Jeśli do zabezpieczenia wzorca usługi Jenkins jest używane hasło, uruchom następujące polecenie:
```
ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Aby się zalogować, wprowadź hasło.

Jeśli używasz protokołu SSH, uruchom następujące polecenie:
```
ssh -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

## <a name="connect-to-jenkins"></a>Nawiązywanie połączenia z usługą Jenkins
Po uruchomieniu tunelu przejdź do adresu http://localhost:8080/ na maszynie lokalnej.

Odblokuj pulpit nawigacyjny usługi Jenkins za pomocą początkowego hasła administratora.

![Odblokowywanie usługi Jenkins](./media/install-jenkins-solution-template/jenkins-unlock.png)

Aby uzyskać token, nawiąż połączenie SSH z maszyną wirtualną i uruchom polecenie `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.

![Odblokowywanie usługi Jenkins](./media/install-jenkins-solution-template/jenkins-ssh.png)

Pojawi się monit o zainstalowanie sugerowanych wtyczek.

Następnie utwórz konto administratora dla wzorca usługi Jenkins.

Wystąpienie usługi Jenkins jest teraz gotowe do użycia. Widok tylko do odczytu jest dostępny pod adresem http://\<publiczna nazwa DNS utworzonego wystąpienia\>.

![Usługa Jenkins jest gotowa do użycia.](./media/install-jenkins-solution-template/jenkins-welcome.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Utworzenie wzorca usługi Jenkins za pomocą szablonu rozwiązania.
> * Przeprowadzenie początkowej konfiguracji usługi Jenkins.
> * Zainstalowanie wtyczek.

Użyj poniższego linku, aby dowiedzieć się, jak używać agentów maszyn wirtualnych platformy Azure w celu zapewnienia ciągłej integracji z usługą Jenkins.

> [!div class="nextstepaction"]
> [Azure VMs as Jenkins agents (Maszyny wirtualne platformy Azure jako agenci usługi Jenkins)](jenkins-azure-vm-agents.md)

