<properties
    pageTitle="Przed wdrożeniem usługi Azure Stack POC | Microsoft Azure"
    description="Przejrzyj wymagania dotyczące środowiska i sprzętu dla usługi Azure Stack POC (administrator usługi)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# Wymagania wstępne dotyczące wdrożenia usługi Azure Stack

Przed wdrożeniem usługi Azure Stack POC ([Proof of Concept — weryfikacja koncepcji](azure-stack-poc.md)), upewnij się, że komputer spełnia poniższe wymagania.
Wymagania te dotyczą tylko usługi Azure Stack POC i mogą ulec zmianie dla przyszłych wersji.

Pomocne może być również obejrzenie tego samouczka wideo dotyczącego wdrożenia:

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-deployment-tutorial]

## Sprzęt

| Składnik | Minimalne  | Zalecane |
|---|---|---|
| Stacje dysków: system operacyjny | 1 dysk systemu operacyjnego z co najmniej 200 GB miejsca dostępnego dla partycji systemowej (SSD lub HDD) | 1 dysk systemu operacyjnego z co najmniej 200 GB miejsca dostępnego dla partycji systemowej (SSD lub HDD) |
| Stacje dysków: ogólne dane usługi Azure Stack POC | 4 dyski. Każdy dysk udostępnia co najmniej 140 GB pojemności (SSD lub HDD). Wszystkie dostępne dyski będą używane. | 4 dyski. Każdy dysk udostępnia co najmniej 250 GB pojemności (SSD lub HDD). Wszystkie dostępne dyski będą używane.|
| Obliczenia: procesor CPU | Dwa gniazda: 12 rdzeni fizycznych (łącznie)  | Dwa gniazda: 16 rdzeni fizycznych (łącznie) |
| Obliczenia: pamięć | 96 GB pamięci RAM  | 128 GB pamięci RAM |
| Obliczenia: system BIOS | Włączona funkcja Hyper-V (z obsługą usługi SLAT)  | Włączona funkcja Hyper-V (z obsługą usługi SLAT) |
| Sieć: karta sieciowa | Wymagany certyfikat systemu Windows Server 2012 R2 dla karty sieciowej; żadne specjalne funkcje nie są wymagane | Wymagany certyfikat systemu Windows Server 2012 R2 dla karty sieciowej; żadne specjalne funkcje nie są wymagane |
| Certyfikacja logo sprzętu | [Certified for Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certified for Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

Korzystając z [Narzędzia do sprawdzania wdrożenia usługi Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1), można potwierdzić zgodność z wymaganiami.

**Konfiguracja stacji dysków danych:** wszystkie dyski danych muszą być tego samego typu (wszystkie SAS lub wszystkie SATA) i pojemności. Jeśli używane są dyski SAS, stacje dysków muszą być dołączone za pomocą pojedynczej ścieżki (nie jest zapewniana obsługa funkcji MPIO, obsługa wielościeżkowa).

**Opcje konfiguracji karty HBA**
 
- (Preferowane) Prosta karta HBA
- Karta HBA z technologią RAID — karta musi być skonfigurowana w trybie przekazywania
- Karta HBA z technologią RAID — dyski należy skonfigurować jako pojedynczy dysk, RAID-0

**Obsługiwane kombinacje typu nośnika i magistrali**

-   HDD SATA

-   HDD SAS

-   HDD RAID

-   SSD RAID (jeśli typ nośnika jest nieokreślony/nieznany\*)

-   SSD SATA + HDD SATA

-   SSD SAS + HDD SAS

\* Kontrolery RAID bez możliwości przekazywania nie rozpoznają typu nośnika. Takie kontrolery oznaczą zarówno dyski HDD, jak i dyski SSD jako nieokreślone. W takim przypadku dysk SSD będzie używany jako magazyn trwały zamiast buforowania urządzeń. W związku z tym można wdrożyć usługę Microsoft Azure Stack POC na tych dyskach SSD.

**Przykładowe karty HBA**: LSI 9207-8i, LSI-9300-8i lub LSI-9265-8i w trybie przekazywania

Dostępne są przykładowe konfiguracje OEM.




## System operacyjny

| | **Wymagania**  |
|---|---|
| **Wersja systemu operacyjnego** | Windows Server 2016 Datacenter Edition **Technical Preview 4** z zainstalowanymi najnowszymi ważnymi aktualizacjami. Plik WindowsServer2016Datacenter.vhdx znajduje się w pakiecie do pobrania. Można wykonać rozruch na tym dysku VHDX, a następnie użyć go jako podstawowego systemu operacyjnego dla wdrożenia usługi Azure Stack POC.|
| **Metody instalacji** | Czysta instalacja. Plik WindowsServer2016Datacenter.vhdx udostępniony w pakiecie wdrożeniowym umożliwia szybkie zainstalowanie systemu operacyjnego na maszynie usługi Azure Stack POC. |
| **Przyłączony do domeny?** | Nie. |


## Konta usługi Microsoft Azure Active Directory

1. Należy utworzyć konto usługi Azure AD będące administratorem katalogu dla co najmniej jednej usługi Azure Active Directory. Jeśli masz już takie konto, możesz go użyć. W przeciwnym razie możesz utworzyć je bezpłatnie na stronie [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (w Chinach zamiast tej odwiedź stronę <http://go.microsoft.com/fwlink/?LinkID=717821>).

    Zapisz te poświadczenia do użycia w kroku 6 procedury [Uruchamianie skryptu programu PowerShell](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Za pomocą tego konta *administratora usługi* można skonfigurować zasoby chmury, konta użytkowników, plany dzierżawy, limity przydziału oraz ceny i zarządzać nimi. W portalu umożliwia ono tworzenie chmur witryn sieci Web, chmur prywatnych maszyn wirtualnych, tworzenie planów i zarządzanie subskrypcjami użytkowników.

2. [Utwórz](azure-stack-add-new-user-aad.md) co najmniej jedno konto umożliwiające logowanie się do usługi Azure Stack POC jako dzierżawca.

  	| **Konto usługi Azure Active Directory**  | **Obsługiwane?** |
  	|---|---| 
  	| Identyfikator organizacji z ważną subskrypcją publicznej platformy Azure  | Tak |
  	| Konto Microsoft z ważną subskrypcją publicznej platformy Azure  | Tak |
  	| Identyfikator organizacji z ważną subskrypcją platformy Azure w Chinach  | Tak |
  	| Identyfikator organizacji z ważną subskrypcją platformy Azure dla klientów rządowych USA  | Nie |

>[AZURE.NOTE] Usługa Azure Stack POC obsługuje tylko uwierzytelnianie za pomocą usługi Azure Active Directory.


## Network (Sieć)

### Przełącznik

Jeden dostępny port na przełączniku dla maszyny usługi POC.  

Maszyna usługi Azure Stack POC obsługuje połączenie z portem dostępu przełącznika lub portem magistrali. Nie są wymagane żadne specjalne funkcje na przełączniku. Jeśli jest używany port magistrali lub konieczne jest skonfigurowanie identyfikatora sieci VLAN, należy podać identyfikator sieci VLAN jako parametr wdrożenia. Na przykład:

    DeployAzureStack.ps1 –Verbose –PublicVLan 305

Określenie tego parametru spowoduje ustawienie identyfikatora sieci VLAN tylko dla hosta i maszyny wirtualnej NAT.

### Podsieć

Nie należy łączyć maszyny usługi POC z podsieciami 192.168.200.0/24, 192.168.100.0/24 lub 192.168.133.0/24. Są one zarezerwowane dla sieci wewnętrznych w obrębie środowiska usługi Microsoft Azure Stack POC.

### IPv4/IPv6

Obsługiwany jest tylko protokół IPv4. Nie można tworzyć sieci obsługujących protokół IPv6.

### DHCP

Upewnij się, że serwer DHCP jest dostępny w sieci, z którą łączy się karta sieciowa. Jeśli usługa DHCP nie jest dostępna, należy przygotować dodatkowe sieci korzystające ze statycznego adresu IPv4 oprócz tej używanej przez hosta. Należy podać ten adres IP i bramę jako parametr wdrożenia. Na przykład:

    DeployAzureStack.ps1 -Verbose -NATVMStaticIP 10.10.10.10/24 -NATVMStaticGateway 10.10.10.1

### Dostęp do Internetu

Upewnij się, że karta sieciowa może połączyć się z Internetem. Adres IP hosta i nowy adres IP przypisane do maszyny wirtualnej NAT (za pomocą protokołu DHCP lub statycznego adresu IP) muszą mieć możliwość dostępu do Internetu. Porty 80 i 443 są używane w domenach graph.windows.net i login.windows.net.

### Serwer proxy

Jeśli serwer proxy jest wymagany w danym środowisku, należy określić adres i port serwera proxy jako parametr wdrożenia. Na przykład:

    DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080

Usługa Azure Stack POC nie obsługuje uwierzytelniania serwera proxy. 

### Telemetria

Port 443 (HTTPS) musi być otwarty dla sieci. Punktem końcowym klienta jest https://vortex-win.data.microsoft.com.


## Następne kroki

[Pobieranie pakietu wdrożeniowego usługi Azure Stack POC](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Wdrażanie usługi Azure Stack POC](azure-stack-run-powershell-script.md)



<!--HONumber=Sep16_HO3-->


