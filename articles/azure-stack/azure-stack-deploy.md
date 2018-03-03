---
title: "Wymagania wstępne wdrożenia stosu Development Kit platformy Azure | Dokumentacja firmy Microsoft"
description: "Wyświetl wymagania środowiska i sprzętowe dla Azure stosu Development Kit (operatorowi chmury)."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 32a21d9b-ee42-417d-8e54-98a7f90f7311
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/08/2017
ms.author: jeffgilb
ms.openlocfilehash: 2410b1a3c3c2856cceda87ced7e66f140ea0c323
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="azure-stack-deployment-prerequisites"></a>Wymagania wstępne dotyczące wdrożenia usługi Azure Stack

*Dotyczy: Azure stosu Development Kit*

Przed wdrożeniem [Azure stosu Development Kit](azure-stack-poc.md), upewnij się, że komputer spełnia następujące wymagania:


## <a name="hardware"></a>Sprzęt
| Składnik | Minimalne | Zalecane |
| --- | --- | --- |
| Stacje dysków: system operacyjny |1 dysk systemu operacyjnego z co najmniej 200 GB miejsca dostępnego dla partycji systemowej (SSD lub HDD) |1 dysk systemu operacyjnego z co najmniej 200 GB miejsca dostępnego dla partycji systemowej (SSD lub HDD) |
| Stacje dysków: Programowanie ogólne zestawu danych * |4 dyski. Każdy dysk udostępnia co najmniej 140 GB pojemności (SSD lub HDD). Wszystkie dostępne dyski będą używane. |4 dyski. Każdy dysk udostępnia co najmniej 250 GB pojemności (SSD lub HDD). Wszystkie dostępne dyski będą używane. |
| Obliczenia: procesor CPU |Dwa gniazda: 12 rdzeni fizycznych (łącznie) |Dwa gniazda: 16 rdzeni fizycznych (łącznie) |
| Obliczenia: pamięć |96 GB pamięci RAM |128 GB pamięci RAM (jest to co najmniej do obsługi dostawców zasobów PaaS).|
| Obliczenia: system BIOS |Włączona funkcja Hyper-V (z obsługą usługi SLAT) |Włączona funkcja Hyper-V (z obsługą usługi SLAT) |
| Sieć: karta sieciowa |Wymagany certyfikat systemu Windows Server 2012 R2 dla karty sieciowej; żadne specjalne funkcje nie są wymagane |Wymagany certyfikat systemu Windows Server 2012 R2 dla karty sieciowej; żadne specjalne funkcje nie są wymagane |
| Certyfikacja logo sprzętu |[Certyfikowane dla systemu Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certyfikowane dla systemu Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

\*Konieczne będzie większa niż zalecana pojemność, jeśli planowane jest dodanie wiele [elementów marketplace](azure-stack-download-azure-marketplace-item.md) z platformy Azure.

**Konfiguracja dysku danych:** wszystkich dysków z danymi musi być tego samego typu (wszystkie skojarzenia zabezpieczeń, wszystkie SATA lub wszystkich NVMe) i pojemności. Jeśli używane są dyski SAS, stacje dysków muszą być dołączone za pomocą pojedynczej ścieżki (nie jest zapewniana obsługa funkcji MPIO, obsługa wielościeżkowa).

**Opcje konfiguracji karty HBA**

* (Preferowane) Prosta karta HBA
* Karta HBA z technologią RAID — karta musi być skonfigurowana w trybie przekazywania
* Karta HBA z technologią RAID — dyski należy skonfigurować jako pojedynczy dysk, RAID-0

**Obsługiwane magistrali i nośniki wpisz kombinacji**

* HDD SATA
* HDD SAS
* HDD RAID
* SSD RAID (jeśli typ nośnika jest nieokreślony/nieznany\*)
* SSD SATA + HDD SATA
* SSD SAS + HDD SAS
* NVMe

\* Kontrolerów RAID bez możliwości przekazujący nie można rozpoznać typu nośnika. Takie kontrolery oznaczą zarówno dyski HDD, jak i dyski SSD jako nieokreślone. W takim przypadku dysk SSD będzie używany jako magazyn trwały zamiast buforowania urządzeń. W związku z tym wdrożeniem development kit na tych dyskach SSD.

**Przykładowe karty HBA**: LSI 9207-8i, LSI-9300-8i lub LSI-9265-8i w trybie przekazywania

Dostępne są przykładowe konfiguracje OEM.

## <a name="operating-system"></a>System operacyjny
|  | **Wymagania** |
| --- | --- |
| **Wersja systemu operacyjnego** |Windows Server 2012 R2 lub nowszym. Wersja systemu operacyjnego nie są krytyczne przed rozpoczęciem wdrażania, jak do wirtualnego dysku twardego, który znajduje się w instalacji stosu Azure będzie rozruchu komputera hosta. System operacyjny i wszystkie wymagane poprawki są już zintegrowane z obrazu. Nie używaj klawiszy, aby aktywować wystąpienia systemu Windows Server, używany w zestawie. |

## <a name="deployment-requirements-check-tool"></a>Narzędzie sprawdzić wymagania dotyczące wdrażania
Po zainstalowaniu systemu operacyjnego, można użyć [wdrożenia sprawdzanie stosu Azure](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) aby upewnić się, że sprzęt spełnia wszystkie wymagania.

## <a name="account-requirements"></a>Wymagania dotyczące konta
Zazwyczaj z połączeniem internetowym, w którym można podłączyć do systemu Microsoft Azure jest wdrażany z zestawem deweloperskim. W takim przypadku należy skonfigurować konto usługi Azure Active Directory (Azure AD) do wdrożenia z zestawem deweloperskim.

Jeśli środowisko nie jest połączony z Internetem lub nie chcesz używać usługi Azure AD, Azure stosu można wdrożyć przy użyciu usługi Active Directory Federation Services (AD FS). Zestaw deweloperski obejmuje własnego wystąpienia usług AD FS i usług domenowych w usłudze Active Directory. W przypadku wdrożenia przy użyciu tej opcji, nie trzeba skonfigurować konta wcześniejsze.

>[!NOTE]
W przypadku wdrożenia przy użyciu opcji usług AD FS, należy ponownie wdrożyć stosu Azure, aby przełączyć się do usługi Azure AD.

### <a name="azure-active-directory-accounts"></a>Azure kont usługi Active Directory
Aby wdrożyć stosu Azure przy użyciu konta usługi Azure AD, należy przygotować konta usługi Azure AD przed uruchomieniem wdrożenia skryptu programu PowerShell. To konto staje się administratorem globalnym dla dzierżawy usługi Azure AD. Służy do obsługi administracyjnej, a następnie delegatem aplikacji i nazwy główne usług dla wszystkich usług Azure stosu współpracujące z usługą Azure Active Directory i interfejsu API programu Graph. Służy również jako właściciela subskrypcji dostawcy domyślnego (które można później zmienić). Możesz zalogować się do portalu administratora systemu Azure stosu przy użyciu tego konta.

1. Utwórz konto usługi Azure AD, który jest administratorem katalogu dla co najmniej jednej usługi Azure AD. Jeśli masz już takie konto, możesz go użyć. W przeciwnym razie możesz utworzyć je bezpłatnie w [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (w Chinach, odwiedź stronę <http://go.microsoft.com/fwlink/?LinkID=717821> zamiast niego). Jeśli zamierzasz później [zarejestrować stosu Azure w usłudze Azure](azure-stack-register.md), musi również mieć subskrypcji w tym nowo utworzone konto.
   
    Zapisz te poświadczenia do użycia w kroku 6 [wdrażanie zestaw deweloperski](azure-stack-run-powershell-script.md). Za pomocą tego konta *administratora usługi* można skonfigurować zasoby chmury, konta użytkowników, plany dzierżawy, limity przydziału oraz ceny i zarządzać nimi. W portalu umożliwia ono tworzenie chmur witryn sieci Web, chmur prywatnych maszyn wirtualnych, tworzenie planów i zarządzanie subskrypcjami użytkowników.
2. [Utwórz](azure-stack-add-new-user-aad.md) co najmniej jedno konto, aby móc zalogować się zestaw deweloperski dzierżawcy w.
   
   | **Konto usługi Azure Active Directory** | **Obsługiwane?** |
   | --- | --- |
   | Konto służbowe z prawidłową subskrypcją Azure publiczny |Yes |
   | Konto Microsoft z ważną subskrypcją publicznej platformy Azure |Yes |
   | Konto służbowe z prawidłową subskrypcją Azure w Chinach |Yes |
   | Konto służbowe z prawidłową nam dla instytucji rządowych subskrypcji platformy Azure |Yes |

## <a name="network"></a>Sieć
### <a name="switch"></a>Przełącznik
Jeden dostępny port przełącznika maszyny development kit.  

Na komputerze deweloperskim zestaw obsługuje łączenie się z portu dostępu przełącznika lub portu magistrali. Nie są wymagane żadne specjalne funkcje na przełączniku. Jeśli jest używany port magistrali lub konieczne jest skonfigurowanie identyfikatora sieci VLAN, należy podać identyfikator sieci VLAN jako parametr wdrożenia. Zawiera przykłady w [lista parametrów wdrożenia](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Podsieć
Nie podłączaj na komputerze deweloperskim zestaw do następujących podsieci:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Te podsieci są zastrzeżone dla wewnętrznej sieci w środowisku development kit.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Obsługiwany jest tylko protokół IPv4. Nie można tworzyć sieci obsługujących protokół IPv6.

### <a name="dhcp"></a>DHCP
Upewnij się, że serwer DHCP jest dostępny w sieci, z którą łączy się karta sieciowa. Jeśli usługa DHCP nie jest dostępna, należy przygotować dodatkowe sieci korzystające ze statycznego adresu IPv4 oprócz tej używanej przez hosta. Należy podać ten adres IP i bramę jako parametr wdrożenia. Zawiera przykłady w [lista parametrów wdrożenia](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Dostęp do Internetu
Stos Azure wymaga dostępu do Internetu, bezpośrednio lub za pośrednictwem przezroczystego obiektu pośredniczącego. Stos Azure nie obsługuje konfiguracji serwera proxy sieci web, aby umożliwić dostęp do Internetu. IP hosta i nowego adresu IP przypisane do MAS BGPNAT01 (DHCP lub statyczny adres IP) musi mieć możliwość dostępu do Internetu. Porty 80 i 443 są używane w domenach graph.windows.net i login.microsoftonline.com.

Pytanie: czy jest możliwe Uruchom ASDK na maszynie Wirtualnej platformy Azure? Odpowiedź: nie. Obecnie nie jest oficjalnie obsługiwane.

## <a name="next-steps"></a>Kolejne kroki
[Pobierz pakiet wdrażania zestawu programowanie Azure stosu](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Wdrażanie zestaw deweloperski Azure stosu](azure-stack-run-powershell-script.md)
