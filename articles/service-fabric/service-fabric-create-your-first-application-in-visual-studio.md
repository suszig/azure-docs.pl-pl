---
title: "Tworzenie niezawodnej usługi Azure Service Fabric w języku C#"
description: "Twórz, wdrażaj i debuguj aplikację niezawodnej usługi skompilowaną w usłudze Azure Service Fabric przy użyciu programu Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 2ecb8f8068043936d00f2c9752666490137414e3
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Tworzenie pierwszej aplikacji niezawodnych usług stanowych w usłudze Service Fabric w języku C#

Dowiedz się, jak wdrożyć pierwszą aplikację usługi Service Fabric dla platformy .NET w systemie Windows w ciągu kilku minut. Po zakończeniu będziesz mieć uruchomiony lokalny klaster z aplikacją niezawodnej usługi.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz [skonfigurowane środowisko programowania](service-fabric-get-started.md). Obejmuje to również zainstalowanie zestawu SDK usługi Service Fabric i programu Visual Studio 2017 lub 2015.

## <a name="create-the-application"></a>Tworzenie aplikacji

Uruchom program Visual Studio jako **administrator**.

Tworzenie projektu przy użyciu klawiszy `CTRL`+`SHIFT`+`N`

W oknie dialogowym **Nowy projekt** wybierz kolejno pozycje **Chmura > Aplikacja usługi Service Fabric**.

Nazwij aplikację **MyApplication** i naciśnij przycisk **OK**.

   
![Okno dialogowe nowego projektu w programie Visual Studio][1]

W następnym oknie dialogowym można utworzyć aplikację usługi Service Fabric dowolnego typu. Na potrzeby tego przewodnika Szybki start wybierz pozycję **Usługa stanowa**.

Nazwij usługę **MyStatefulService** i naciśnij przycisk **OK**.

![Okno dialogowe nowej usługi w programie Visual Studio][2]


Program Visual Studio utworzy projekt aplikacji i projekt usługi stanowej, a następnie wyświetli je w Eksploratorze rozwiązań.

![Eksplorator rozwiązań po utworzeniu aplikacji z usługą stanową][3]

Projekt aplikacji (**MyApplication**) nie zawiera bezpośrednio żadnego kodu. Zamiast tego odwołuje się do zestawu projektów usług. Ponadto zawiera trzy inne typy zawartości:

* **Profile publikowania**  
Profile na potrzeby wdrażania w różnych środowiskach.

* **Skrypty**  
Skrypt programu PowerShell przeznaczony do wdrażania/uaktualniania aplikacji.

* **Definicja aplikacji**  
W obszarze *ApplicationPackageRoot* znajduje się plik ApplicationManifest.xml, który opisuje kompozycję aplikacji. Skojarzone pliki parametrów aplikacji znajdują się w obszarze *ApplicationParameters* i mogą służyć do określania parametrów związanych z danym środowiskiem. Program Visual Studio wybiera plik parametrów aplikacji określony w skojarzonym profilu publikowania podczas wdrażania w danym środowisku.
    
Aby zapoznać się z omówieniem zawartości projektu usługi, zobacz [Pierwsze kroki z usługami Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Wdrażanie i debugowanie aplikacji

Teraz, gdy masz już aplikację, uruchom ją.

W programie Visual Studio naciśnij klawisz `F5`, aby wdrożyć aplikację do debugowania.

>[!NOTE]
>Przy pierwszym uruchamianiu i wdrażaniu aplikacji w środowisku lokalnym program Visual Studio tworzy lokalny klaster na potrzeby debugowania. Może to potrwać jakiś czas. Stan tworzenia klastra jest wyświetlany w oknie danych wyjściowych programu Visual Studio.

Gdy klaster jest gotowy, z aplikacji znajdującej się w zestawie SDK otrzymasz powiadomienie menedżera paska zadań klastra lokalnego.
   
![Powiadomienie klastra lokalnego na pasku zadań][4]

Po uruchomieniu aplikacji program Visual Studio automatycznie wyświetla okno **podglądu zdarzeń diagnostycznych**, w którym można zobaczyć wyniki śledzenia usług.
   
![Podgląd zdarzeń diagnostycznych][5]

>[!NOTE]
>Zdarzenia powinny automatycznie rozpocząć śledzenie w Podglądzie zdarzeń diagnostycznych, ale jeśli konieczne się okaże ich ręczne skonfigurowanie, najpierw otwórz plik `ServiceEventSource.cs` znajdujący się w projekcie **MyStatefulService**. Skopiuj wartość atrybutu `EventSource` u góry klasy `ServiceEventSource`. W przykładzie poniżej źródło zdarzenia ma nazwę `"MyCompany-MyApplication-MyStatefulService"` (u Ciebie może ona być inna).
>
>![Lokalizowanie nazwy źródła zdarzeń usługi][service-event-source-name]
>
>Następnie kliknij ikonę koła zębatego zlokalizowaną na karcie Podgląd zdarzeń diagnostycznych w celu otworzenia okna dialogowego **Dostawcy ETW**. Wklej nazwę właśnie skopiowanego źródła zdarzeń do pola wejściowego **Dostawcy ETW**. Następnie kliknij przycisk **Zastosuj**. To spowoduje automatyczne rozpoczęcie śledzenia zdarzeń.
>
>![Ustawianie nazwy źródła zdarzeń diagnostycznych][setting-event-source-name]
>
>W oknie Zdarzenia diagnostyczne powinny się teraz zacząć pojawiać zdarzenia.

Używany szablon usługi stanowej przedstawia wartość licznika, która zwiększa się w metodzie `RunAsync` w pliku **MyStatefulService.cs**.

Rozwiń jedno ze zdarzeń, aby zobaczyć więcej szczegółów, w tym węzeł, w którym wykonywany jest kod. W tym przypadku jest to węzeł \_Node\_0, ale może się to różnić w zależności od maszyny.
   
![Szczegóły w podglądzie zdarzeń diagnostycznych][6]

Klaster lokalny zawiera pięć węzłów, które są hostowane na jednej maszynie. W środowisku produkcyjnym każdy węzeł jest hostowany na odrębnej maszynie fizycznej lub wirtualnej. Wyłączmy jeden z węzłów w klastrze lokalnym, aby zasymulować utratę maszyny przy jednoczesnym działaniu debugera programu Visual Studio.

W oknie **Eksplorator rozwiązań** otwórz plik **MyStatefulService.cs**. 

Znajdź metodę `RunAsync` i ustaw punkt przerwania w pierwszym wierszu metody.

![Punkt przerwania w metodzie RunAsync usługi stanowej][7]

Aby uruchomić narzędzie **Service Fabric Explorer**, kliknij prawym przyciskiem myszy aplikację **Menedżer klastra lokalnego** na pasku zadań i wybierz polecenie **Zarządzaj klastrem lokalnym**.

![Uruchamianie narzędzia Service Fabric Explorer z menedżera klastra lokalnego][systray-launch-sfx]

Narzędzie [**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) oferuje wizualną reprezentację klastra. Zawiera ono zestaw aplikacji wdrożonych w klastrze oraz zestaw składających się na niego węzłów fizycznych.

W lewym okienku rozwiń opcje **Klaster > Węzły** i znajdź węzeł, w którym wykonywany jest kod.

Kliknij pozycje **Akcje > Dezaktywuj (ponowne uruchomienie)**, aby zasymulować ponowne uruchomienie maszyny.

![Zatrzymanie węzła w narzędziu Service Fabric Explorer][sfx-stop-node]

Chwilowo powinien być widoczny trafiony punkt przerwania w programie Visual Studio, gdy obliczenia wykonywane w jednym węźle zostaną awaryjnie przełączone na inny węzeł.


Następnie wróć do podglądu zdarzeń diagnostycznych i sprawdź komunikaty. Wartość licznika nadal wzrastała, mimo że zdarzenia w rzeczywistości pochodzą z innego węzła.

![Szczegóły w podglądzie zdarzeń diagnostycznych po przełączeniu do trybu failover][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a>Czyszczenie klastra lokalnego (opcjonalnie)

Pamiętaj, że ten klaster lokalny to klaster rzeczywisty. Zatrzymanie debugera powoduje usunięcie wystąpienia aplikacji i wyrejestrowanie typu aplikacji. Klaster będzie jednak nadal uruchomiony w tle. Jeśli chcesz zatrzymać klaster lokalny, możesz to zrobić na kilka sposobów.

### <a name="keep-application-and-trace-data"></a>Przechowywanie danych aplikacji i śledzenia

Aby zamknąć klaster, kliknij prawym przyciskiem myszy aplikację **Menedżer klastra lokalnego** na pasku zadań i wybierz polecenie **Stop Local Cluster** (Zatrzymaj klaster lokalny).

### <a name="delete-the-cluster-and-all-data"></a>Usuwanie klastra i wszystkich danych

Aby usunąć klaster, kliknij prawym przyciskiem myszy aplikację **Menedżer klastra lokalnego** na pasku zadań i wybierz polecenie **Remove Local Cluster** (Usuń klaster lokalny). 

Jeśli wybierzesz tę opcję, program Visual Studio ponownie wdroży klaster po następnym uruchomieniu aplikacji. Wybierz tę opcję tylko wtedy, gdy nie będziesz używać klastra lokalnego przez pewien czas lub gdy chcesz odzyskać zasoby.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [niezawodnych usługach](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
