---
title: "Omówienie usługi Azure Cloud powłoki (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Omówienie powłoki chmury Azure."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: 7f5c034df3410fc5563a5ed657f7c1aa0f4135ec
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Omówienie powłoki chmury Azure (wersja zapoznawcza)
Powłoki chmury Azure jest interaktywny, dostępny w przeglądarce powłoki zarządzania zasobami platformy Azure.
Udostępnia było wybrać środowisko powłoki, który najlepiej odpowiada sposobu pracy.
Użytkownicy systemu Linux mogą wybrać korzystanie z powłoki Bash, zaś użytkownicy systemu Windows mogą wybrać środowisko PowerShell.

![Bash w powłoce chmury](media/overview/overview-bash-pic.png)

![PowerShell w chmurze powłoki](media/overview/overview-ps-pic.png)

## <a name="features"></a>Funkcje
### <a name="browser-based-shell-experience"></a>Środowisko powłoki przeglądarki
Chmury powłoki zapewnia dostęp do wiersza polecenia w przeglądarce skompilowanej za pomocą zadań zarządzania platformy Azure na uwadze.
Zapewniają powłoki chmury wykorzystać do pracy autonomiczne z komputera lokalnego w sposób tylko chmury.

### <a name="choice-of-preferred-shell-experience"></a>Wybór preferowanego powłoki środowisko
Usługa Azure Cloud Shell zapewnia elastyczność wyboru powłoki, która najlepiej pasuje do sposobu, w jaki pracujesz.
Użytkownicy systemu Linux mogą wybrać korzystanie z powłoki Bash, zaś użytkownicy systemu Windows mogą wybrać środowisko PowerShell.

### <a name="pre-configured-azure-workstation"></a>Wstępnie skonfigurowane Azure stacji roboczej
Powłoki chmury jest wstępnie zainstalowane z popularnych narzędzi wiersza polecenia i język obsługuje, dzięki czemu można pracować szybciej.

Wyświetl listę narzędzi Pełny dla [Bash środowisko](features.md#tools) i [środowiska PowerShell.](features-powershell.md#tools)

### <a name="automatic-authentication"></a>Automatyczne uwierzytelnianie
W każdej sesji dla natychmiastowy dostęp do zasobów przy użyciu poleceń cmdlet programu PowerShell i Azure 2.0 interfejsu wiersza polecenia powłoki chmury bezpiecznie uwierzytelnia automatycznie.

### <a name="connect-your-azure-file-storage"></a>Łączenie z magazynem plików Azure
Chmury maszyn powłoki są tymczasowe i w związku z tym wymagają udział plików na platformę Azure można zainstalować jako `clouddrive` do utrwalenia $Home katalogu.
Na pierwszego uruchomienia powłoki chmury monituje o Utwórz zasób grupy, konto magazynu i udziału plików w Twoim imieniu. To jest jednorazowe kroku i być automatycznie dołączane do wszystkich sesji. Jeden udział pliku można zamapować i będzie używana przez Bash i programu PowerShell w chmurze powłoki.

#### <a name="create-new-storage"></a>Tworzenie nowego magazynu
![](media/overview/basic-storage.png)

Magazyn lokalnie nadmiarowy (LRS) konta i udziału plików platformy Azure można tworzyć w Twoim imieniu. Udziału plików platformy Azure będzie używany dla środowisk zarówno Bash, jak i programu PowerShell, jeśli chcesz korzystać z obu. Koszty przechowywania regularne mają zastosowanie.

Trzy zasoby zostaną utworzone w Twoim imieniu:
1. Grupa zasobów o nazwie:`cloud-shell-storage-<region>`
2. Konto magazynu o nazwie:`cs<uniqueGuid>`
3. Udział plików o nazwie:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Domyślny obraz dysku 5 GB, aby utrwalić tworzy także bash w chmurze powłoki `$Home`. Wszystkie pliki w katalogu $Home, takie jak klucze SSH są trwale przechowywane w udziale zainstalowanego pliku obrazu dysku użytkownika. Zastosowanie najlepszych rozwiązań podczas zapisywania plików w katalogu $Home i udziału zainstalowanego pliku.

#### <a name="use-existing-resources"></a>Korzystać z istniejących zasobów
![](media/overview/advanced-storage.png)

Zaawansowana opcja podano do skojarzenia z istniejącymi zasobami chmury powłoki.
Kliknij przycisk "Pokaż zaawansowane ustawienia" w wierszu polecenia instalacji magazynu, aby wyświetlić dodatkowe opcje.
Listę rozwijaną są filtrowane przypisany region powłoki w chmurze i kont/globalnie — magazyn lokalnie nadmiarowy.

[Informacje na temat magazynowania powłoki chmury aktualizowanie udziałów plików i przekazywanie pobierania plików.](persisting-shell-storage.md)

## <a name="concepts"></a>Pojęcia
* Powłoka działa na tymczasowej maszyny na sesji, poszczególnych użytkowników w chmurze
* Chmura powłoki upłynie limit czasu po upływie 20 minut bez interaktywnego działania
* Powłoka chmury można uzyskać tylko z udziałem plików dołączone
* Chmury powłoki używa tego samego udziału plików dla Bash i programu PowerShell
* Chmura powłoki przypisano na jednym komputerze na konto użytkownika
* Uprawnienia zostały ustawione jako zwykłych użytkowników systemu Linux (Bash)

Dowiedz się więcej o funkcjach [Bash w chmurze powłoki](features.md) i [programu PowerShell w chmurze powłoki](features-powershell.md).

## <a name="examples"></a>Przykłady
* Korzystanie ze skryptów do automatyzacji zadań zarządzania platformy Azure
* Jednocześnie zarządzania zasobami Azure za pośrednictwem portalu Azure i narzędzi wiersza polecenia platformy Azure
* Test-Drive poleceń cmdlet Azure CLI w wersji 2.0 lub Azure PowerShell

Wypróbuj te przykłady w poradniki Szybki Start dla [Bash w chmurze powłoki](quickstart.md) i [programu PowerShell w chmurze powłoki](quickstart-powershell.md).

## <a name="pricing"></a>Cennik
Komputer, na którym powłoki chmury jest wolne, wymaga wstępnie posiadania udziału zainstalowanego pliku Azure. Koszty przechowywania regularne mają zastosowanie.

## <a name="supported-browsers"></a>Obsługiwane przeglądarki
Powłoka w chmurze jest zalecane Chrome, Edge i przeglądarki Safari.
Powłoka chmury jest obsługiwana dla programu Chrome, Firefox Safari, IE i krawędzi, powłoki chmury podlega ustawieniom konkretnej przeglądarki.
