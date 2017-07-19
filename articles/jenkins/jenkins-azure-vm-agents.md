---
title: "Używanie agentów maszyn wirtualnych platformy Azure w celu zapewnienia ciągłej integracji z usługą Jenkins."
description: "Agenty maszyn wirtualnych platformy Azure jako elementy podrzędne usługi Jenkins."
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: 5f2df414b4d0e8798b7ed6d90d0ea0fb79d42fc2
ms.contentlocale: pl-pl
ms.lasthandoff: 06/21/2017

---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Używanie agentów maszyn wirtualnych platformy Azure w celu zapewnienia ciągłej integracji z usługą Jenkins.

Ten przewodnik Szybki start opisuje, jak przy użyciu wtyczki agentów maszyn wirtualnych platformy Azure dla usługi Jenkins utworzyć agenta systemu Linux (Ubuntu) na platformie Azure na żądanie.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki start:

* Jeśli nie masz jeszcze wzorca usługi Jenkins, możesz na początek użyć [szablonu rozwiązania](install-jenkins-solution-template.md). 
* Jeśli nie masz jeszcze jednostki usługi Azure, zobacz artykuł [Tworzenie jednostki usługi platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json).

## <a name="install-azure-vm-agents-plugin"></a>Instalowanie wtyczki agentów maszyn wirtualnych platformy Azure

Jeśli używasz [szablonu rozwiązania](install-jenkins-solution-template.md), wtyczka agentów maszyn wirtualnych platformy Azure jest zainstalowana we wzorcu usługi Jenkins.

W innym przypadku zainstaluj wtyczkę **agentów maszyn wirtualnych platformy Azure** z poziomu pulpitu nawigacyjnego usługi Jenkins.

## <a name="configure-the-plugin"></a>Konfigurowanie wtyczki

* Na pulpicie nawigacyjnym usługi Jenkins kliknij **Manage Jenkins -> Configure System** (Zarządzanie usługą Jenkins -> Konfiguracja systemu). Przewiń do dołu strony i znajdź sekcję z listą rozwijaną **Add new cloud** (Dodaj nową chmurę). Wybierz z menu opcję **Microsoft Azure VM Agents** (Agenty maszyn wirtualnych platformy Microsoft Azure).
* Wybierz istniejące konto z listy rozwijanej Azure Credentials (Poświadczenia platformy Azure).  Aby dodać nową jednostkę usługi Microsoft Azure (**Microsoft Azure Service Principal**), wprowadź następujące wartości: Subscription ID (Identyfikator subskrypcji), Client ID (Identyfikator klienta), Client Secret (Klucz tajny klienta) i OAuth 2.0 Token Endpoint (Punkt końcowy tokenu OAuth 2.0).

![Poświadczenia platformy Azure](./media/jenkins-azure-vm-agents/service-principal.png)

* Kliknij przycisk **Verify configuration** (Sprawdź konfigurację), aby upewnić się, że konfiguracja profilu jest prawidłowa.
* Zapisz konfigurację i przejdź do następnego kroku.

## <a name="template-configuration"></a>Konfigurowanie szablonu

### <a name="general-configuration"></a>Konfiguracja ogólna
Następnie skonfiguruj szablon używany do definiowania agenta maszyny wirtualnej platformy Azure. 

* Kliknij przycisk **Add** (Dodaj), aby dodać szablon. 
* Wpisz nazwę nowego szablonu. 
* Jako etykietę wprowadź wartość „ubuntu”. Ta etykieta jest używana podczas konfigurowania zadania.
* Wybierz region z pola kombi.
* Wybierz rozmiar maszyny wirtualnej.
* Określ nazwę konta Azure Storage lub pozostaw to pole puste, aby użyć domyślnej nazwy „jenkinsarmst”.
* Określ czas przechowywania w minutach. To ustawienie określa liczbę minut, po jakiej usługa Jenkins automatycznie usunie bezczynnego agenta. Wpisz 0, jeśli nie chcesz, aby bezczynne agenty były automatycznie usuwane.

![Konfiguracja ogólna](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Konfiguracja obrazu

Aby utworzyć agenta systemu Linux (Ubuntu), wybierz **Image reference** (Obraz odniesienia) i użyj poniższej konfiguracji jako przykładu. Najnowsze obrazy obsługiwane przez platformę Azure znajdziesz w portalu [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1).

* Image Publisher: Canonical
* Image Offer: UbuntuServer
* Image Sku: 14.04.5-LTS
* Image version: latest
* OS Type: Linux
* Launch method: SSH
* Wprowadź poświadczenia administratora
* W przypadku skryptu inicjowania maszyny wirtualnej wpisz:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Konfiguracja obrazu](./media/jenkins-azure-vm-agents/image-config.png)

* Kliknij przycisk **Verify template** (Sprawdź szablon), aby sprawdzić konfigurację.
* Kliknij przycisk **Save** (Zapisz).

## <a name="create-a-job-in-jenkins"></a>Tworzenie zadania w usłudze Jenkins

* Na pulpicie nawigacyjnym usługi Jenkins kliknij **New Item** (Nowy element). 
* Wprowadź nazwę, wybierz **Freestyle project** (Projekt Freestyle) i kliknij przycisk **OK**.
* Na karcie **General** (Ogólne) wybierz opcję „Restrict where project can be run” (Ogranicz miejsca uruchomienia projektu) i wpisz „ubuntu” w polu Label Expression (Wyrażenie etykiety). Na liście rozwijanej pojawi się pozycja „ubuntu”.
* Kliknij przycisk **Save** (Zapisz).

![Konfigurowanie zadania](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Tworzenie nowego projektu

* Wróć do pulpitu nawigacyjnego usługi Jenkins.
* Kliknij prawym przyciskiem myszy nowo utworzone zadanie, a następnie kliknij przycisk **Build now** (Skompiluj teraz). Rozpocznie się kompilowanie. 
* Po ukończeniu kompilacji przejdź do obszaru **Console output** (Dane wyjściowe konsoli). Będzie widać, że kompilacja została wykonana zdalnie na platformie Azure.

![Dane wyjściowe konsoli](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Dokumentacja

* Wideo z cyklu „Piątek z Azure”: [Ciągła integracja z usługą Jenkins przy użyciu agentów maszyn wirtualnych platformy Azure](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Informacje uzupełniające i opcje konfiguracji: [Wiki wtyczki agentów maszyn wirtualnych platformy Azure dla usługi Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 


