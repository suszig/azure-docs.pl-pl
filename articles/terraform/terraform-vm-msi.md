---
title: "Użyj obrazu portalu Azure Marketplace do utworzenia maszyny wirtualnej systemu Linux Terraform zarządzane tożsamości usługi"
description: "Aby utworzyć maszyny wirtualnej systemu Linux Terraform z zarządzanych tożsamość usługi i stan zdalnego zarządzania łatwe wdrażanie zasobów na platformie Azure, Użyj obrazu z witryny Marketplace."
keywords: terraform, devops, MSI, virtual machine, remote state, azure
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: ce8a3e8b813bf4224a1fd77d60ec30f2f8e080a7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Użyj obrazu portalu Azure Marketplace do utworzenia maszyny wirtualnej systemu Linux Terraform zarządzane tożsamości usługi

W tym artykule przedstawiono sposób użycia [obrazu z witryny Terraform Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) utworzyć `Ubuntu Linux VM (16.04 LTS)` z najnowszej [Terraform](https://www.terraform.io/intro/index.html) wersji zainstalowany i skonfigurowany za pomocą [(zarządzane tożsamości usługi MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Ten obraz konfiguruje również zdalnego zaplecza, aby włączyć [stan zdalnej](https://www.terraform.io/docs/state/remote.html) zarządzania przy użyciu Terraform. Obrazu witryny Terraform Marketplace ułatwia rozpoczęcie pracy z Terraform na platformie Azure (w minutach), bez konieczności instalowania Terraform i ręcznie skonfigurować uwierzytelnianie. 

Nie ma żadnych opłat oprogramowania dla tego obrazu Terraform maszyny Wirtualnej. Płaci się tylko opłaty użycia Azure sprzętu, które są oceniane na podstawie rozmiaru maszyny wirtualnej, udostępnione. Więcej informacji na temat opłat obliczeniowe można znaleźć w [cennik maszyn wirtualnych systemu Linux strony](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Wymagania wstępne
Przed utworzeniem maszyny wirtualnej systemu Linux Terraform musi mieć subskrypcję platformy Azure. Jeśli nie masz już jeden, zobacz [utworzyć bezpłatne konto platformy Azure obecnie](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Tworzenie maszyny wirtualnej Terraform 

Poniżej przedstawiono kroki, aby utworzyć wystąpienie maszyny wirtualnej Terraform systemu Linux. 

1. Przejdź do [Utwórz zasób](https://ms.portal.azure.com/#create/hub) wyświetlania w portalu Azure.
2. Wyszukaj `Terraform` w `Search the Marketplace` pasek wyszukiwania. Wybierz `Terraform` szablonu. Wybierz **Utwórz** przycisku na karcie Szczegóły Terraform w prawej dolnej.
![tekst alternatywny](media\terraformmsi.png)
3. Poniższe sekcje zawierają dane wejściowe dla poszczególne kroki w Kreatorze (**wyliczyć po prawej stronie**) do utworzenia maszyny wirtualnej systemu Linux Terraform.  W tym miejscu są potrzebne do skonfigurowania każdego z tych kroków dane wejściowe

## <a name="details-in-create-terraform-tab"></a>Szczegóły w utworzenia Terraform karty

Poniżej przedstawiono szczegółowe informacje, które należy wprowadzić na karcie Terraform utworzyć.

a. **Podstawy**
    
* **Nazwa**: Nazwa Terraform maszyny wirtualnej.
* **Nazwa użytkownika**: pierwsze konto logowania identyfikatora.
* **Hasło**: pierwszy hasło do konta (zamiast hasła można użyć klucza publicznego SSH).
* **Subskrypcja**: Jeśli masz więcej niż jedną subskrypcję, wybierz jedną, na którym maszyna ma być utworzony i są rozliczane. Musi mieć uprawnienia do tworzenia zasobów dla tej subskrypcji.
* **Grupy zasobów**: można utworzyć nowej lub istniejącej grupy.
* **Lokalizacja**: wybierz centrum danych, która jest najbardziej odpowiednia. Zazwyczaj jest centrum danych, która zawiera większość danych lub zbliżony do fizycznej lokalizacji najszybszy dostęp do sieci.

b. **Dodatkowe ustawienia**

* : Rozmiar pamięci maszyny wirtualnej.
* Typ dysku maszyny Wirtualnej: wybór między SSD i HDD

c. **Podsumowanie Terraform**

* Sprawdź, czy wszystkie wprowadzone informacje jest poprawna. 

d. **Buy**

* Aby rozpocząć, udostępnianie, kliknij dostawcy. Łącze znajduje się na warunki transakcji. Maszyna wirtualna nie ma żadnych dodatkowych kosztów poza obliczeń rozmiarze serwera, który wybrano w kroku rozmiar.

Obraz maszyny Wirtualnej Terraform wykonuje następujące czynności

* Tworzy Maszynę wirtualną z systemu przypisane tożsamości na podstawie obrazu Ubuntu 16.04 LTS
* Instaluje rozszerzenie MSI na maszynie Wirtualnej, aby umożliwić tokenów OAuth zostanie wysłane do zasobów platformy Azure
* Przypisz uprawnienia RBAC tożsamości zarządzanych przyznanie praw właściciela grupy zasobów
* Tworzy folder szablonu Terraform (tfTemplate)
* Wstępnie konfiguruje stan zdalnego Terraform z wewnętrznej bazy danych Azure

## <a name="how-to-access-and-configure-linux-terraform-virtual-machine"></a>Uzyskiwanie dostępu i konfigurowanie maszyny wirtualnej systemu Linux Terraform

Po utworzeniu maszyny Wirtualnej można logowania się do go przy użyciu protokołu SSH. Korzystać z poświadczeń konta, które zostały utworzone w sekcji podstawy krok 3 dla interfejsu powłoki tekstu. W systemie Windows, możesz pobrać narzędzie klienta SSH, takie jak [programu Putty](http://www.putty.org/)

Po zastosowaniu `SSH` Aby podłączyć się do maszyny wirtualnej, należy nadać uprawnienia współautora dla całej subskrypcji dla zarządzanych tożsamości usługi na maszynie wirtualnej. Uprawnienia współautora pomaga MSI w maszynie Wirtualnej, aby Terraform umożliwiają utworzenie zasobów spoza grupy zasobów maszyny Wirtualnej. Ta akcja może łatwo osiągnąć za raz wykonywania skryptu. Oto polecenia, aby wykonać to zadanie.

`. ~/tfEnv.sh`

Poprzedni skrypt używa [AZ CLI v 2.0 interakcyjnego logowania](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) mechanizm uwierzytelniania w usłudze Azure i przypisać tożsamość usługi zarządzania maszyny wirtualnej uprawnienia współautora dla całej subskrypcji. 

 Maszyna wirtualna ma stan zdalnej Terraform wewnętrznej bazy danych utworzone i aby ją włączyć w danym wdrożeniu Terraform, należy skopiować plik remoteState.tf z katalogu tfTemplate do katalogu głównego skrypty Terraform.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Możesz przeczytać więcej na temat zdalnego zarządzania stanem [tutaj](https://www.terraform.io/docs/state/remote.html). Klucz dostępu do magazynu jest widoczne w tym pliku i musi zostać dokładnie sprawdzony pod kontrolą źródła.  

## <a name="next-steps"></a>Następne kroki
W tym artykule przedstawiono sposób ustawić się Terraform maszyny wirtualnej systemu Linux na platformie Azure. Poniżej przedstawiono dodatkowe zasoby, aby dowiedzieć się więcej o Terraform na platformie Azure. 

 [Koncentrator Terraform w domenie Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Dokumentacji dostawcy Terraform platformy Azure](http://aka.ms/terraform)  
 [Źródłowy dostawca Terraform Azure](http://aka.ms/tfgit)  
 [Moduły Terraform Azure](http://aka.ms/tfmodules)
 

















