---
title: "Jak korzystać z notesów Jupyter w Workbench Azure Machine Learning | Dokumentacja firmy Microsoft"
description: "Przewodnik dla funkcji notesów Jupyter Azure Machine Learning Workbench za pomocą"
services: machine-learning
author: jopela
ms.author: jopela
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 93850a7c9e3d9d69b0da22ebd0656ae40cee2e63
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="how-to-use-jupyter-notebook-in-azure-machine-learning-workbench"></a>Jak używać notesu Jupyter w konsoli usługi Azure Machine Learning Workbench

Azure Machine Learning Workbench obsługuje eksperymenty nauki interaktywnych danych za pośrednictwem jego integracji notesu Jupyter. W tym artykule opisano sposób efektywnie wykorzystać tę funkcję, aby zwiększyć szybkość i jakość eksperymenty nauki Twoje dane interakcyjne.

## <a name="prerequisites"></a>Wymagania wstępne
- [Instalowanie i tworzenie usługi Azure Machine Learning](/machine-learning/preview/quickstart-installation.md).
- Należy zapoznać się z [notesu Jupyter](http://jupyter.org/), ponieważ w tym artykule nie jest o nauczania sposób użycia Jupyter.

## <a name="jupyter-notebook-architecture"></a>Architektura notesu Jupyter
Na wysokim poziomie architektura notesu Jupyter zawiera trzy składniki, każdy można uruchomić w środowiskach różnych obliczeniowych:

- **Klient**: otrzymuje dane wejściowe użytkownika i wyświetla renderowania danych wyjściowych
- **Serwer**: serwer sieci web obsługujący notesu (pliki .ipynb)
- **Jądra**: środowisko uruchomieniowe, gdzie się stanie rzeczywistego wykonania komórek notesu

Więcej szczegółów, odwołaj się do urzędnika [dokumentacji Jupyter](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Fowllowing jest diagram przedstawiający sposób mapowania tego klienta, serwera i Architektura jądra ze składnikami w uczenie Maszynowe Azure.

![Architektura notesu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-ml-workbench-notebook"></a>Jądra w notesie Workbench uczenia Maszynowego Azure
Dostęp do wielu różnych jądra w Azure ML Workbench przez po prostu ustawienia konfiguracji uruchamiania i obliczeniowe elementy docelowe w `aml_config` folder w projekcie. Dodanie nowego miejsca docelowego obliczeń wysyłając `az ml computetarget attach` polecenie jest odpowiednikiem Dodawanie nowych jądra.

>[!NOTE]
>Przegląd [skonfigurować wykonywania](experimentation-service-configuration.md) więcej informacji na temat uruchamiania konfiguracji i obliczeniowe elementy docelowe.

### <a name="kernel-naming-convention"></a>Konwencja nazewnictwa jądra
Jądra nazwy w formacie "\<Nazwa projektu > \<Uruchom Nazwa konfiguracji >". Na przykład, jeśli masz konfigurację uruchomieniową o nazwie _docker python_ w projekcie o nazwie _myIris_, można znaleźć jądra o nazwie "myIris docker-python" na liście jądra po otwarciu notesu Jupyter.

Obecnie Workbench obsługuje następujące typy jądra.

### <a name="local-python-kernel"></a>Lokalne jądra Python
Ta jądra Python obsługuje wykonywanie na komputerze lokalnym. Jest zintegrowany z obsługą historii uruchamiania usługi Azure Machine Learning. Nazwa jądra jest zwykle "my_project_name lokalny".

### <a name="python-kernel-in-docker-local-or-remote"></a>Python jądra w Docker (lokalne i zdalne)
Ta jądra Python działa w kontenerze Docker na komputerze lokalnym lub zdalnym maszyny Wirtualnej systemu Linux. Nazwa jądra jest zwykle "my_project docker". Skojarzony `docker.runconfig` plik ma `Framework` pole `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>Jądro PySpark w Docker (lokalne i zdalne)
Ta jądra PySpark wykonuje skrypty w kontekście Spark działają w ramach kontenera Docker na komputerze lokalnym lub zdalnym maszyny wirtualnej systemu Linux. Nazwa jądra jest zwykle "my_project docker". Skojarzony `docker.runconfig` plik ma `Framework` pole `PySpark`.

### <a name="pyspark-kernel-on-hdinsight-cluster"></a>Jądro PySpark w klastrze usługi HDInsight
Działa to jądra w zdalnego klastra usługi HDInsight dołączony jako cel obliczeniowe dla projektu. Nazwa jądra jest zwykle "my_project my_hdi". 

>[!IMPORTANT]
>W `.compute` pliku HDI obliczeniowe docelowej, musisz zmienić `yarnDeployMode` do `client` (wartość domyślna to `cluster`) Aby użyć tego jądra. 

## <a name="start-jupyter-server-from-the-workbench"></a>Uruchom serwer Jupyter z Workbench
Z Azure Machine Learning Workbench notesów jest możliwy za pośrednictwem Workbench **notesów** kartę. _Klasyfikowania Iris_ zawiera przykładowy projekt `iris.ipynb` przykładowy notes.

![Karta notesów](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Po otwarciu Notes w konsoli usługi Azure Machine Learning Workbench, pojawi się w osobnej karcie dokumentu w **trybu podglądu**. To jest widoku tylko do odczytu, która nie wymaga uruchomiony serwer Jupyter i jądra.

![Podgląd notesu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Kliknięcie przycisku **uruchamianie serwera notesu** przycisk uruchamia serwer Jupyter i zmienia notesu do **trybu edycji**. Znanego interfejsu użytkownika notesu Jupyter wydaje się być osadzony w Workbench. Teraz możesz ustawić jądra z **jądra** menu i uruchomić sesję interakcyjne notesu. 

>[!NOTE]
>Uwaga jądra nie lokalnego może potrwać minutę lub dwie można uruchomić, jeśli używasz jej po raz pierwszy. Możesz wykonać `az ml experiment prepare` polecenia z okna interfejsu wiersza polecenia, aby przygotować docelowy obliczeń, po przygotowaniu jest element docelowy obliczeń jądra można uruchomić znacznie szybciej.

![Tryb edycji](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Jest to całkowicie interakcyjny środowisko notesu Jupyter. Wszystkie operacje regularne notesu i skróty klawiaturowe są obsługiwane z tego okna, z wyjątkiem niektórych operacji na plikach, ponieważ może odbywać za pośrednictwem Workbench **notesów** kartę i **pliku** kartę.

## <a name="start-jupyter-server-from-command-line"></a>Uruchom serwer Jupyter z wiersza polecenia
Można również uruchomić sesję notesu wysyłając `az ml notebook start` w oknie wiersza polecenia:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Domyślnej przeglądarki zostanie automatycznie uruchomiony z serwerem Jupyter wskazaniu katalogu macierzystego projektu. Aby uruchomić lokalnie inne okna przeglądarki, można użyć adresu URL i token wyświetlane w oknie interfejsu wiersza polecenia. 

![pulpit nawigacyjny projektu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Możesz teraz kliknąć `.ipynb` pliku notesu otwórz go, ustaw jądra (Jeśli nie została ustawiona) i uruchomić sesję interaktywnego.

![pulpit nawigacyjny projektu](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="next-steps"></a>Następne kroki
- Aby dowiedzieć się, jak użyć notesu Jupyter, odwiedź stronę [Jupyter oficjalnej dokumentacji](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Aby lepiej zrozumieć środowiska wykonawczego eksperymenty uczenie Maszynowe Azure, przejrzyj [usługi eksperymenty Omówienie usługi Azure Machine Learning](experimentation-service-configuration.md)

