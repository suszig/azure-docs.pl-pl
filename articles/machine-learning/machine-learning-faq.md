<properties
    pageTitle="Azure Machine Learning — często zadawane pytania | Microsoft Azure"
    description="Wprowadzenie do usługi Azure Machine Learning: odpowiedzi na często zadawane pytania dotyczące rozliczeń, możliwości i ograniczeń usługi w chmurze na potrzeby sprawnego modelowania predykcyjnego."
    keywords="machine learning introduction,predictive modeling,what is machine learning"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/18/2016"
    ms.author="garye"/>

# Azure Machine Learning — często zadawane pytania: rozliczenia, możliwości, ograniczenia i pomoc techniczna

Przedstawione tutaj często zadawane pytania dotyczą usługi Azure Machine Learning, która jest usługą w chmurze przeznaczoną do tworzenia modeli predykcyjnych i rozwiązań operacyjnych za pośrednictwem usług sieci Web. Wśród często zadawanych pytań znajdują się pytania dotyczące korzystania z samej usługi, w tym między innymi na temat modelu rozliczeń, możliwości, ograniczeń i pomocy technicznej.

## Pytania ogólne

**Co to jest usługa Azure Machine Learning?**

Azure Machine Learning to w pełni zarządzana usługa, która służy do tworzenia, testowania i obsługi rozwiązań z zakresu analiz predykcyjnych w chmurze oraz zarządzania nimi. Wystarczy tylko przeglądarka, aby się zalogować, przekazać dane i natychmiast rozpocząć eksperymenty z uczeniem maszynowym. Modelowanie predykcyjne metodą „przeciągnij i upuść”, duża paleta modułów i biblioteka szablonów początkowych — wszystko to sprawia, że typowe zadania uczenia maszynowego można wykonywać prosto i szybko.  Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/). Wprowadzenie do uczenia maszynowego obejmujące kluczową terminologię i najważniejsze koncepcje przedstawiono w temacie [Introduction to Azure Machine Learning](machine-learning-what-is-machine-learning.md) (Wprowadzenie do usługi Azure Machine Learning).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co to jest Machine Learning Studio?**

Machine Learning Studio to środowisko robocze, do którego można uzyskiwać dostęp za pośrednictwem przeglądarki sieci Web. Usługa Machine Learning Studio udostępnia paletę modułów z interfejsem graficznym do tworzenia, w którym można tworzyć kompletne przepływy pracy do analizy danych w formie eksperymentów.

Aby uzyskać więcej informacji o usłudze Machine Learning Studio, zobacz [What is Machine Learning Studio?](machine-learning-what-is-ml-studio.md) (Co to jest Machine Learning Studio?).

**Co to jest usługa Machine Learning API?**

Usługa Machine Learning API umożliwia wdrażanie modeli predykcyjnych, na przykład tworzonych w środowisku usługi Machine Learning Studio, jako skalowalnych i odpornych na błędy usług sieci Web. Usługi sieci Web utworzone przez usługę Machine Learning API są interfejsami API REST, które zapewniają interfejs do komunikacji między aplikacjami zewnętrznymi i modelami analizy predykcyjnej.

Aby uzyskać więcej informacji, zobacz [Connect to a Machine Learning web service](machine-learning-connect-to-azure-machine-learning-web-service.md) (Łączenie się z usługą sieci web usługi Machine Learning).


## Pytania dotyczące rozliczeń

**Jak działa rozliczanie w usłudze Machine Learning?**

Informacje o rozliczeniach i cenach zawiera temat [Machine Learning — cennik](https://azure.microsoft.com/pricing/details/machine-learning/).

**Czy istnieje bezpłatna wersja próbna usługi Machine Learning?**

 Gdy zarejestrujesz się w celu uzyskania bezpłatnej wersji próbnej platformy Azure, możesz przez miesiąc próbować dowolnych usług Azure. Aby dowiedzieć się więcej na temat bezpłatnej wersji próbnej platformy Azure, odwiedź stronę [Bezpłatna wersja próbna platformy Azure — często zadawane pytania](/pricing/free-trial-faq/).

## Pytania dotyczące usługi Machine Learning Studio

### Tworzenie eksperymentu

**Czy jest dostępna kontrola wersji lub integracja z usługą Git dla wykresów eksperymentów?**

Nie. Jednak usługa Machine Learning Studio zachowuje każdą iterację eksperymentu, która nie może zostać zmodyfikowana przez innych użytkowników.
Aby uzyskać więcej informacji, zobacz temat [Manage experiment iterations in Machine Learning Studio](machine-learning-manage-experiment-iterations.md) (Zarządzanie iteracjami eksperymentów w usłudze Machine Learning Studio).

### Importowanie i eksportowanie danych na potrzeby usługi Machine Learning

**Jakie źródła danych obsługuje usługa Machine Learning?**

Dane mogą być ładowane do usługi Machine Learning Studio na trzy sposoby: poprzez przekazanie pliku lokalnego jako zestawu danych, użycie modułu w celu zaimportowania danych z usług danych w chmurze albo zaimportowanie zestawu danych zapisanego z innego eksperymentu. Aby dowiedzieć się więcej na temat obsługiwanych formatów plików, zobacz temat [Import training data into Machine Learning Studio](machine-learning-data-science-import-data.md) (Importowanie danych szkoleniowych do środowiska usługi Machine Learning Studio).


#### <a id="ModuleLimit"></a>Jak duży może być zestaw danych dla moich modułów?

W typowych przypadkach użycia moduły w usłudze Machine Learning Studio obsługują zestawy danych o rozmiarze maksymalnie 10 GB, zawierające gęsto upakowane dane liczbowe. Jeśli moduł przyjmuje więcej niż jedną operację wprowadzania danych wejściowych, wówczas 10 GB to łączny rozmiar wszystkich danych wejściowych. Większe zestawy danych można przed pozyskaniem próbkować przy użyciu programu Hive lub zapytań usługi Azure SQL Database albo stosując przetwarzanie wstępne metodą uczenia przez liczenie.  

Podczas normalizacji funkcji następujące typy danych mogą ulegać rozszerzaniu do większych zestawów danych. Takie dane muszą być mniejsze niż 10 GB:

- Rozrzedzone
- Podzielone na kategorie
- Ciągi
- Dane binarne

W przypadku następujących modułów obowiązuje ograniczenie do zestawów danych mniejszych niż 10 GB:

- Moduły polecania
- Moduł SMOTE
- Moduły skryptów: R, Python, SQL
- Moduły, w których rozmiar danych wyjściowych może być większy niż rozmiar danych wejściowych, na przykład Przyłączenie lub Tworzenie skrótu funkcji.
- Krzyżowa weryfikacja, Hiperparametry modelu strojenia, Regresja porządkowa oraz Multiklasa Jedna kontra wszystkie, gdy liczba iteracji jest bardzo duża.

W przypadku zestawów danych o rozmiarach większych niż kilka GB należy przekazać dane do magazynu Azure lub usługi Azure SQL Database albo użyć usługi HDInsight zamiast przekazywać dane bezpośrednio z pliku lokalnego.


####<a id="UploadLimit"></a>Jakie są ograniczenia przekazywania danych?
W przypadku zestawów danych o rozmiarach większych niż kilka GB dane należy przekazać do magazynu Azure lub usługi Azure SQL Database albo użyć usługi HDInsight zamiast przekazywać dane bezpośrednio z pliku lokalnego.

**Czy mogę odczytywać dane z usługi Amazon S3?**

Jeśli masz niewielką ilość danych i chcesz je ujawnić za pośrednictwem adresu URL http, możesz użyć modułu [Import danych][import-data]. W przypadku większych ilości danych należy najpierw przenieść dane do magazynu Azure, a następnie użyć modułu [Import danych][import-data], aby wprowadzić dane do eksperymentu.
<!--
<SEE CLOUD DS PROCESS>
-->

**Czy istnieje wbudowana możliwość wprowadzania obrazów?**

Informacje na temat możliwości wprowadzania obrazu są dostępne w temacie [Import Images][image-reader] (Import obrazów).

### Moduły

**W środowisku uslugi Azure Machine Learning Studio nie ma poszukiwanego przeze mnie algorytmu, źródła danych, formatu danych lub szukanej operacji transformacji danych. Jakie są moje opcje?**

Możesz odwiedzić [forum opinii użytkowników](http://go.microsoft.com/fwlink/?LinkId=404231), aby zobaczyć żądania funkcji, które śledzimy. Jeśli pojawiło się już żądanie dotyczące możliwości, której poszukujesz, zagłosuj na to żądanie. Jeśli możliwość, której poszukujesz, nie istnieje, utwórz nowe żądanie. Na tym forum możesz również sprawdzić stan swojego żądania. Ściśle śledzimy tę listę i często aktualizujemy stan dostępności funkcji. Ponadto wbudowana obsługa języków R i Python umożliwia w razie potrzeby tworzenie niestandardowych przekształceń.


**Czy mogę przenieść mój istniejący kod do środowiska usługi Machine Learning Studio?**

Tak. Możesz przenieść istniejący kod w języku R lub Python do środowiska usługi Azure Machine Learning Studio, uruchomić go w tym samym eksperymencie co procesy uczące usługi Azure Machine Learning, a następnie wdrożyć rozwiązanie jako usługę sieci web za pośrednictwem usługi Azure Machine Learning. Aby uzyskać więcej informacji, zobacz tematy [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md) (Rozszerzanie eksperymentu, korzystając z języka R) i [Execute Python machine learning scripts in Azure Machine Learning Studio](machine-learning-execute-python-scripts.md) (Wykonywanie skryptów uczenia maszynowego w języku Python w usłudze Azure Machine Learning Studio).

**Czy w celu definiowania modeli można używać języków, takich jak [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language)?**

Nie. Takie języki nie są obsługiwane. Jednak w celu zdefiniowania modułu można użyć niestandardowego kodu w języku R i języku Python.

**Jak wiele modułów mogę równoległe uruchomić w moim eksperymencie?**  

Równolegle w eksperymencie można uruchomić maksymalnie 4 moduły.


### Przetwarzanie danych

**Czy istnieje możliwość interaktywnej wizualizacji danych (poza wizualizacjami języka R) w ramach eksperymentu?**

Klikając wyjście modułu można zwizualizować dane i uzyskać statystyki.

**Podczas przeglądania wyników lub danych w przeglądarce liczba wierszy i kolumn jest ograniczona. Dlaczego?**

Dane są przesyłane do przeglądarki i mogą być duże, dlatego rozmiar danych jest ograniczony, aby zapobiec spowolnieniu środowiska usługi Machine Learning Studio. W celu zwizualizowania wszystkich danych/wyników lepiej jest pobrać dane i użyć programu Excel lub innego narzędzia.

### Algorytmy

**Jakie istniejące algorytmy są obsługiwane w środowisku usługi Machine Learning Studio?**

Usługa Machine Learning Studio udostępnia najnowocześniejsze algorytmy, takie jak skalowalne wzmocnione drzewa decyzyjne, bayesowskie systemy rekomendacji, głębokie sieci neuronowe i dżungle decyzyjne opracowywane w dziale badań firmy Microsoft. Dostępne są również skalowalne pakiety uczenia maszynowego typu open source, takie jak Vowpal Wabbit. Usługa Machine Learning Studio obsługuje algorytmy uczenia maszynowego na potrzeby binarnej i wieloklasowej klasyfikacji i regresji oraz klastrowania. Zobacz pełną listę [modułów usługi Machine Learning][machine-learning-modules].

**Czy algorytm uczenia maszynowego właściwy dla moich danych jest sugerowany automatycznie?**

Nie. Jednak w usłudze Machine Learning Studio istnieje wiele sposobów porównywania wyników poszczególnych algorytmów w celu wyznaczenia algorytmu właściwego dla danego problemu.

**Czy dostępne są jakiekolwiek wskazówki dotyczące wyboru jednego udostępnionego algorytmu zamiast innego?**
Zobacz [How to choose an algorithm](machine-learning-algorithm-choice.md) (Jak wybrać algorytm).

**Czy udostępnione algorytmy są napisane w języku R lub Python?**

Nie. Te algorytmy są w większości napisane w językach kompilowanych w celu zapewnienia większej wydajności.

**Czy dostępne są jakiekolwiek szczegóły dotyczące udostępnionych algorytmów?**

W dokumentacji udostępniono pewne informacje o algorytmach i opisano parametry przeznaczone do strojenia w celu optymalizacji algorytmu do użycia.  

**Czy są dostępne jakieś rozwiązania do uczenia online?**

Nie. Obecnie jest obsługiwane tylko programowe ponowne trenowanie.

**Czy mogę zwizualizować warstwy modelu sieci neuronowej z użyciem modułu wbudowanego?**

Nie.

**Czy mogę tworzyć własne moduły w języku C# lub innym?**

Obecnie nowe niestandardowe moduły można tworzyć tylko w języku R.

### Moduł R

**Jakie pakiety języka R są dostępne w usłudze Machine Learning Studio?**

Usługa Machine Learning Studio obsługuje obecnie ponad 400 pakietów języka R z sieci CRAN, a tutaj jest [aktualna lista](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) wszystkich dostępnych pakietów. Zobacz też temat [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md) (Rozszerz swój eksperyment, korzystając z języka R), aby dowiedzieć się, jak pobrać tę listę samodzielnie. Jeśli żądanego pakietu nie ma na tej liście, należy podać nazwę pakietu na [forum opinii użytkowników](http://go.microsoft.com/fwlink/?LinkId=404231).

**Czy jest możliwe utworzenie niestandardowego modułu R?**

Tak. Zobacz temat [Author custom R modules in Azure Machine Learning](machine-learning-custom-r-modules.md) (Tworzenie niestandardowych modułów R w usłudze Azure Machine Learning).

**Czy dostępne jest środowisko REPL dla języka R?**

Nie. W studio nie ma środowiska REPL dla języka R.

### Moduł Python

**Czy jest możliwe utworzenie niestandardowego modułu Python?**

Aktualnie nie, ale można użyć dowolnej liczby modułów [Wykonanie skryptu Python][python] w celu uzyskania tego samego rezultatu.

**Czy dostępne jest środowisko REPL dla języka Python?**

W usłudze Machine Learning Studio można skorzystać z notesów Jupyter. Aby uzyskać więcej informacji, zobacz [Introducing Jupyter Notebooks in Azure Machine Learning Studio] (Wprowadzenie do notesów Jupyter w usłudze Azure Machine Learning Studio) (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).

## Usługa sieci Web

###Programowe ponowne trenowanie modeli

**W jaki sposób ponownie trenować modele usługi Azure Machine Learning programowo?**

Użyj interfejsów API do ponownego trenowania. Aby uzyskać więcej informacji, zobacz temat [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) (Ponowne trenowanie modeli uczenia maszynowego programowo). Przykładowy kod jest również dostępny w [demonstracji ponownego trenowania w usłudze Microsoft Azure Machine Learning](https://azuremlretrain.codeplex.com/).

### Tworzenie

**Czy mogę wdrożyć model lokalnie lub w aplikacji bez połączenia z Internetem?**

Nie.


**Czy istnieje opóźnienie bazowe oczekiwane dla wszystkich usługi sieci Web?**

Zobacz [limity subskrypcji platformy Azure](../azure-subscription-service-limits.md)

### Użycie

**Kiedy należy uruchomić model predykcyjny jako usługę wykonywania wsadowego, a kiedy jako usługę odpowiedzi na żądanie?**

Usługa odpowiedzi na żądanie (RRS, Request Response Service) to usługa sieci Web o małym opóźnieniu i dużej skali. Zapewnia interfejs do modeli bezstanowych, które są tworzone i wdrażane ze środowiska, w którym wykonywane są eksperymenty. Usługa wykonywania wsadowego (BES, Batch Execution Service) jest usługą przeznaczoną do asynchronicznego oceniania partii rekordów danych. Dane wejściowe dla usługi BES są podobne do danych wejściowych używanych w usłudze RRS. Główna różnica polega na tym, że usługa BES odczytuje blok rekordów z różnych źródeł, takich jak usługa Blob i usługa tabel na platformie Azure, usługa Azure SQL Database, usługa HDInsight (zapytanie Hive) i źródła HTTP. Aby uzyskać więcej informacji, zobacz [How to consume Machine Learning web services](machine-learning-consume-web-services.md) (Jak korzystać z usług sieci Web usługi Machine Learning).

**Jak zaktualizować model dla wdrożonej usługi sieci Web?**

Aby zaktualizować model predykcyjny dla już wdrożonej usługi wystarczy zmodyfikować i ponownie wykonać eksperyment, którego użyto w celu utworzenia i zapisania trenowanego modelu. Gdy dostępna jest nowa wersja trenowanego modelu, usługa Machine Learning Studio zapyta, czy wymagana jest aktualizacja usługi sieci Web. Szczegółowe informacje na temat aktualizowania wdrożonej usługi sieci Web zawiera temat [Deploy a Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md) (Wdrażanie usługi sieci Web usługi Machine Learning).

Można również użyć interfejsów API do ponownego trenowania.
Aby uzyskać więcej informacji, zobacz temat [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md) (Ponowne trenowanie modeli uczenia maszynowego programowo). Przykładowy kod jest również dostępny w [demonstracji ponownego trenowania w usłudze Microsoft Azure Machine Learning](https://azuremlretrain.codeplex.com/).

**Jak mogę monitorować moją usługę sieci Web wdrożoną w środowisku produkcyjnym?**

Po wdrożeniu modelu predykcyjnego można go monitorować przy użyciu klasycznego portalu Azure. Dla każdej wdrożonej usługi istnieje osobny pulpit nawigacyjny, w którym dostępne są informacje pozwalające na monitorowanie tej usługi.

**Czy jest jakieś miejsce, w którym mogę zobaczyć dane wyjściowe moich usług RRS/BES?**

W przypadku usługi RRS wynik jest zwykle widoczny w odpowiedzi usługi sieci Web. Można go także zapisać do magazynu obiektów blob platformy Azure. W przypadku usługi BES dane wyjściowe są domyślnie zapisywane do obiektu blob. Dane wyjściowe można również zapisać do bazy danych lub tabeli, używając modułu [Eksport danych][export-data].

**Czy usługi sieci Web można tworzyć tylko z modeli utworzonych w usłudze Machine Learning Studio?**

Nie. Usługi sieci Web można również tworzyć bezpośrednio z notesów Jupyter i programu RStudio.

**Gdzie można znaleźć informacji o kodach błędów?**

Listę kodów błędów i opisy zawiera temat [Machine Learning Module Error Codes](https://msdn.microsoft.com/library/azure/dn905910.aspx) (Kody błędów modułów usługi Machine Learning).

## Skalowalność

**Co to jest skalowalność usługi sieci Web?**

Aktualnie każdemu domyślnemu punktowi końcowemu przydzielone jest 20 równoczesnych żądań RRS. Tę wartość można skalować do 200 równoczesnych żądań na punkt końcowy, a każdą usługę sieci Web można skalować do 10 000 punktów końcowych, zgodnie z opisem w temacie [Scaling API endpoints](machine-learning-scaling-endpoints.md) (Skalowanie punktów końcowych interfejsu API). W przypadku usługi BES każdy punkt końcowy pozwala na przetwarzanie 40 żądań jednocześnie, a dodatkowe żądania (po przekroczeniu 40 żądań) są dodawane do kolejki. Żądania z kolejki są wykonywane automatycznie w miarę opróżniania kolejki.


**Czy zadania R są dystrybuowane między węzłami?**

Nie.  


**Ile danych mogę użyć do trenowania?**

W typowych przypadkach użycia moduły w usłudze Machine Learning Studio obsługują zestawy danych o rozmiarze maksymalnie 10 GB, zawierające gęsto upakowane dane liczbowe. Jeśli moduł przyjmuje więcej niż jeden zestaw danych wejściowych, wówczas łączny rozmiar wszystkich danych wejściowych wynosi 10 GB. Większe zestawy danych można przed pozyskaniem próbkować przy użyciu programu Hive lub zapytań usługi Azure SQL Database albo stosując przetwarzanie wstępne za pomocą modułów [Uczenie przy użyciu liczenia][counts].  

Podczas normalizacji funkcji następujące typy danych mogą ulegać rozszerzaniu do większych zestawów danych. Takie dane muszą być mniejsze niż 10 GB:

- rozrzedzone
- podzielone na kategorie
- ciągi
- dane binarne

W przypadku następujących modułów obowiązuje ograniczenie do zestawów danych mniejszych niż 10 GB:

- Moduły polecania
- Moduł SMOTE
- Moduły skryptów: R, Python, SQL
- Moduły, w których rozmiar danych wyjściowych może być większy niż rozmiar danych wejściowych, na przykład Przyłączenie lub Tworzenie skrótu funkcji.
- Krzyżowa weryfikacja, Hiperparametry modelu strojenia, Regresja porządkowa oraz Multiklasa Jedna kontra wszystkie, gdy liczba iteracji jest bardzo duża.

W przypadku zestawów danych o rozmiarach większych niż kilka GB należy przekazać dane do magazynu Azure lub usługi Azure SQL Database albo użyć usługi HDInsight zamiast przekazywać dane bezpośrednio z pliku lokalnego.


**Czy istnieją jakiekolwiek ograniczenia dotyczące rozmiaru wektora?**

Wiersze i kolumny są ograniczone zgodnie z ograniczeniem .NET dla maksymalnej liczby całkowitej: 2 147 483 647.

**Czy można dostosować rozmiar maszyny wirtualnej używanej do uruchamiania usługi sieci Web?**

Nie.  

## Bezpieczeństwo i dostępność

**Kto domyślnie ma dostęp do punktu końcowego HTTP usługi sieci Web? Jak ograniczyć dostęp do tego punktu końcowego?**

Po wdrożeniu usługi sieci Web dla tej usługi tworzony jest domyślny punkt końcowy. Domyślny punkt końcowy może być wywoływany przy użyciu właściwego dla niego klucza interfejsu API. Dodatkowe punkty końcowe można dodać z właściwymi dla nich kluczami z klasycznego portalu Azure albo programowo z użyciem interfejsów API zarządzania usługami sieci Web. W celu wykonywania wywołań do usługi sieci Web potrzebne są klucze dostępu. Aby uzyskać więcej informacji, zobacz [Connect to a Machine Learning web service](machine-learning-connect-to-azure-machine-learning-web-service.md) (Łączenie się z usługą sieci web usługi Machine Learning).


**Co się stanie, jeśli nie można odnaleźć konta magazynu Azure?**

Usługa Machine Learning Studio jest oparta na udostępnionym przez użytkownika koncie magazynu Azure, w którym zapisywane są dane pośrednie podczas wykonywania przepływu pracy. To konto magazynu jest udostępniane na rzecz środowiska usługi Machine Learning Studio w momencie utworzenia obszaru roboczego. Jeśli po utworzeniu obszaru roboczego konto magazynu zostanie usunięte i nie można go znaleźć, obszar roboczy przestanie działać, a wszystkie eksperymenty w tym obszarze zakończą się niepowodzeniem.

Jeśli konto magazynu zostanie przypadkowo usunięte, jedynym sposobem na powrót do poprzedniego stanu jest odtworzenie konta magazynu z tą samą nazwą w tym samym regionie, co usunięte konto magazynu. Następnie należy ponownie zsynchronizować klucz dostępu.


**Co się dzieje, gdy mój klucz dostępu do konta magazynu nie jest zsynchronizowany?**

Usługa Machine Learning Studio jest oparta na udostępnionym przez użytkownika koncie magazynu Azure, w którym zapisywane są dane pośrednie podczas wykonywania przepływu pracy. To konto magazynu jest udostępniane na rzecz środowiska usługi Machine Learning Studio w momencie utworzenia obszaru roboczego, a klucz dostępu jest skojarzony z tym obszarem roboczym. Jeśli po utworzeniu obszaru roboczego klucze dostępu zostaną zmienione, obszar roboczy nie będzie mógł uzyskać dostępu do konta magazynu — w rezultacie przestanie działać, a wszystkie eksperymenty w tym obszarze roboczym zakończą się niepowodzeniem.

Jeśli doszło do zmiany kluczy dostępu do konta magazynu, należy ponownie zsynchronizować klucze dostępu w obszarze roboczym, korzystając z klasycznego portalu Azure.  


## Azure Marketplace

Zobacz [często zadawane pytania dotyczące publikowania i używania aplikacji w portalu Marketplace usługi Machine Learning](machine-learning-marketplace-faq.md).

## Pomoc techniczna i szkolenia

**Gdzie można znaleźć szkolenia dotyczące usługi Azure Machine Learning?**

[Centrum dokumentacji usługi Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) udostępnia samouczki wideo, a także przewodniki dotyczące wykonywania określonych zadań. Te przewodniki krok po kroku stanowią wprowadzenie do usług i przedstawiają cykl życia analizy danych, który obejmuje importowanie danych, czyszczenie danych, budowanie modeli predykcyjnych i wdrażanie ich w środowisku produkcyjnym z użyciem usługi Azure Machine Learning.

Będziemy stale wzbogacać centrum uczenia maszynowego o nowe materiały. Żądania dotyczące dodatkowych materiałów szkoleniowych w centrum uczenia maszynowego można zamieszczać na [forum opinii użytkowników](https://windowsazure.uservoice.com/forums/257792-machine-learning).

Szkolenia można również znaleźć w witrynie [Microsoft Virtual Academy](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning).

**Jak uzyskać pomoc techniczną dotyczącą usługi Azure Machine Learning?**

Aby uzyskać pomoc techniczną dotyczącą usługi Azure Machine Learning, przejdź do witryny [Pomoc techniczna platformy Azure](/support/options/) i wybierz pozycję **Machine Learning**.

Dla usługi Azure Machine Learning udostępniono również forum społeczności w witrynie MSDN, gdzie można zadawać pytania dotyczące usługi Azure Machine Learning. Forum jest monitorowane przez zespół ds. usługi Azure Machine Learning. Odwiedź [Forum platformy Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning).


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx



<!--HONumber=Jun16_HO2-->


