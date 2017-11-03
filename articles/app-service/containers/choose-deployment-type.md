---
title: "Usługa aplikacji Azure na wdrażanie systemu Linux — niestandardowy obraz lub wbudowanych platformy?  | Microsoft Docs"
description: "Jak wybór między niestandardowe wdrożenie kontenera Docker i framework wbudowaną aplikację usługi aplikacji w systemie Linux"
keywords: "Usługa aplikacji Azure, aplikacji sieci web, linux, oss"
services: app-service
documentationCenter: 
authors: nickwalk
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: nickwalk
ms.openlocfilehash: 4a04bba2375b5a107bc3cb8cdc1a75d037c50af6
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="custom-image-or-built-in-platform-image"></a>Niestandardowy obraz lub obrazu platformy wbudowanego?

[Usługa aplikacji w systemie Linux](app-service-linux-intro.md) oferuje dwa różne ścieżki do pobierania aplikacji publikowanych w sieci Web:

- **Wdrażanie niestandardowego obrazu**: "Dockerize" aplikację do obrazu Docker, który zawiera wszystkie pliki i zależności w pakiecie gotowy do uruchomienia.
- **Wdrażanie aplikacji przy użyciu obrazu platformy wbudowanych**: naszych obrazy platformy wbudowanych zawierają wspólne środowisk uruchomieniowych aplikacji sieci web i zależności, takich jak Node i PHP. Skorzystaj z jednej z [metody wdrażania usługi Azure App Service](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) wdrażanie aplikacji w usłudze Magazyn aplikacji sieci web, a następnie użyć obrazu platformy wbudowanych go uruchomić.

Która metoda jest odpowiednia dla aplikacji? Podstawowy czynniki, które należy wziąć pod uwagę są:

- **Dostępność Docker w przepływie pracy programowanie**: tworzenie obrazów niestandardowych wymaga podstawową wiedzę na temat przepływu pracy programowania Docker. Wdrażanie niestandardowych obrazów aplikacji sieci web wymaga publikacji niestandardowego obrazu na hoście repozytorium, takich jak Centrum Docker. Jeśli znasz Docker i można dodawać zadania Docker do przepływu pracy kompilacji lub są już publikowanie aplikacji jako obraz Docker, najlepszym rozwiązaniem jest prawie na pewno niestandardowego obrazu.
- **Wymagania środowiska uruchomieniowego unikatowy**: obrazy wbudowane platformy zostały zaprojektowane na potrzeby większości aplikacji sieci web, ale są ograniczone w ich dostosowywalności. Aplikacja może mieć unikatowe zależności oraz innych wymagań środowiska uruchomieniowego, przekraczające co wbudowane obrazy są w stanie.
- **Wymagania dotyczące kompilacji**: Z [ciągłe wdrażanie](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), można pobrać aplikacji działa na platformie Azure bezpośrednio z kodu źródłowego. Nie jest wymagany żaden zewnętrznych proces kompilacji lub publikacji. Jednak ma limitu dostosowywalności i dostępności narzędzia kompilacji w [Kudu](https://github.com/projectkudu/kudu/wiki) aparat wdrażania. Aplikacji może pokonać możliwości Kudu w miarę zwiększania się w zależności lub wymagania dotyczące kompilacji niestandardowej logiki.
- **Wymagania dotyczące odczytu/zapisu na dysku**: wszystkie aplikacje sieci web są przydzielane wolumin magazynu dla zawartości sieci web. Jest zainstalowany ten wolumin, obsługiwanej przez usługi Azure Storage `/home` w aplikacji w systemie plików. W przeciwieństwie do plików w kontenerze systemu plików pliki zawartości na woluminie są dostępne we wszystkich wystąpieniach skalowania aplikacji, a modyfikacji zostanie zachowany po uruchomieniu aplikacji. Jednak opóźnienie zawartości woluminu dysku jest większy i więcej zmiennych niż opóźnienia kontenera lokalnego systemu plików i dostępu może mieć wpływ uaktualnienia platformy, nieplanowane przestoje i problemy z połączeniem sieciowym. Aplikacje, które wymagają duże dostęp tylko do odczytu do plików zawartości mogą korzystać z wdrożenia niestandardowego obrazu, który umieszcza pliki w systemie plików obrazu zamiast na woluminie zawartości.
- **Tworzenie użycia zasobów**: po wdrożeniu aplikacji ze źródła skryptów wdrażania uruchamiane przy użyciu Kudu tego samego planu usługi App Service zasoby obliczeniowe i magazynujące uruchomionej aplikacji. Wdrożenia dużych aplikacji może zużywać więcej zasobów lub czas niż wymagany. W szczególności wiele przepływów pracy wdrażania Generowanie działania mocno dysku na woluminie zawartości aplikacji, która nie jest zoptymalizowana pod kątem takiego działania. Obraz niestandardowy zapewnia wszystkie pliki i zależności aplikacji na platformie Azure w jednym pakiecie bez potrzeby do transferu plików dodatkowych lub akcji wdrażania.
- **Potrzebne do szybkiego iteracji**: Dockerizing aplikacji wymaga kompilacji dodatkowych kroków. Aby zmiany zaczęły obowiązywać należy wypchnąć nowego obrazu do repozytorium przy każdej aktualizacji. Te aktualizacje są następnie są pobierane do środowiska platformy Azure. Jeśli jednego z wbudowanych kontenerów zaspokoi potrzeby aplikacji, wdrażanie ze źródła może zaoferować szybsze programowanie przepływu pracy.