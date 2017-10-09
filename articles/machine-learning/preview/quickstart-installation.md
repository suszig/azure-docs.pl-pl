---
title: "Instalacja — Szybki start dla usług Azure Machine Learning | Microsoft Docs"
description: "Ten przewodnik Szybki start przedstawia sposób tworzenia zasobów usług Azure Machine Learning oraz instalowania aplikacji Azure Machine Learning Workbench."
services: machine-learning
author: hning86
ms.author: haining, raymondl, chhavib
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: hero-article
ms.date: 09/20/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 91d2f47a528050f644973044f96c0354b91dba25
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---

# <a name="create-azure-machine-learning-preview-accounts-and-install-azure-machine-learning-workbench"></a>Tworzenie kont usługi Azure Machine Learning w wersji zapoznawczej i instalowanie aplikacji Azure Machine Learning Workbench
Usługa Azure Machine Learning stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do zaawansowanych analiz przeznaczone dla profesjonalnych analityków. Będą oni korzystać z tego rozwiązania w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Ten przewodnik Szybki start przedstawia sposób tworzenia kont eksperymentowania oraz zarządzania modelem w wersji zapoznawczej usługi Azure Machine Learning. Przedstawia on również sposób instalowania aplikacji klasycznej Azure Machine Learning Workbench oraz narzędzi interfejsu wiersza polecenia. Następnie zapoznasz się z krótką prezentacją funkcji w wersji zapoznawczej usługi Azure Machine Learning przy użyciu ponadczasowego [zbioru danych na temat irysów](https://en.wikipedia.org/wiki/iris_flower_data_set) w celu utworzenia modelu do prognozowania typu irysa opartego na niektórych jego cechach fizycznych.  

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne
Obecnie aplikację Azure Machine Learning Workbench można zainstalować tylko w następujących systemach operacyjnych: Windows 10, Windows Server 2016 i macOS Sierra.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal
Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-azure-machine-learning-accounts"></a>Tworzenie kont usługi Azure Machine Learning
Aprowizuj konta usługi Azure Machine Learning za pomocą witryny Azure Portal. 
1. Wybierz przycisk **Nowy** (+) w lewym górnym rogu portalu.

2. Wpisz ciąg „Machine Learning” na pasku wyszukiwania. Wybierz wynik wyszukiwania o nazwie **Eksperymentowanie w usłudze Machine Learning (wersja zapoznawcza)**.  Kliknij ikonę gwiazdki, aby dodać tę pozycję do ulubionych w swojej witrynie Azure Portal.

   ![Wyszukiwanie w usłudze Azure Machine Learning](media/quickstart-installation/portal-more-services.png)

3. Kliknij przycisk **+ Dodaj**, aby skonfigurować nowe konto Eksperymentowanie w usłudze Machine Learning. Zostanie otwarty formularz szczegółowy.

   ![Konto Eksperymentowanie w usłudze Machine Learning](media/quickstart-installation/portal-create-experimentation.png)

4. Wypełnij formularz Eksperymentowanie w usłudze Machine Learning następującymi informacjami:

   Ustawienie|Sugerowana wartość|Opis
   ---|---|---
   Nazwa konta Eksperymentowanie | _Unikatowa nazwa_ |Wybierz unikatową nazwę, która identyfikuje Twoje konto. Podaj własną nazwę albo nazwę działu lub projektu, która najlepiej identyfikuje eksperyment. Nazwa musi mieć od 2 do 32 znaków i może zawierać tylko znaki alfanumeryczne oraz znak kreski „-”. 
   Subskrypcja | _Twoja subskrypcja_ |Subskrypcja platformy Azure, której chcesz używać do eksperymentu. Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w ramach której są naliczane opłaty za ten zasób.
   Grupa zasobów | _Twoja grupa zasobów_ | Możesz wprowadzić nową nazwę grupy zasobów lub użyć istniejącej nazwy z subskrypcji.
   Lokalizacja | _Region najbliżej Twoich użytkowników_ | Wybierz lokalizację znajdującą się najbliżej Twoich użytkowników i zasobów danych.
   Liczba stanowisk | 2 | Wpisz liczbę stanowisk. Ten wybór ma wpływ na [ceny](https://azure.microsoft.com/pricing/details/machine-learning/). Bezpłatne są dwa pierwsze stanowiska. Na potrzeby tego przewodnika Szybki start użyj dwóch stanowisk. Liczbę stanowisk możesz zaktualizować później zgodnie z potrzebami w witrynie Azure Portal.
   Konto magazynu | _Unikatowa nazwa_ | Wybierz pozycję **Utwórz nowy** i podaj nazwę, aby utworzyć nowe konto magazynu platformy Azure, lub wybierz pozycję **Użyj istniejącego** i wybierz swoje już istniejące konto magazynu z listy rozwijanej. Konto magazynu jest wymagane i jest używane do przechowywania artefaktów projektu oraz danych historii uruchamiania. 
   Obszar roboczy dla konta Eksperymentowanie | _Unikatowa nazwa_ | Określ nazwę dla nowego obszaru roboczego. Nazwa musi mieć od 2 do 32 znaków i może zawierać tylko znaki alfanumeryczne oraz znak kreski „-”.
   Przypisz właściciela obszaru roboczego | _Twoje konto_ | Wybierz własne konto jako właściciela obszaru roboczego.
   Utwórz konto Zarządzanie modelami | *zaznaczone* | W ramach tworzenia konta Eksperymentowanie możesz również utworzyć konto Zarządzanie modelami w usłudze Machine Learning. Ten zasób będzie używany, gdy wszystko będzie gotowe do wdrożenia Twoich modeli i zarządzania nimi jako usługami internetowymi w czasie rzeczywistym. Zaleca się utworzenie konta Zarządzanie modelami jednocześnie z kontem Eksperymentowanie.
   Nazwa konta | _Unikatowa nazwa_ | Wybierz unikatową nazwę, która identyfikuje Twoje konto Zarządzanie modelami. Podaj własną nazwę albo nazwę działu lub projektu, która najlepiej identyfikuje eksperyment. Nazwa musi mieć od 2 do 32 znaków i może zawierać tylko znaki alfanumeryczne oraz znak kreski „-”. 
   Warstwa cenowa Zarządzanie modelami | **DEVTEST** | Kliknij pozycję **Nie wybrano warstwy cenowej**, aby określić warstwę cenową dla nowego konta Zarządzanie modelami. W celu uzyskania oszczędności wybierz warstwę cenową **DEVTEST**, jeśli jest dostępna w ramach Twojej subskrypcji (ograniczona dostępność). W przeciwnym razie wybierz warstwę cenową S1, aby uzyskać oszczędności. Kliknij przycisk **Wybierz**, aby zapisać wybraną warstwę cenową. 
   Przypnij do pulpitu nawigacyjnego | _zaznaczone_ | Zaznacz pole opcji **Przypnij do pulpitu nawigacyjnego**, aby umożliwiać łatwe śledzenie konta Eksperymentowanie w usłudze Machine Learning na pierwszej stronie pulpitu nawigacyjnego witryny Azure Portal.

5. Kliknij przycisk **Utwórz**, aby rozpocząć proces tworzenia.

6. U góry z prawej strony paska narzędzi witryny Azure Portal kliknij pozycję **Powiadomienia** (ikonę dzwonka), aby monitorować proces wdrażania. 

   Powiadomienie zawiera informację „Wdrażanie w toku...”. Po zakończeniu stan zmieni się na „Pomyślnie wdrożono”. W razie powodzenia zostanie otwarta strona Twojego konta Eksperymentowanie w usłudze Machine Learning.
   
   ![Powiadomienia w witrynie Azure Portal](media/quickstart-installation/portal-notification.png)

Teraz, w zależności od systemu operacyjnego używanego na komputerze lokalnym, wykonaj czynności opisane w jednej z dwóch następnych sekcji, aby zainstalować aplikację Azure Machine Learning Workbench na swoim komputerze. 

## <a name="install-azure-machine-learning-workbench-on-windows"></a>Instalowanie aplikacji Azure Machine Learning Workbench w systemie Windows
Zainstaluj aplikację Azure Machine Learning Workbench na swoim komputerze z systemem Windows 10, Windows Server 2016 lub nowszym.

1. Pobierz najnowszą wersję instalatora aplikacji Azure Machine Learning Workbench **[AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi)**.

2. Kliknij dwukrotnie pobrany plik instalatora _AmlWorkbenchSetup.msi_ w Eksploratorze plików.

   >[!IMPORTANT]
   >Pobierz instalatora w całości na dysk, a następnie uruchom go z tego miejsca. Nie uruchamiaj go bezpośrednio ze swojego widżetu pobierania przeglądarki.

3. Zakończ instalację, postępując zgodnie z wyświetlanymi instrukcjami.

   Instalator pobierze wszystkie niezbędne składniki zależne, takie jak Python, Miniconda i inne powiązane biblioteki. Instalacja wszystkich składników może zająć około pół godziny. 

4. Aplikacja Azure Machine Learning Workbench jest teraz zainstalowana w następującym katalogu:
   
   `C:\Users\<user>\AppData\Local\AmlWorkbench`

## <a name="install-azure-machine-learning-workbench-on-macos"></a>Instalowanie aplikacji Azure Machine Learning Workbench w systemie macOS
Zainstaluj aplikację Azure Machine Learning Workbench na swoim komputerze z systemem macOS Sierra.

1. Zainstaluj bibliotekę openssl za pomocą oprogramowania [Homebrew](http://brew.sh). Zobacz [Wymagania wstępne dla platformy .NET Core w systemie Mac](https://docs.microsoft.com/dotnet/core/macos-prerequisites), aby uzyskać więcej szczegółów.
   ```
   # install Homebrew first if you don't have it already
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   # install latest openssl needed for .NET Core 1.x
   brew update
   brew install openssl
   mkdir -p /usr/local/lib
   ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
   ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
   ```

2. Pobierz najnowszą wersję instalatora aplikacji Azure Machine Learning Workbench **[AmlWorkbench.dmg](https://aka.ms/azureml-wb-dmg)**.

   >[!IMPORTANT]
   >Pobierz instalatora w całości na dysk, a następnie uruchom go z tego miejsca. Nie uruchamiaj go bezpośrednio ze swojego widżetu pobierania przeglądarki.

3. Kliknij dwukrotnie pobranego instalatora _AmlWorkbench.dmg_ z programu Finder.

4. Zakończ instalację, postępując zgodnie z wyświetlanymi instrukcjami.

   Instalator pobierze wszystkie niezbędne składniki zależne, takie jak Python, Miniconda i inne powiązane biblioteki. Instalacja wszystkich składników może zająć około pół godziny. 

5. Aplikacja Azure Machine Learning Workbench jest teraz zainstalowana w następującym katalogu: 

   _/Applications/AmlWorkbench.App_

## <a name="run-azure-machine-learning-workbench-to-log-in-the-first-time"></a>Uruchamianie aplikacji Azure Machine Learning Workbench, aby zalogować się po raz pierwszy
1. Kliknij przycisk **Uruchom aplikację Workbench** na ostatnim ekranie instalatora po zakończeniu procesu instalacji. W przypadku zamknięcia instalatora znajdź skrót do aplikacji Azure Machine Learning Workbench na pulpicie lub w menu Start o nazwie **Azure Machine Learning Workbench**, aby uruchomić aplikację.

2. Zaloguj się do aplikacji Workbench przy użyciu tego samego konta, które było wcześniej używane do aprowizacji zasobów platformy Azure. 

3. Po pomyślnym zakończeniu procesu logowania aplikacja Workbench próbuje odnaleźć utworzone wcześniej konta Eksperymentowanie w usłudze Machine Learning. Wyszukuje ona wszystkie subskrypcje platformy Azure, do których Twoje poświadczenie ma dostęp. Po znalezieniu co najmniej jednego konta Eksperymentowanie aplikacja Workbench otwiera się przy użyciu tego konta. Następnie są wyświetlane obszary robocze i projekty znalezione na tym koncie. 

   >[!TIP]
   > Jeśli masz dostęp do więcej niż jednego konta Eksperymentowanie, możesz przełączyć się na inne konto, klikając ikonę awatara w lewym dolnym rogu aplikacji Workbench.

Zobacz [Konfiguracja środowiska wdrażania](deployment-setup-configuration.md), aby uzyskać informacje o tworzeniu środowiska na potrzeby wdrażania usług internetowych.

## <a name="create-a-new-project"></a>Tworzenie nowego projektu
1. Uruchom aplikację Azure ML Workbench i zaloguj się. 

2. Kliknij pozycję **Plik** --> **Nowy projekt** (lub kliknij znak **+** w okienku **PROJEKTY**). 

3. Wypełnij pola **Nazwa projektu** i **Katalog projektu**. Pole **Opis projektu** jest opcjonalne, ale przydatne. Pole **Adres URL repozytorium GIT Visualstudio.com** na razie pozostaw puste. Wybierz obszar roboczy i wybierz pozycję **Klasyfikowanie irysów** jako szablon projektu.

   >[!TIP]
   >Opcjonalnie możesz wpisać w pole tekstowe Repozytorium Git adres URL repozytorium Git hostowanego w projekcie usługi [VSTS (Visual Studio Team Services)](https://www.visualstudio.com). To repozytorium Git musi już istnieć i musi być puste bez gałęzi głównej. Ponadto musisz mieć do niego dostęp do zapisu. Dodanie repozytorium Git teraz umożliwia późniejsze włączenie scenariuszy roamingu i udostępniania. [Dowiedz się więcej](using-git-ml-project.md).

4. Kliknij przycisk **Utwórz**, aby utworzyć projekt. Nowy projekt zostanie utworzony i otwarty. Teraz możesz eksplorować stronę główną projektu, źródła danych, notesy i pliki kodu źródłowego. 

    >[!TIP]
    >Możesz również otworzyć projekt w edytorze VS Code lub innym edytorze, po prostu konfigurując link zintegrowanego środowiska projektowego (IDE), a następnie otwierając w nim katalog projektu. [Dowiedz się więcej](how-to-configure-your-IDE.md). 

## <a name="run-a-python-script"></a>Uruchamianie skryptu w języku Python
Wykonajmy skrypt na Twoim komputerze lokalnym. 

1. Każdy projekt jest otwierany na własnej stronie **pulpitu nawigacyjnego projektu**. Wybierz pozycję `local` jako element docelowy wykonywania na pasku poleceń u góry aplikacji po lewej stronie przycisku uruchamiania i `iris_sklearn.py` jako skrypt do uruchomienia.  Istnieje wiele innych plików zawartych w przykładzie, które możesz sprawdzić później. 

   ![img](media/quickstart-installation/run_control.png)

2. W polu tekstowym **Argumenty** wprowadź wartość `0.01`. Ta liczba jest używana w kodzie do ustawienia współczynnika uregulowania, wartości służącej do konfigurowania sposobu uczenia modelu regresji liniowej. 

3. Kliknij przycisk **Uruchom**, aby rozpocząć wykonywanie skryptu `iris_sklearn.py` na Twoim komputerze. 

   Ten kod używa do tworzenia modelu algorytmu [regresji logistycznej](https://en.wikipedia.org/wiki/logistic_regression) z popularnej biblioteki [scikit-learn](http://scikit-learn.org/stable/index.html) języka Python.

4. Panel **Zadania** wysuwa się z prawej strony, jeśli nie jest jeszcze widoczny, a zadanie `iris_sklearn` jest dodawane w panelu. Jego stan zmienia się z **Przesyłanie** na **Uruchamianie** po rozpoczęciu zadania, a następnie na **Ukończono** w ciągu kilku sekund. 

5. Gratulacje. Skrypt w języku Python został pomyślnie wykonany w aplikacji Azure ML Workbench.

6. Powtórz kroki 2–4 kilka razy. Za każdym razem użyj innych wartości argumentu z zakresu od `10` do `0.001`.

## <a name="view-run-history"></a>Widok historii uruchamiania
1. Przejdź do widoku **Przebiegi**, a następnie kliknij plik **iris_sklearn.py** na liście przebiegów. Zostanie otwarty pulpit nawigacyjny historii uruchamiania dla skryptu `iris_sklearn.py`. Zawiera on każdy przebieg skryptu `iris_sklearn.py`, który został wykonany. 

   ![img](media/quickstart-installation/run_view.png)

2. Pulpit nawigacyjny historii uruchamiania wyświetla również najważniejsze metryki, zestaw domyślnych grafów i listę metryk dla każdego przebiegu. Ten widok możesz dostosować, sortując, filtrując i dostosowując konfiguracje po kliknięciu ikony konfiguracji lub ikony filtru.

   ![img](media/quickstart-installation/run_dashboard.png)

3. Kliknij zakończony przebieg, a będzie można wyświetlić widok szczegółowy tego konkretnego wykonania, w tym dodatkowe metryki, pliki, które zostały wygenerowane, i inne potencjalnie przydatne dzienniki.

## <a name="next-steps"></a>Następne kroki
Pomyślnie utworzono konto Eksperymentowanie w usłudze Machine Learning i konto Zarządzanie modelami w usłudze Machine Learning. Zainstalowano aplikację klasyczną Azure Machine Learning Workbench i interfejs wiersza polecenia. Utworzono nowy projekt, utworzono model, wykonując skrypt, i zbadano historię uruchamiania skryptu.

Aby uzyskać więcej informacji na temat tego przepływu pracy oraz o sposobie wdrażania swojego modelu irysów w postaci usługi internetowej, wykonaj cały samouczek klasyfikowania irysów zawierający szczegółowe kroki [przygotowywania danych](tutorial-classifying-iris-part-1.md), [eksperymentów](tutorial-classifying-iris-part-2.md) i [zarządzania modelami](tutorial-classifying-iris-part-3.md). 

> [!div class="nextstepaction"]
> [Samouczek Klasyfikowanie irysów](tutorial-classifying-iris-part-1.md)

