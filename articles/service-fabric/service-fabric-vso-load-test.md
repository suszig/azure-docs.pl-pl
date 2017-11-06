---
title: "Aplikacja testu obciążenia za pomocą programu Visual Studio Team Services | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przetestować aplikacje sieć szkieletowa usług Azure za pomocą programu Visual Studio Team Services."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: e8e270ce865d4da3ee219958b308db2c1c89b11b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Aplikacja testu obciążenia za pomocą programu Visual Studio Team Services
W tym artykule pokazano, jak przetestować za pomocą funkcji testu obciążenia programu Microsoft Visual Studio aplikacji. Używa usługi Azure Service Fabric usługi stanowej zaplecza i usługi bezstanowej frontonu sieci web. Przykładowa aplikacja używana w tym miejscu jest samolotowy symulatora lokalizacji. Musisz podać identyfikator samolotowy, czas wyjścia i docelowej. Zaplecze aplikacji przetwarza żądania i fronton wyświetla na mapie samolotowy, który spełnia kryteria.

Na poniższym diagramie przedstawiono aplikacji usługi Service Fabric, który będzie testowania.

![Diagram przykładowej aplikacji lokalizacji samolotowy][0]

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem pracy należy wykonać następujące czynności:

* Uzyskaj konto usługi Visual Studio Team Services. Możesz ją uzyskać, za darmo w [Visual Studio Team Services](https://www.visualstudio.com).
* Pobierz i zainstaluj program Visual Studio 2013 lub Visual Studio 2015. W tym artykule korzysta z wersji programu Visual Studio Enterprise 2015, ale programu Visual Studio 2013 i inne wersje powinny działać podobnie.
* Wdrażanie aplikacji w środowisku przemieszczania. Zobacz [jak wdrażać aplikacje do zdalnego klastra przy użyciu programu Visual Studio](service-fabric-publish-app-remote-cluster.md) informacje na temat.
* Zrozumienie wzorca użycia aplikacji. Te informacje jest używane do symulowania wzorca obciążenia.
* Dowiedz się, cel dla testów obciążenia. Dzięki temu można zinterpretować i analizowanie wyników testów obciążenia.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Tworzenie i uruchamianie projektu wydajności sieci Web i testów obciążenia
### <a name="create-a-web-performance-and-load-test-project"></a>Tworzenie projektu wydajności sieci Web i testów obciążenia
1. Otwórz program Visual Studio 2015. Wybierz **pliku** > **nowy** > **projektu** na pasku menu, aby otworzyć **nowy projekt** okno dialogowe.
2. Rozwiń węzeł **Visual C#** węzeł i wybierz polecenie **testu** > **wydajności sieci Web i testów obciążenia projektu**. Nadaj nazwę projektu, a następnie wybierz pozycję **OK** przycisku.

    ![Zrzut ekranu: okno dialogowe Nowy projekt][1]

    Powinien zostać wyświetlony nowy projekt wydajności sieci Web i testów obciążenia w Eksploratorze rozwiązań.

    ![Zrzut ekranu przedstawiający Eksploratora rozwiązań przedstawiający nowy projekt][2]

### <a name="record-a-web-performance-test"></a>Rekord testu wydajności sieci web
1. Otwórz projekt .webtest.
2. Wybierz **dodać rejestrowania** ikonę, aby rozpocząć sesję rejestrowania w przeglądarce.

    ![Zrzut ekranu: Dodawanie nagrania ikony w przeglądarce][3]

    ![Zrzut ekranu przedstawiający przycisk rekordu w przeglądarce][4]
3. Przejdź do aplikacji sieci szkieletowej usług. Panel rejestrowania powinny być wyświetlane żądania sieci web.

    ![Zrzut ekranu przedstawiający żądań sieci web w panelu rejestrowania][5]
4. Wykonaj sekwencję akcji, które użytkownicy do wykonania. Te akcje są używane jako wzorzec do generowania obciążenia.
5. Gdy wszystko będzie gotowe, wybierz pozycję **zatrzymać** przycisk, aby zatrzymać rejestrowanie.

    ![Zrzut ekranu przedstawiający przycisk zatrzymywania][6]

    Seria żądań powinien przechwycone .webtest projektu w programie Visual Studio. Parametry dynamiczne są zastępowane automatycznie. W tym momencie można usunąć wszelkie zależności dodatkowe, powtarzane żądania nie są częścią Twojego scenariusza testu.
6. Zapisz projekt, a następnie wybierz pozycję **Uruchom Test** polecenia testu wydajności sieci web lokalnie i upewnij się, że wszystko działa prawidłowo.

    ![Zrzut ekranu przedstawiający polecenie uruchomienia testu][7]

### <a name="parameterize-the-web-performance-test"></a>Parametryzacja testu wydajności sieci web
Parametryzacja z testów wydajności sieci web, konwersją do testu wydajności sieci web kodowane, a następnie edytując kod. Alternatywnie można powiązać testu wydajności sieci web z listy danych tak, aby testu iteruje danych. Zobacz [Generowanie i Uruchamianie testów wydajności sieci web kodowane](https://msdn.microsoft.com/library/ms182552.aspx) dla testu kodowanego testu szczegółowe informacje o sposobach konwertowania wydajności sieci web. Zobacz [Dodawanie źródła danych do testu wydajności sieci web](https://msdn.microsoft.com/library/ms243142.aspx) informacji o tym, jak wiązanie danych do testu wydajności sieci web.

Na przykład firma Microsoft będzie przekonwertować testu wydajności sieci web kodowanego testu, można zastąpić identyfikator samolotowy generowanym identyfikatorze GUID lub dodawać więcej żądań do wysyłania lotach do różnych lokalizacji.

### <a name="create-a-load-test-project"></a>Tworzenie projektu testu obciążenia
Projekt testowy obciążenia składa się z co najmniej jeden z opisanych testu wydajności sieci web i testu jednostkowego, wraz z ustawień testu obciążenia dodatkowych scenariuszy. W poniższej procedurze pokazano, jak tworzenie projektu testu obciążenia:

1. Menu skrótów projektu wydajności sieci Web i testów obciążenia, wybierz **Dodaj** > **testu obciążenia**. W **testu obciążenia** kreatora wybierz **dalej** przycisk, aby skonfigurować ustawienia testu.
2. W **wzorzec obciążenia** sekcji, można wybrać obciążenia stałej użytkownika lub obciążenia krok, który rozpoczyna się od kilku użytkowników i zwiększa użytkowników w czasie.

    Jeśli masz dobrej oszacowanie obciążenie użytkowników i chcesz zobaczyć, jak bieżący system wykonuje, wybierz pozycję **załadować stałej**. Jeśli celem jest, aby dowiedzieć się, czy system wykonuje spójnie przy różnych obciążeniach, wybierz **obciążenia krok**.
3. W **testu mieszanego** wybierz **Dodaj** przycisk, a następnie wybierz test, który chcesz uwzględnić w teście obciążenia. Można użyć **dystrybucji** kolumny, aby określić procent całkowitej testów dla każdego z testów.
4. W **parametrów uruchomieniowych** Określ czas trwania testu obciążenia.

   > [!NOTE]
   > **Iteracje testu** opcja jest dostępna tylko po uruchomieniu testu obciążenia lokalnie za pomocą programu Visual Studio.
   >
   >
5. W **lokalizacji** sekcji **parametrów uruchomieniowych**, określ lokalizację, w których są generowane żądań testu obciążenia. Kreator może monitować logować się do konta usługi Team Services. Zaloguj się, a następnie wybierz lokalizację geograficzną. Gdy wszystko będzie gotowe, wybierz pozycję **Zakończ** przycisku.
6. Po utworzeniu testu obciążenia, otwórz projekt .loadtest i wybierz polecenie bieżący przebieg ustawienia, takie jak **parametrów uruchomieniowych** > **Settings1 Uruchom [Active]**. Spowoduje to otwarcie uruchomieniowych w **właściwości** okna.
7. W **wyniki** sekcji **parametrów uruchomieniowych** okno właściwości **magazynowania szczegółów chronometrażu** powinien mieć ustawienie **Brak** jako wartość domyślną. Zmień tę wartość na **wszystkie szczegóły poszczególnych** Aby uzyskać więcej informacji na temat obciążenia wyników testu. Zobacz [testowania obciążenia](https://www.visualstudio.com/load-testing.aspx) Aby uzyskać więcej informacji na temat nawiązać połączenia z programu Visual Studio Team Services i uruchamianie testu obciążenia.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Uruchamianie testu obciążenia za pomocą programu Visual Studio Team Services
Wybierz **uruchomienia testu obciążenia** polecenie, aby uruchomić uruchomienia testu.

![Zrzut ekranu przedstawiający polecenie uruchomienia testu obciążenia][8]

## <a name="view-and-analyze-the-load-test-results"></a>Wyświetlanie i analizowanie wyników testów obciążenia
Jako obciążenia test realizowany, informacje o wydajności jest wyświetlone na wykresie. Powinny zostać wyświetlone informacje podobne do następujących wykresu.

![Zrzut ekranu przedstawiający wykres wydajności dla wyników testu obciążenia][9]

1. Wybierz **Pobierz raport** łącze w górnej części strony. Po pobraniu raportu, wybierz **wyświetlić raport** przycisku.

    Na **wykres** są widoczne wykresy różnych liczników wydajności. Na **Podsumowanie** karcie są wyświetlane ogólne wyniki testu. **Tabel** karta zawiera całkowitą liczbę testów obciążenia przekazany i nie powiodło się.
2. Wybierz numer łącza na **testu** > **niepowodzenie** i **błędy** > **liczba** kolumn, aby wyświetlić szczegóły błędu.

    **Szczegółów** karcie są wyświetlane wirtualnego testu scenariusza informacji o użytkownikach i niepomyślnych żądań. Może to być przydatne, jeśli test obciążenia zawiera wielu scenariuszy.

Zobacz [analizowanie załadować wyników testu w widoku wykresu analizatora testu obciążenia](https://www.visualstudio.com/load-testing.aspx) Aby uzyskać więcej informacji o wyświetlaniu obciążenia wyniki testu.

## <a name="automate-your-load-test"></a>Automatyzowanie testów obciążenia
Visual Studio Team Services testu obciążenia udostępnia interfejsy API, aby ułatwić zarządzanie testów obciążenia i analiza wyników w ramach konta usługi Team Services. Zobacz [API Rest testowania obciążenia chmury](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) Aby uzyskać więcej informacji.

## <a name="next-steps"></a>Następne kroki
* [Monitorowanie i diagnozowanie usług w Instalatorze programowanie komputera lokalnego](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
