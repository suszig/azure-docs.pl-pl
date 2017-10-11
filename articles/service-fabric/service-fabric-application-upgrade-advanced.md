---
title: "Tematy dotyczące uaktualniania aplikacji zaawansowanego | Dokumentacja firmy Microsoft"
description: "W tym artykule omówiono niektóre tematy zaawansowane dotyczące uaktualniania aplikacji sieci szkieletowej usług."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: 8d3b922f3d50b645ac9db2cc879a319df1262e0a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Uaktualnianie aplikacji usługi Service Fabric: Tematy zaawansowane
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Dodawanie lub usuwanie usługi podczas uaktualniania aplikacji
Jeśli nowa usługa zostanie dodany do aplikacji, która jest już wdrożony i opublikowane jako uaktualnienie, nowej usługi jest dodawany do wdrożonej aplikacji.  Takie uaktualnienia nie wpływa na usług, które zostały już część aplikacji. Jednak należy uruchomić wystąpienie usługi, która została dodana dla nowej usługi jest aktywne (przy użyciu `New-ServiceFabricService` polecenia cmdlet).

Usługi może zostać także usunięty z aplikacji w ramach uaktualnienia. Jednak wszystkie bieżącego wystąpienia usługi usunięte być do musi zostać zatrzymana przed kontynuowaniem uaktualniania (przy użyciu `Remove-ServiceFabricService` polecenia cmdlet).

## <a name="manual-upgrade-mode"></a>Ręczny tryb uaktualniania
> [!NOTE]
> Należy rozważyć niemonitorowane ręczny tryb tylko w przypadku uaktualniania nie powiodło się lub został wstrzymany. Monitorowane tryb jest zalecany tryb uaktualniania aplikacji sieci szkieletowej usług.
>
>

Azure Service Fabric zawiera wiele trybów uaktualnienia do obsługi klastrów rozwoju i produkcji. Wybrane opcje wdrażania może się różnić w różnych środowiskach.

Uaktualnienie stopniowe aplikacji monitorowanych jest najbardziej typowe uaktualnienie do użycia w środowisku produkcyjnym. Po określeniu zasad uaktualniania sieci szkieletowej usług gwarantuje, że aplikacja jest w dobrej kondycji przed uaktualnianie będzie kontynuowane.

 Administrator aplikacji umożliwia ręczne stopniowego tryb uaktualniania aplikacji ma pełną kontrolę nad postęp uaktualnienia za pośrednictwem różnych domen uaktualnienia. Ten tryb jest przydatne, gdy wymagane są zasady oceny kondycji dostosowane lub złożonych lub wykonywane jest uaktualnienie nietypowe (na przykład aplikacja jest już utraty danych).

Na koniec automatycznego uaktualnienia stopniowego aplikacji przydaje się do rozwoju lub środowiska testowe w celu zapewnienia szybkiego iteracji cyklu podczas tworzenia usługi.

## <a name="change-to-manual-upgrade-mode"></a>Zmień na ręczny tryb uaktualniania
**Ręczne**— Zatrzymaj uaktualnienie aplikacji w bieżącym UD i zmienić niemonitorowane ręczny tryb uaktualniania. Administrator musi ręcznie wywołać **MoveNextApplicationUpgradeDomainAsync** do wyzwalacza wycofanie inicjując nowego uaktualnienia lub kontynuować proces uaktualniania. Po uaktualnieniu wchodzi w trybie ręcznym, pozostaje w trybie ręcznym do momentu zainicjowania nowego uaktualnienia. **GetApplicationUpgradeProgressAsync** polecenie zwraca sieci SZKIELETOWEJ\_aplikacji\_uaktualnienia\_stanu\_STOPNIOWYCH\_do przodu\_OCZEKUJE.

## <a name="upgrade-with-a-diff-package"></a>Uaktualnienie z pakietem różnicowego
Można uaktualnić aplikacji usługi Service Fabric przy inicjowania obsługi administracyjnej z pakietem aplikacji pełne, niezależne. Można również uaktualnić aplikację przy użyciu pakietu różnicowego, który zawiera tylko pliki zaktualizowaną aplikację, manifest aplikacji zaktualizowane i pliki manifestu usługi.

Pakiet aplikacji pełne zawiera wszystkie pliki niezbędne do uruchomienia aplikacji sieci szkieletowej usług. Pakiet różnicowego zawiera tylko te pliki, które zmienione od ostatniego udostępniania i uaktualniania bieżącego oraz manifest aplikacji pełną a usługą pliki manifestu. Wszystkie odwołania w manifeście aplikacji lub manifestu usługi, której nie można znaleźć w układzie kompilacji jest wyszukiwany w magazynie obrazu.

Pakiety aplikacji pełne są wymagane dla pierwszej instalacji aplikacji do klastra. Kolejne aktualizacje mogą być pakiet pełnej aplikacji lub pakietu różnic.

Sytuacji, gdy przy użyciu pakietu różnicowego dobrym wyborem będzie:

* Pakiet różnicowego jest preferowane w przypadku pakietu dużej aplikacji, który odwołuje się wiele plików manifestu usługi i/lub kilka pakietów kodu, pakiety konfiguracji lub pakietów danych.
* Pakiet różnicowego jest preferowane w przypadku systemu wdrożenia, w którym generuje układu kompilacji bezpośrednio w procesie kompilacji aplikacji. W takim przypadku mimo że nie zmieniła się kod, nowo zbudowany zestawy uzyskać różne sumy kontrolnej. Przy użyciu pakietu pełnej aplikacji należy zaktualizować wersję na wszystkie pakiety kodu. Przy użyciu pakietu różnicowego, musisz tylko podać zmienionych plików i pliki manifestu, których wersja uległa zmianie.

Po uaktualnieniu aplikacji przy użyciu programu Visual Studio, pakietu różnic jest automatycznie publikowany. Aby ręcznie utworzyć pakiet różnicowego, manifest aplikacji i manifestów usługi muszą zostać zaktualizowane, ale tylko zmienione pakiety powinny być uwzględnione w pakiet aplikacji końcowego.

Na przykład Zacznijmy następującej aplikacji (dostępne w celu ułatwienia zrozumienia numery wersji):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Teraz załóżmy, że chcesz zaktualizować tylko kod pakietu programu service1 przy użyciu pakietu różnicowego przy użyciu programu PowerShell. Teraz zaktualizowane aplikacji ma następującą strukturę folderów:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

W takim przypadku należy zaktualizować manifest aplikacji 2.0.0 i manifest usługi service1 w celu uwzględnienia aktualizacji pakietu kodu. Folder dla pakietu aplikacji może mieć następującą strukturę:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Następne kroki
[Uaktualnianie aplikacji za pomocą Visual Studio](service-fabric-application-upgrade-tutorial.md) przeprowadzi Cię przez proces uaktualnienia aplikacji przy użyciu programu Visual Studio.

[Uaktualnienie z aplikacji przy użyciu programu Powershell](service-fabric-application-upgrade-tutorial-powershell.md) przeprowadzi Cię przez proces uaktualnienia aplikacji przy użyciu programu PowerShell.

Kontrolowanie sposobu uaktualnienia aplikacji przy użyciu [uaktualnienia parametrów](service-fabric-application-upgrade-parameters.md).

Uzyskania uaktualnień aplikacji zgodnych przez poznanie [szeregowanie danych](service-fabric-application-upgrade-data-serialization.md).

Rozwiązywania typowych problemów w uaktualnień aplikacji, korzystając z procedury opisanej w [Rozwiązywanie problemów z uaktualnieniami aplikacji](service-fabric-application-upgrade-troubleshooting.md).
