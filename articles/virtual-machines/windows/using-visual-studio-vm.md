---
title: "Za pomocą programu Visual Studio na maszynie wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "Za pomocą programu Visual Studio na maszynie wirtualnej platformy Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: sacalla
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 01/30/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: 813022f1778e2c7f3174e11192b845c2c33ad219
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a id="top"></a> Obrazów programu visual Studio na platformie Azure
Za pomocą programu Visual Studio działa w wstępnie skonfigurowane Azure maszyny wirtualnej (VM) jest łatwiejsze i szybsze sposobem przejść z nic do środowiska projektowego w górę i uruchomiona.  Obrazy systemu z różnymi konfiguracjami programu Visual Studio są dostępne w [portalu Azure Marketplace](https://portal.azure.com/). Po prostu rozruchu maszyny Wirtualnej i Wyłącz możesz przejść.

Dopiero zaczynasz korzystać z platformy Azure? [Tworzenie bezpłatnego konta platformy Azure](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Jakie konfiguracje i wersje są dostępne?
W portalu Azure Marketplace możesz znaleźć obrazy dla najnowszej wersji głównych: Visual Studio 2017 i programu Visual Studio 2015.  Dla każdej wersji głównej widzisz pierwotnie wydanej (alias ' RTW") wersji, a"najnowszej"zaktualizowanych wersji.  Dla każdego z tych różnych wersji można znaleźć w różnych wersjach programu Visual Studio Enterprise i Visual Studio Community.

|               Wersja              |          Wersje            |    Wersja produktu:    |
|:------------------------------------------:|:----------------------------:|:---------------------:|
| Visual Studio 2017 — najnowsza wersja (wersja 15,5 cala) |    Enterprise, Community     |     Wersja 15.5.3    |
|         Visual Studio 2017 - RTW           |    Enterprise, Community     |     Wersja 15.0.7    |
|   Visual Studio 2015 — najnowsza wersja (Update 3)   |    Enterprise, Community     | Wersja 14.0.25431.01 |
|         Visual Studio 2015 — RTW           | Brak (wygasłe obsługi) |          ---          |

> [!NOTE]
> Zgodnie z Microsoft obsługi zasad pierwotnie wydanej (alias ' RTW') wygasła wersja programu Visual Studio 2015 obsługi.  W związku z tym Visual Studio 2015 Update 3 jest tylko wersja pozostałych oferowane dla linii produktów Visual Studio 2015.

Aby uzyskać więcej informacji, zobacz [programu Visual Studio obsługi zasad](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Jakie funkcje są zainstalowane?
Obraz zawiera skonfigurowaną zalecane w przypadku tej wersji programu Visual Studio.  Ogólnie rzecz biorąc instalacja obejmuje:

* Wszystkie dostępne obciążenia, aby obciążenie w tym zalecane składniki opcjonalne
* Program .NET 4.6.2 i .NET 4.7 zestawów SDK, określania wartości docelowej pakiety i narzędzia dla deweloperów
* Visual F #
* Rozszerzenie GitHub dla programu Visual Studio
* LINQ do SQL narzędzia

To jest używana do zainstalowania programu Visual Studio, tworząc obrazy wiersza polecenia:

   * vs_enterprise.exe — allWorkloads — includeRecommended — pasywnym ^
   * Dodaj Microsoft.Net.Component.4.7.SDK ^
   * Dodaj Microsoft.Net.Component.4.7.TargetingPack ^ 
   * Dodaj Microsoft.Net.Component.4.6.2.SDK ^
   * Dodaj Microsoft.Net.Component.4.6.2.TargetingPack ^
   * Dodaj Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
   * Dodaj Microsoft.VisualStudio.Component.FSharp ^
   * add Component.GitHub.VisualStudio ^
   * add Microsoft.VisualStudio.Component.LinqToSql

Jeśli obrazy nie zawiera funkcji programu Visual Studio, które są wymagane, podaj tę opinię za pomocą narzędzia opinii (prawym górnym rogu strony).

## <a name="what-size-vm-should-i-choose"></a>Jaki rozmiar maszyny Wirtualnej należy wybrać?
Alokowanie nowej maszyny wirtualnej jest proste, a system Azure oferuje szeroką gamę rozmiarów maszyn wirtualnych.  Podobnie jak w przypadku dowolnego zakupów sprzętu ma być saldo wydajności i kosztów.  Ponieważ program Visual Studio jest aplikacji zaawansowane, wielowątkowych, ma rozmiar maszyny Wirtualnej, która zawiera co najmniej dwa procesory i 7 GB pamięci.  Aby uzyskać więcej informacji na najnowszej rozmiary zobacz [rozmiary dla systemu Windows maszyny wirtualne na platformie Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Przy użyciu platformy Azure użytkownik nie jest wstrzymywana Twojego pierwszego pobrania — ponowne zrównoważenie początkowy wybór zmiana rozmiaru maszyny Wirtualnej.  Albo obsługi administracyjnej nowej maszyny Wirtualnej o rozmiarze bardziej odpowiednie lub zmianę istniejącej maszyny Wirtualnej na inny komputer źródłowy.  Aby uzyskać więcej informacji, zobacz [zmiana rozmiaru maszyny Wirtualnej systemu Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-i-get-the-vm-running-then-what"></a>Po uzyskać uruchomionych maszyn wirtualnych, następnie co?
Visual Studio następuje modelu "w bring możesz własnej licencji" na platformie Azure.  Tak podobnie do instalacji na sprzęcie, jednym z pierwszych kroków jest licencjonowania instalację programu Visual Studio.  Odblokować programu Visual Studio, albo logowanie się przy użyciu konta Microsoft, który został skojarzony z subskrypcją programu Visual Studio lub Visual Studio z kluczem produktu można odblokować z początkowej zakupu.  Aby uzyskać więcej informacji, zobacz [rejestrowanie w programie Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) i [jak odblokować program Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="after-i-build-out-the-dev-vm-how-do-i-save-capture-it-for-future-or-team-use"></a>Po I kompilacji limit deweloperów maszyny Wirtualnej, jak zapisać (przechwycenie) go na przyszłość lub użyj zespołu?

Spektrum środowisk deweloperskich jest ogromny, a istnieje rzeczywistych kosztów związanych z zbudowaniu bardziej złożonych środowiskach.  Niezależnie od konfiguracji w środowisku Azure udostępnia jednak zachowania inwestycyjnych łatwe przez zapisywanie/Przechwytywanie doskonale skonfigurowane maszyny Wirtualnej jako "podstawowy obraz" do użytku w przyszłości — dla siebie i/lub innych członków zespołu.  Następnie podczas rozruchu nowej maszyny Wirtualnej, udostępnij je z obrazu podstawowego zamiast obrazu z witryny Marketplace.

Jako krótkie podsumowanie, musisz narzędzia sysprep i zamknij uruchomionej maszyny Wirtualnej, następnie *przechwytywania (rysunek 1)* maszyny Wirtualnej jako obraz za pomocą interfejsu użytkownika portalu Azure.  Azure będzie zapisywał `.vhd` pliku zawierającego obraz na koncie magazynu wybrane.  Następnie nowy obraz jest wyświetlany jako zasób obrazu w Twojej subskrypcji listy zasobów.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal’s UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Rysunek 1) Przechwycenie obrazu przy użyciu interfejsu użytkownika portalu Azure.*</center>

Aby uzyskać więcej informacji, zobacz [Przechwytywanie maszyny Wirtualnej, aby obraz](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

  **Przypomnienie:** nie zapomnij sysprep maszyny Wirtualnej!  Jeśli pominiesz ten krok Azure nie może obsłużyć maszyny Wirtualnej z obrazu.

> [!NOTE]
> Nadal naliczane pewien koszt związany z magazynu obrazów, ale ten koszt przyrostowe jest prawdopodobnie nieistotne w porównaniu do kosztów siły odbudować maszyny Wirtualnej od początku — dla każdej osoby w zespole wymaga maszyny Wirtualnej.  Na przykład kosztuje kilka dolarów do tworzenia i przechowywania obrazu 127 GB dla miesiąca, w którym jest wielokrotnego użytku przez wszystkie elementy członkowskie zespołu.  Koszty te są jednak niewielka w porównaniu do godziny, w których każdy pracownik inwestuje do tworzenia, a następnie sprawdź okno deweloperów poprawnie skonfigurowane do własnego użytku.

Ponadto zadań rozwoju lub technologii mogą wymagać więcej skali — podobnie jak odmian konfiguracje programowanie i wielu konfiguracji maszyny.  Azure DevTest Labs umożliwia utworzenie _przepisami_ który zautomatyzować konstrukcji "złotego obrazu" i zarządzanie zasadami dla swojego zespołu na działających maszyn wirtualnych.  [Za pomocą usługi Azure DevTest Labs dla deweloperów](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) jest najlepsze źródła, aby uzyskać więcej informacji na temat DevTest Labs.

## <a name="next-steps"></a>Kolejne kroki
Teraz, gdy wiadomo na temat wstępnie skonfigurowane obrazów programu Visual Studio, następnym krokiem jest tworzenie nowej maszyny Wirtualnej:

* [Utwórz maszynę Wirtualną za pośrednictwem portalu Azure](quick-create-portal.md)
* [Maszyn wirtualnych systemu Windows — omówienie](overview.md)
