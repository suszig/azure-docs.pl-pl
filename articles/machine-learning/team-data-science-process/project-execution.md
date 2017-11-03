---
title: "Wykonanie danych nauki projektów - Azure | Dokumentacja firmy Microsoft"
description: "Jak naukowca danych można wykonać projektu analizy danych w trackable, niepoddany kontroli wersji, a także sposób współpracy."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: bradsev;
ms.openlocfilehash: 8c318f87243d0c98b6a42bebcdffb433f9cc456e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="execution-of-data-science-projects"></a>Wykonanie projektów analizy danych

W tym dokumencie opisano, jak naukowca danych można wykonać projektu analizy danych w systematyczne, niepoddany kontroli wersji i sposób współpracy w zespole projektu za pomocą [proces nauki danych zespołu](overview.md) (TDSP). TDSP jest opracowanym przez firmę Microsoft, zapewniająca strukturalnych sekwencji działań do wykonania wydajne rozwiązania oparte na chmurze analizy predykcyjnej. Aby schematem ról pracowników i ich skojarzonych zadań, które są obsługiwane przez zespół nauki danych standaryzacji w tym procesie, zobacz [proces nauki danych zespołu ról i zadań](roles-tasks.md). 

Ten temat zawiera instrukcje dotyczące sposobu: 

1. czy **planowanie przebiegu** dla elementów roboczych związane z projektem.<br> Jeśli użytkownik nie zna planowanie przebiegu, szczegóły poniżej i ogólne informacje można znaleźć [tutaj](https://en.wikipedia.org/wiki/Sprint_(software_development) "tutaj"). 
2. **Dodawanie elementów roboczych** do przebiegów.
3. **połączyć elementy robocze z kodowania działania** śledzone przez git.
4. czy **Przegląd kodu**. 


>[AZURE.NOTE] Firma Microsoft przedstawiają kroki niezbędne do konfigurowania środowiska zespołu TDSP przy użyciu programu Visual Studio Team Services (VSTS) w następujący zestaw instrukcji. Określono sposobu wykonywania tych zadań z programu VSTS, ponieważ sposób wprowadzania TDSP firmy Microsoft. Elementy [3] i [4] na poprzedniej liście są korzyści, które można uzyskać naturalnie, jeśli chcesz użyć programu VSTS. Jeśli inny kod obsługujący platformy jest używany dla tej grupy, zadania, które muszą zostać wykonane przez realizacji zespołu zwykle nie należy zmieniać. Jednak sposób do wykonania tych zadań ma być różne. Na przykład element w sekcji sześciu, **Link elementu roboczego z gałęzi git**, nie może być łatwym znajduje się na usługi VSTS.

Na poniższym rysunku przedstawiono typowe przebiegu planowania, kodowanie i przepływu pracy z kontroli źródła wymagane w przypadku implementowania projektu nauki danych:

![1](./media/project-execution/project-execution-1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminologia 

Wypalenie w ramach planowania w TDSP, istnieją cztery typy często używanych **elementy robocze**: **funkcji**, **scenariusza**, **zadań**, i **Usterki**. Każdego projektu zespołowego obsługuje pojedynczy zaległości dla wszystkich elementów roboczych. Nie występuje Zaległość na poziomie repozytorium git w projekcie zespołowym. Oto ich definicje:

- **Funkcja**: funkcja odpowiada zaangażowania projektu. Różne korzystają z klientem są traktowane jako różne funkcje. Podobnie warto wziąć pod uwagę różne fazy projektu za pomocą klienta jako różne funkcje. Jeśli wybierzesz takie jak schemat ***ClientName EngagementName*** nazwę funkcje, następnie możesz łatwo rozpoznać kontekstu projektu/zaangażowania z samych nazw.
- **Wątek**: wątki są elementy robocze różnych, które są niezbędne do ukończenia funkcji (projekt) end-to-end. Przykłady scenariuszy:
    - Pobieranie danych 
    - Eksplorowanie danych 
    - Generowanie funkcji
    - Tworzenie modeli
    - Modele operacyjnych 
    - Ponownego trenowania modeli
- **Zadanie**: zadania są elementy pracy można przypisać kodu lub dokument lub innych działań, które należy wykonać w celu przeprowadzenia określonego wątku. Na przykład zadania w wątku *pobierania danych* można:
    -  Uzyskiwanie poświadczeń programu SQL Server 
    -  Przekazywanie danych do usługi SQL Data Warehouse. 
- **Błąd**: błędy zazwyczaj odwoływać się do poprawki, które są potrzebne dla istniejącego kodu lub które są wykonywane po zakończeniu zadania. Może on eskalować obecności wątku lub zadanie, jeśli ten błąd jest spowodowany przez odpowiednio Brak etapów lub zadania. 

>[AZURE.NOTE] Firma Microsoft finansowania zewnętrznego pojęcia, funkcje, wątki, zadań i błędów z zarządzania kodu oprogramowania (SCM), który ma być używany podczas analizy danych. One może różnić się nieznacznie od ich z konwencjonalnej definicje SCM.

##  2. <a name='SprintPlanning-2'></a>Planowanie przebiegu 

Planowanie przebiegu jest przydatne w przypadku priorytetów projektu i planowania zasobów i alokacji. Prowadzi wielu analityków danych z wielu projektów, z których każdy może zająć miesięcy, aby zakończyć. Projekty często realizowane w różnych nadać tempo. Na serwerze programu VSTS można łatwo utworzyć, zarządzać i śledzenia elementów roboczych w projekcie zespołowym i przeprowadzić przebiegu planowania zapewnienie przechodzenia do przodu, zgodnie z oczekiwaniami projektów. 

Postępuj zgodnie z [to łącze](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) instrukcje krok po kroku w przebiegu planowania w VSTS. 


##  3. <a name='AddFeature-3'></a>Dodawanie funkcji  

Po utworzeniu repozytorium projektu w projekcie zespołowym, przejdź do zespołu **omówienie** i kliknij przycisk **zarządzać pracą**.

![2](./media/project-execution/project-execution-2-sprint-team-overview.png)

Aby dołączyć funkcji zaległości, kliknij przycisk **zaległości** --> **funkcje** --> **nowy**, typ funkcji **tytuł**(zazwyczaj nazwę projektu), a następnie kliknij przycisk **Dodaj** .

![3](./media/project-execution/project-execution-3-sprint-team-add-work.png)

Kliknij dwukrotnie funkcja, do której został utworzony. Wypełnij opisy, przypisz członków zespołu dla tej funkcji i ustaw planowania parametrów dla tej funkcji. 

Ta funkcja może także połączyć z repozytorium projektu. Kliknij przycisk **Dodaj** w obszarze **programowanie** sekcji. Po zakończeniu edycji funkcji, kliknij przycisk **Zapisz i Zamknij** aby zakończyć.


##  4. <a name='AddStoryunderfeature-4'></a>Dodaj wątku w funkcji 

W funkcji można dodać wątków do opisano najważniejsze czynności wymagane do zakończenia projektu (funkcja). Aby dodać nowy artykuł, kliknij przycisk  **+**  Zaloguj się na lewo od funkcji w widoku listy prac.  

![4](./media/project-execution/project-execution-4-sprint-add-story.png)

Można edytować szczegóły wątku, takie jak stan, opis, komentarze, planowania i priorytetu w oknie podręcznym.

![5](./media/project-execution/project-execution-5-sprint-edit-story.png)

Ten artykuł można połączyć istniejące repozytorium, klikając **+ Dodaj** w obszarze **programowanie**. 

![6](./media/project-execution/project-execution-6-sprint-link-existing-branch.png)


##  5. <a name='AddTaskunderstory-5'></a>Dodaj zadanie do wątku 

Zadania są określone szczegółowy opis kroków, które są niezbędne do ukończenia każdego scenariusza. Po zakończeniu wszystkich zadań wątku zbyt należy wypełnić wątku. 

Można dodać zadania do artykułu, kliknij przycisk  **+**  znak obok elementu scenariusza, wybierz opcję **zadań**, a następnie wypełnij szczegółowe informacje tego zadania w wyskakującym oknie.

![7](./media/project-execution/project-execution-7-sprint-add-task.png)

Po utworzeniu funkcji, scenariuszy i zadań, można wyświetlić je w **zaległości** lub **tablicy** widoków, które można śledzić jego stan.

![8](./media/project-execution/project-execution-8-sprint-backlog-view.png)

![9](./media/project-execution/project-execution-9-link-to-a-new-branch.png)


##  6. <a name='Linkaworkitemwithagitbranch-6'></a>Łączenie elementu roboczego z gałęzi git 

VSTS oferują wygodny sposób nawiązywania połączenia elementu pracy (wątku lub zadania) z gałęzi git. Dzięki temu można połączyć z wątku lub zadanie bezpośrednio do kodu skojarzonego z nim. 

Połącz się nowa gałąź z elementu roboczego, kliknij dwukrotnie element roboczy, a w wyskakującym oknie kliknij **utworzyć nową gałąź** w obszarze **+ Dodaj**.  

![10](./media/project-execution/project-execution-10-sprint-board-view.png)

Podaj informacje dotyczące nowej gałęzi, takie jak nazwa gałęzi, repozytorium git podstawowej i oddziału. Repozytorium git wybrany musi być repozytorium w ramach tego samego projektu zespołowego, należącego do elementu roboczego. Podstawowy gałęzi może być gałęzi głównej lub istniejących gałęzi.

![11](./media/project-execution/project-execution-11-create-a-branch.png)

Dobrym rozwiązaniem jest tworzenie gałęzi git dla każdego elementu roboczego wątku. Następnie dla każdego elementu roboczego zadania, utwórz gałąź oparte na gałąź wątku. Organizowanie gałęzie w ten sposób hierarchiczne, umożliwiająca relacje wątku zadanie jest przydatne, gdy masz wiele osób pracuje różnych scenariuszy tego samego projektu lub ma wiele osób pracuje różne zadania tego samego wątku. Podczas każdego członka zespołu działa w różnych gałęzi i każdy element członkowski działa w różnych kodów lub pozostałych artefaktów, podczas udostępniania gałąź, można zminimalizować konflikty. 

Poniższej ilustracji przedstawiono zalecane strategii rozgałęziania dla TDSP. Nie trzeba wiele gałęzi, jak przedstawiono poniżej, zwłaszcza jeśli masz tylko jedną lub dwie osoby działa na tym samym projekcie lub tylko jedna osoba działa we wszystkich zadaniach wątku. Ale rozdzielających gałęzi programowanie z gałęzi głównej jest zawsze dobrym rozwiązaniem. Może to zapobiec gałęzi wersji przez zadania programistyczne. Bardziej szczegółowy opis modelu gałęzi git znajdują się w [A pomyślnie Git rozgałęzianie modelu](http://nvie.com/posts/a-successful-git-branching-model/).

![12](./media/project-execution/project-execution-12-git-branches.png)

Aby przejść do gałęzi, do której chcesz pracować na, uruchom następujące polecenie w polecenia powłoki (Windows lub Linux). 

    git checkout <branch name>

Zmiana *< Nazwa gałęzi\>*  do **wzorca** przełączników z powrotem do **wzorca** gałęzi. Po przełączeniu do gałęzi roboczej, można rozpocząć pracy na dany element roboczy, tworzenie artefaktów kodu lub dokumentacji, wymaganego do ukończenia elementu. 

Możesz również połączyć elementu roboczego się na istniejącą. W **szczegółów** strony elementu roboczego, zamiast klikać **utworzyć nową gałąź**, możesz kliknąć przycisk **+ Dodaj**. Wybierz gałąź, który chcesz połączyć elementu roboczego do. 

![13](./media/project-execution/project-execution-13-link-to-an-existing-branch.png)

Można również utworzyć nową gałąź w usłudze git bash poleceń. Jeśli < Nazwa gałęzi podstawowej\> brakuje, < Nowa nazwa gałęzi\> opiera się na _wzorca_ gałęzi. 
    
    git checkout -b <new branch name> <base branch name>


##  7. <a name='WorkonaBranchandCommittheChanges-7'></a>Pracy w gałęzi i zatwierdzić zmiany 

Teraz załóżmy, że niektóre zmiany do *danych\_wprowadzanie* gałęzi dla elementu roboczego, takie jak dodanie pliku R w gałęzi w komputerze lokalnym. Można zatwierdzić R pliku dodanego do gałęzi dla tego elementu roboczego, pod warunkiem jesteś w oddziale w powłoce programu Git za pomocą następujących poleceń Git:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![14](./media/project-execution/project-execution-14-sprint-push-to-branch.png)

##  8. <a name='CreateapullrequestonVSTS-8'></a>Utwórz żądanie ściągnięcia na VSTS 

Gdy wszystko będzie gotowe po kilku zatwierdzeniami i wypchnięć, do scalenia bieżącej gałęzi w swojej gałęzi podstawowej, można kierować **żądania ściągnięcia** na serwerze programu VSTS. 

Przejdź do strony głównej projektu zespołowego, a następnie kliknij przycisk **kod**. Wybierz gałąź do scalenia i nazwę repozytorium git do scalenia gałęzi do. Następnie kliknij przycisk **żądania ściągnięcia**, kliknij przycisk **nowe żądanie ściągnięcia** tworzenie Przegląd żądania ściągnięcia, przed scalonego pracy w gałęzi do jego podstawowej gałęzi.

![15](./media/project-execution/project-execution-15-spring-create-pull-request.png)

Wypełnij niektóre opis tego żądania ściągnięcia, dodawać recenzentów i wysłać go.

![16](./media/project-execution/project-execution-16-spring-send-pull-request.png)

##  9. <a name='ReviewandMerge-9'></a>Przejrzyj i dokonać scalania 

Po utworzeniu żądania ściągania recenzentów powiadomienie e-mail do przeglądania żądania ściągnięcia. Recenzentów, należy sprawdzić, czy zmiany pracy lub nie, a jeśli to możliwe testowanie zmian z obiektu żądającego. Na podstawie ich oceny, recenzentów można zatwierdzić lub odrzucić żądanie ściągnięcia. 

![17](./media/project-execution/project-execution-17-add_comments.png)

![18](./media/project-execution/project-execution-18-spring-approve-pullrequest.png)

Po wykonaniu czynności przeglądu gałąź roboczą scalonego jego podstawowej gałęzi, klikając **Complete** przycisku. Możesz usunąć gałąź roboczą, po jego został scalony. 

![19](./media/project-execution/project-execution-19-spring-complete-pullrequest.png)

Upewnij się, w lewym górnym rogu, że żądanie jest oznaczony jako **UKOŃCZONO**. 

![20](./media/project-execution/project-execution-20-spring-merge-pullrequest.png)

Jeśli możesz powrócić do repozytorium, w obszarze **kod**, pojawi się informacja, że użytkownik został przełączony do głównej gałęzi.

![21](./media/project-execution/project-execution-21-spring-branch-deleted.png)

Można także użyć następujących poleceń Git gałąź pracy do swojej gałęzi podstawowej i Usuń gałąź pracy po scaleniu:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![22](./media/project-execution/project-execution-22-spring-branch-deleted-commandline.png)


##  10. <a name='DataQualityReportUtility-10'></a>Eksploracja danych interaktywnych, analizy i raportowania narzędzia (IDEAR)

To narzędzie oparte na języku znaczników markdown R zapewnia elastyczne i interaktywne narzędzia do oceny i Eksploruj zestawów danych. Użytkownicy mogą szybko generować raporty z zestawu danych z minimalnym kodowania. Użytkownicy mogą kliknąć przycisk, aby wyeksportować wyniki eksploracji zauważa interaktywne narzędzia do raportu końcowego, które mogą zostać dostarczone do klientów lub umożliwia podjęcie decyzji, na które zmienne, aby uwzględnić w kroku kolejnych modelowania.

W tej chwili narzędzie działa tylko na ramek danych w pamięci. Plik .yaml jest potrzebne do określania parametrów zestawu danych do eksplorowania. Aby uzyskać więcej informacji, zobacz [IDEAR w narzędzia do analizy danych TDSP](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


##  11. <a name='ModelingUtility-11'></a>Narzędzie raportowania i modelowania linii bazowej

To narzędzie oferuje można dostosowywać, częściowo zautomatyzowanych narzędzia do wykonania parametru funkcji hyper-profilach do tworzenia modelu i porównania dokładność tych modeli. 

Narzędzie tworzenia modelu jest plikiem markdown R, który można uruchomić w celu generowania danych wyjściowych html niezależne ze spisu treści dla nawigację za pośrednictwem jego różne sekcje. Trzy algorytmy są wykonywane po uruchomieniu pliku markdown (Dzianina): umorzyć regresji przy użyciu glmnet pakietu losowe lasu przy użyciu pakietu randomForest i zwiększania drzew przy użyciu pakietu xgboost). Każdy z tych algorytmów tworzy uczonego modelu. Dokładność tych modeli jest następnie porównywany i zgłosił względną funkcji powierzchni znaczenie. Obecnie istnieją dwa narzędzia: jeden jest przeznaczona zadania klasyfikacji binarnej i jeden dla zadania regresji. Podstawowe różnice między nimi jest sposób parametrów sterujących i dokładność metryki są określone dla tych zadań learning. 

Plik yaml programu służy do określania:

- dane wejściowe (źródła SQL lub pliku danych R) 
- jaka część danych służy do szkolenia i jakie części do testowania
- jakie algorytmy do uruchomienia 
- wybór parametrów sterujących optymalizacji modelu:
    - krzyżowe sprawdzanie poprawności 
    - Uruchamianie
    - złożeń krzyżowego sprawdzania poprawności
- Ustawia hyper parametr dla każdej algorytmu. 

Liczba algorytmów, liczba złożeń optymalizacji, hyper parametrów i liczby zestawów hyper parametru do odchylenia za pośrednictwem również może być modyfikowany w pliku yaml programu, aby szybko uruchomić modeli. Na przykład można ich uruchomić z mniejszą liczbę złożeń CV, Zmniejsz liczbę zestawów parametrów. Ich można również uruchomić bardziej kompleksowe z większej liczby złożeń CV lub większej liczby zestawów parametrów, jeśli, który jest uzasadnione.

Aby uzyskać więcej informacji, zobacz [automatycznego modelowania i raportowania narzędzia w narzędzia do analizy danych TDSP](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).


##  12. <a name='PowerBI-12'></a>Śledzenie postępu projektów za pomocą pulpitów nawigacyjnych usługi Power BI

Dane nauki grupy Menedżerowie, zespołu potencjalnych klientów i projektu potencjalnych klientów należy śledzić postęp projektów, jakie pracy zostało wykonane na nich i przez kogo i pozostanie na liście zadań do wykonania. Jeśli używasz programu VSTS jest możliwość tworzenia pulpitów nawigacyjnych usługi Power BI do śledzenia działań i elementów roboczych skojarzonych z repozytorium Git. Aby uzyskać więcej informacji na temat usługi Power BI nawiązać połączenia z programu Visual Studio Team Services, zobacz [Połącz usługi Power BI do usługi Team Services](https://www.visualstudio.com/en-us/docs/report/powerbi/connect-vso-pbi-vs). 

Informacje na temat tworzenia pulpitów nawigacyjnych usługi Power BI i raportów do śledzenia działań repozytorium Git i elementami pracy po podłączeniu dane programu VSTS z usługą Power BI, zobacz [tworzenia usługi Power BI pulpity nawigacyjne i raporty](https://www.visualstudio.com/en-us/docs/report/powerbi/report-on-vso-with-power-bi-vs). 

Poniżej przedstawiono dwa pulpity nawigacyjne prosty przykład, które budujemy śledzenia działań Git i elementów roboczych. W pierwszym przykładzie pulpitu nawigacyjnego działania zobowiązań git są wyświetlane według różnych użytkowników w różnych terminach i na różnych repozytoriów. Użytkownik może łatwo kątami te, które chcesz filtrować.

![23](./media/project-execution/project-execution-23-powerbi-git.png)

W drugim przykładzie pulpitu nawigacyjnego są prezentowane elementów roboczych (wątków i zadań) w różnych iteracji. Są pogrupowane według osób przydzielonych i poziomy priorytetu i pokolorowane według stanu.

![24](./media/project-execution/project-execution-24-powerbi-workitem.png)

 
## <a name="next-steps"></a>Następne kroki

Pełne end-to-end wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) tematu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 

Przykłady wykonywania czynności w procesie nauki zespołu danych korzystających z usługi Azure Machine Learning Studio można znaleźć [z ML Azure](http://aka.ms/datascienceprocess) ścieżkę szkoleniową.