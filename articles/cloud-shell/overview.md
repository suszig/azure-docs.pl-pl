---
title: "Omówienie powłoki chmury Azure | Dokumentacja firmy Microsoft"
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
ms.date: 11/13/2017
ms.author: juluk
ms.openlocfilehash: ebf6f1256a280fdff18c0c9060614acf0d4a642b
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="overview-of-azure-cloud-shell"></a>Omówienie powłoki w chmurze Azure
Powłoki chmury Azure jest interaktywny, dostępny w przeglądarce powłoki zarządzania zasobami platformy Azure.
Udostępnia było wybrać środowisko powłoki, który najlepiej odpowiada sposobu pracy.
Użytkownicy systemu Linux mogą wybrać korzystanie z powłoki Bash, zaś użytkownicy systemu Windows mogą wybrać środowisko PowerShell.

Uruchamianie za pośrednictwem portalu Azure z ikony powłoki chmury:

![Uruchomienie portalu](media/overview/portal-launch-icon.png)

Wykorzystanie Bash lub programu PowerShell z listy rozwijanej selektora powłoki:

![Bash w powłoce chmury](media/overview/overview-bash-pic.png)

![PowerShell w chmurze powłoki (wersja zapoznawcza)](media/overview/overview-ps-pic.png)

## <a name="features"></a>Funkcje
### <a name="browser-based-shell-experience"></a>Środowisko powłoki przeglądarki
Chmury powłoki zapewnia dostęp do wiersza polecenia w przeglądarce skompilowanej za pomocą zadań zarządzania platformy Azure na uwadze.
Zapewniają powłoki chmury wykorzystać do pracy autonomiczne z komputera lokalnego w sposób tylko chmury.

### <a name="choice-of-preferred-shell-experience"></a>Wybór preferowanego powłoki środowisko
Usługa Azure Cloud Shell zapewnia elastyczność wyboru powłoki, która najlepiej pasuje do sposobu, w jaki pracujesz.
Użytkowników systemu Linux można wybrać opcję Bash w powłoce chmury podczas włączyć użytkowników systemu Windows dla programu PowerShell w chmurze powłoki (wersja zapoznawcza).

### <a name="authenticated-and-configured-azure-workstation"></a>Uwierzytelniony i skonfigurowane Azure stacji roboczej
Chmury powłoki pochodzi zarządzanego przez firmę Microsoft, dzięki wstępnie zainstalować go z popularnych narzędzi wiersza polecenia i język obsługuje, dzięki czemu można pracować szybciej. Ponadto powłoki chmury bezpiecznie uwierzytelnia automatycznie dla natychmiastowy dostęp do zasobów za pomocą poleceń cmdlet Azure CLI w wersji 2.0 lub Azure PowerShell.

Wyświetl listę narzędzi Pełny dla [Bash środowisko](features.md#tools) i [środowiska PowerShell (wersja zapoznawcza).](features-powershell.md#tools)

### <a name="multiple-access-points"></a>Wiele punktów dostępu
Oprócz powłoki chmurze są dostępne w portalu Azure jest również dostępny od:
* [Dokumentację platformy Azure "Wypróbuj" 2.0 interfejsu wiersza polecenia](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Aplikacji mobilnej Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Rozszerzenie kodu programu Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-azure-files-storage"></a>Łączenie z magazynem plików Azure
Chmury maszyn powłoki są tymczasowe i w związku z tym wymagają udział plików Azure można zainstalować jako `clouddrive` do utrwalenia $Home katalogu.
Na pierwszego uruchomienia powłoki chmury monituje o Utwórz zasób grupy, konto magazynu i udziału plików w Twoim imieniu. To jest jednorazowe kroku i być automatycznie dołączane do wszystkich sesji. Jeden udział pliku można zamapować i będzie używana przez Bash i programu PowerShell w chmurze powłoki (wersja zapoznawcza).

#### <a name="create-new-storage"></a>Tworzenie nowego magazynu
![](media/overview/basic-storage.png)

Magazyn lokalnie nadmiarowy (LRS) konta i udział plików Azure można tworzyć w Twoim imieniu. Udział plików Azure będzie używany dla środowisk zarówno Bash, jak i programu PowerShell, jeśli chcesz korzystać z obu. Koszty przechowywania regularne mają zastosowanie.

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

Dowiedz się więcej o funkcjach [Bash w chmurze powłoki](features.md) i [programu PowerShell w chmurze powłoki (wersja zapoznawcza)](features-powershell.md).

## <a name="examples"></a>Przykłady
* Korzystanie ze skryptów do automatyzacji zadań zarządzania platformy Azure
* Jednocześnie zarządzania zasobami Azure za pośrednictwem portalu Azure i narzędzi wiersza polecenia platformy Azure
* Test-Drive poleceń cmdlet Azure CLI w wersji 2.0 lub Azure PowerShell

Wypróbuj te przykłady w poradniki Szybki Start dla [Bash w chmurze powłoki](quickstart.md) i [programu PowerShell w chmurze powłoki (wersja zapoznawcza)](quickstart-powershell.md).

## <a name="pricing"></a>Cennik
Komputer, na którym powłoki chmury jest wolne, wymaga wstępnie posiadania zainstalowany udział plików Azure. Koszty przechowywania regularne mają zastosowanie.

## <a name="next-steps"></a>Następne kroki
[Bash w chmurze powłoki szybkiego startu](quickstart.md)
[programu PowerShell w chmurze powłoki (wersja zapoznawcza) — Szybki Start](quickstart-powershell.md)