---
title: "Tworzenie pierwszej aplikacji mikrousług platformy Azure | Microsoft Docs"
description: "Tworzenie, wdrażanie i debugowanie aplikacji usługi Service Fabric przy użyciu programu Visual Studio"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/07/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 296f02dd7deb22fd4ca15478b7f90a7688b4304a
ms.lasthandoff: 03/08/2017


---
# <a name="create-your-first-azure-service-fabric-application"></a>Tworzenie pierwszej aplikacji usługi Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# — Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# — Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Zestaw SDK usługi Service Fabric zawiera dodatek dla programu Visual Studio, który zawiera szablony i narzędzia do tworzenia, wdrażania i debugowania aplikacji usługi Service Fabric. W tym temacie przedstawiono proces tworzenia pierwszej aplikacji w programie Visual Studio 2017 lub Visual Studio 2015.

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem upewnij się, że masz [skonfigurowane środowisko programowania](service-fabric-get-started.md).

## <a name="video-walkthrough"></a>Przewodnik wideo
Poniższe wideo przedstawia kroki omówione w niniejszym samouczku:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Creating-your-first-Service-Fabric-application-in-Visual-Studio/player]
> 
> 

## <a name="create-the-application"></a>Tworzenie aplikacji
Aplikacja usługi Service Fabric może zawierać jedną lub więcej usług, a każda z nich pełni określoną rolę w dostarczaniu funkcjonalności aplikacji. Za pomocą kreatora nowego projektu można utworzyć projekt aplikacji wraz z projektem pierwszej usługi. Możesz też dodać później więcej usług.

1. Uruchom program Visual Studio jako administrator.
2. Kliknij pozycję **Plik > Nowy Projekt > Chmura > Aplikacja usługi Service Fabric**.
3. Nazwij aplikację, a następnie kliknij przycisk **OK**.
   
    ![Okno dialogowe nowego projektu w programie Visual Studio][1]
4. Na następnej stronie wybierz pozycję **Stanowy** jako pierwszy typ usługi, który zostanie dodany do aplikacji. Nadaj mu nazwę, a następnie kliknij przycisk **OK**.
   
    ![Okno dialogowe nowej usługi w programie Visual Studio][2]
   
   > [!NOTE]
   > Aby uzyskać więcej informacji o opcjach, zobacz [Service Fabric programming model overview](service-fabric-choose-framework.md) (Omówienie modelu programowania usługi Service Fabric).
   > 
   > 
   
    Program Visual Studio utworzy projekt aplikacji i projekt usługi stanowej, a następnie wyświetli je w Eksploratorze rozwiązań.
   
    ![Eksplorator rozwiązań po utworzeniu aplikacji z usługą stanową][3]
   
    Projekt aplikacji nie zawiera żadnego kodu bezpośrednio. Zamiast tego odwołuje się do zestawu projektów usług. Ponadto zawiera trzy inne typy zawartości:
   
   * **Profile publikowane**: służące do zarządzania preferencjami narzędzi dla różnych środowisk.
   * **Skrypty**: zawiera skrypt programu PowerShell przeznaczony do wdrażania/uaktualniania aplikacji. Program Visual Studio używa tego skryptu w tle. Skrypt można również wywołać bezpośrednio z poziomu wiersza polecenia.
   * **Definicja aplikacji**: zawiera manifest aplikacji w ramach elementu *ApplicationPackageRoot*. Skojarzone pliki parametrów aplikacji znajdują się w ramach elementu *ApplicationParameters*, które definiują aplikację i umożliwiają jej konfigurowanie dla konkretnego środowiska.
     
     Aby zapoznać się z omówieniem zawartości projektu usługi, zobacz [Pierwsze kroki z usługami Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Wdrażanie i debugowanie aplikacji
Teraz, gdy masz już aplikację, spróbuj ją uruchomić.

1. W programie Visual Studio naciśnij klawisz F5, aby wdrożyć aplikację do debugowania.
   
   > [!NOTE]
   > Za pierwszym razem wdrożenie może trochę potrwać, ponieważ program Visual Studio tworzy lokalny klaster na potrzeby programowania. W klastrze lokalnym działa ten sam kod platformy, który stanowi podstawę do kompilacji w klastrze z wieloma maszynami, tylko że w tym przypadku jest to jedna maszyna. Stan tworzenia klastra zostanie wyświetlony w oknie danych wyjściowych programu Visual Studio.
   > 
   > 
   
    Gdy klaster jest gotowy, z aplikacji znajdującej się w zestawie SDK otrzymasz powiadomienie menedżera paska zadań klastra lokalnego.
   
    ![Powiadomienie klastra lokalnego na pasku zadań][4]
2. Po uruchomieniu aplikacji program Visual Studio automatycznie wyświetli okno podglądu zdarzeń diagnostycznych, w którym można zobaczyć wyniki śledzenia usługi.
   
    ![Podgląd zdarzeń diagnostycznych][5]
   
    W przypadku szablonu usługi stanowej komunikaty po prostu przedstawiają wartość licznika, która zwiększa się w metodzie `RunAsync` w pliku MyStatefulService.cs.
3. Rozwiń jedno ze zdarzeń, aby zobaczyć więcej szczegółów, w tym węzeł, w którym wykonywany jest kod. W tym przypadku jest to węzeł _Node_2, ale może się różnić w zależności od maszyny.
   
    ![Szczegóły w podglądzie zdarzeń diagnostycznych][6]
   
    Klaster lokalny zawiera pięć węzłów, które są hostowane na jednej maszynie. Naśladuje klaster z pięcioma węzłami, którego węzły znajdują się na różnych maszynach. Wyłączmy jeden z węzłów w klastrze lokalnym, aby zasymulować utratę maszyny przy jednoczesnym działaniu debugera programu Visual Studio.
   
   > [!NOTE]
   > Zdarzenia diagnostyczne aplikacji, które są emitowane przez szablon projektu, używają dołączonej klasy `ServiceEventSource`. Aby uzyskać więcej informacji, zobacz [Sposób monitorowania i diagnozowania usług lokalnie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
   > 
   > 
4. Znajdź w projekcie usługi klasę pochodzącą z klasy StatefulService (np. MyStatefulService) i ustaw punkt przerwania w pierwszym wierszu metody `RunAsync`.
   
    ![Punkt przerwania w metodzie RunAsync usługi stanowej][7]
5. Aby uruchomić narzędzie Service Fabric Explorer, kliknij prawym przyciskiem myszy aplikację na pasku zadań menedżera klastra lokalnego i wybierz polecenie **Zarządzaj klastrem lokalnym**.
   
    ![Uruchamianie narzędzia Service Fabric Explorer z menedżera klastra lokalnego][systray-launch-sfx]
   
    Narzędzie Service Fabric Explorer oferuje wizualną reprezentację klastra — w tym zestaw wdrożonych w nim aplikacji, a także zestaw węzłów fizycznych, które go tworzą. Aby uzyskać więcej informacji na temat narzędzia Service Fabric Explorer, zobacz [Wizualizacja klastra](service-fabric-visualizing-your-cluster.md).
6. W lewym okienku rozwiń opcje **Klaster > Węzły** i znajdź węzeł, w którym wykonywany jest kod.
7. Kliknij pozycje **Akcje > Dezaktywuj (ponowne uruchomienie)**, aby zasymulować ponowne uruchomienie maszyny. Możesz też dezaktywować węzeł w widoku listy węzłów w okienku po lewej stronie.
   
    ![Zatrzymanie węzła w narzędziu Service Fabric Explorer][sfx-stop-node]
   
    Chwilowo powinien być widoczny trafiony punkt przerwania w programie Visual Studio, gdy obliczenia wykonywane w jednym węźle zostaną awaryjnie przełączone na inny węzeł.
8. Wróć do podglądu zdarzeń diagnostycznych i sprawdź komunikaty. Wartość licznika nadal wzrastała, mimo że zdarzenia w rzeczywistości pochodzą z innego węzła.
   
    ![Szczegóły w podglądzie zdarzeń diagnostycznych po przełączeniu do trybu failover][diagnostic-events-viewer-detail-post-failover]

## <a name="switch-cluster-mode"></a>Przełączanie trybu klastra
Domyślnie lokalny klaster projektowy jest skonfigurowany do uruchamiania jako klaster z pięcioma węzłami, co ułatwia debugowanie usług wdrożonych w wielu węzłach. Wdrożenie aplikacji w klastrze projektowym z pięcioma węzłami może być jednak czasochłonne. Jeśli chcesz szybko iterować zmiany kodu bez uruchamiania aplikacji w pięciu węzłach, przełącz klaster projektowy do trybu jednego węzła. Aby uruchomić kod w klastrze z jednym węzłem, kliknij prawym przyciskiem myszy ikonę Menedżer klastra lokalnego na pasku zadań i wybierz polecenie **Przełącz tryb klastra -> 1 węzeł**.  

![Przełączanie trybu klastra][switch-cluster-mode]

Po zmianie trybu klastra klaster projektowy jest resetowany i wszystkie aplikacje zaprowizowane lub działające w klastrze są usuwane.

Możesz też zmienić tryb klastra przy użyciu programu PowerShell:

1. Uruchom nowe okno programu PowerShell jako administrator.
2. Uruchom skrypt instalacji klastra z folderu zestawu SDK:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    Instalacja klastra trwa kilka chwil. Po zakończeniu instalacji powinny być widoczne dane wyjściowe podobne do poniższych:
   
    ![Dane wyjściowe instalacji klastra][cluster-setup-success-1-node]

## <a name="cleaning-up"></a>Czyszczenie
Przed zakończeniem należy pamiętać, że klaster lokalny jest prawdziwy. Zatrzymanie debugera powoduje usunięcie wystąpienia aplikacji i wyrejestrowanie typu aplikacji. Klaster będzie jednak nadal uruchomiony w tle. Istnieje kilka możliwości zarządzania klastrem:

1. Aby zamknąć klaster, zachowując dane i ślady aplikacji, kliknij opcję **Zatrzymaj klaster lokalny** na pasku zadań systemu.
2. Aby całkowicie usunąć klaster, kliknij opcję **Usuń klaster lokalny** na pasku zadań systemu. Zastosowanie tej opcji spowoduje powolne wdrożenie po następnym naciśnięciu klawisza F5 w programie Visual Studio. Klaster lokalny należy usunąć tylko wtedy, gdy nie będziesz go używać przez pewien czas lub gdy chcesz odzyskać zasoby.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak utworzyć [klaster na platformie Azure](service-fabric-cluster-creation-via-portal.md) lub [autonomiczny klaster w systemie Windows](service-fabric-cluster-creation-for-windows-server.md).
* Spróbuj utworzyć usługę za pomocą usług [Reliable Services](service-fabric-reliable-services-quick-start.md) lub modelu programowania [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Spróbuj wdrożyć [kontener systemu Windows](service-fabric-deploy-container.md) lub istniejącą aplikację jako [aplikację wykonywalną gościa](service-fabric-deploy-existing-app.md).
* Dowiedz się, jak udostępnić swoje usługi w Internecie za pomocą [frontonu usługi sieci Web](service-fabric-add-a-web-frontend.md).
* Zapoznaj się z [warsztatem](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) i utwórz usługę bezstanową, skonfiguruj raporty dotyczące monitorowania i kondycji oraz wykonaj uaktualnienie aplikacji.
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

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

