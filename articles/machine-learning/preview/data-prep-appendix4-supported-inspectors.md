---
title: "Obsługiwane inspektorzy dostępna w programie Azure Machine Learning danych przygotowanie | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera pełną listę inspektorzy dostępne w celu przygotowania danych usługi Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 35d7c04f245e93d8cc795dca7c01c2bab5a14eb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="supported-inspectors-for-the-azure-machine-learning-data-preparation-preview"></a>Obsługiwane inspektorzy dla usługi Azure Machine Learning przygotowywania danych z wersji zapoznawczej
W tym dokumencie przedstawiono zbiór inspektorzy, które są dostępne w tej wersji zapoznawczej.

## <a name="the-halo-effect"></a>Otoczek 
Niektóre inspektorzy obsługuje otoczek. W tym celu używa dwóch różnych kolorów, aby natychmiast wyświetlić zmianę wizualnie transformacji. Kolor szary reprezentuje wartość przed najnowsze transformacji i niebieski pokazuje bieżącą wartość. W tym celu może być włączony i wyłączone w opcji.

## <a name="graphical-filtering"></a>Filtrowanie graficznego 
Filtrowanie danych za pomocą Inspektora edytorem niektóre inspektorzy obsługiwane. Za pomocą Inspektora edytorem obejmuje zaznaczając elementy graficzne, a następnie za pomocą narzędzi w prawej górnej części okna inspektora do filtrowania przychodzący lub wychodzący wybranych wartości. 

## <a name="column-statistics"></a>Statystyk kolumny
Dla kolumny liczbowe Ten inspektor zawiera szereg różnych statystyki dotyczące kolumny. Statystyka zawiera następujące pomiary: 
- Minimalne
- Niższe kwartyl
- Mediana
- Górny kwartyl
- Maksymalna
- Średnia
- Odchylenie standardowe


### <a name="options"></a>Opcje 
- Brak

## <a name="histogram"></a>Histogram 
Oblicza i wyświetla histogram pojedynczej kolumny liczbowe. Domyślna liczba zasobników jest obliczana przy użyciu reguły Scotta. Jednak można zastąpić reguły za pomocą opcji.

Ten inspektor obsługuje otoczek.


### <a name="options"></a>Opcje
- Minimalna liczba zasobników (ma zastosowanie, nawet gdy bucketing domyślny jest zaznaczone)
- Domyślna liczba zasobników (Scotta reguła) 
- Pokaż halo
- Nakładka kreślenia gęstość jądra (Gaussa jądra) 


### <a name="actions"></a>Akcje
Ten inspektor obsługuje filtrowanie za pośrednictwem pakiety, które mogą obejmować zasobników pojedynczego lub wielokrotnego wyboru. Zastosuj filtry, jak opisano wcześniej.

## <a name="value-counts"></a>Wartość liczby
Ten inspektor przedstawia częstotliwość tabeli wartości dla aktualnie wybranej kolumny. Górny sześciu wartości są wyświetlane domyślne. Można jednak zmienić limit na dowolną liczbę. Można również ustawić widok, aby liczba od dołu zamiast podklauzuli top. Ten inspektor obsługuje otoczek.

### <a name="options"></a>Opcje 
- Liczba wartości górnej
- Malejąco
- Zawierają wartości null/błędów
- Pokaż halo


### <a name="actions"></a>Akcje 
Ten inspektor obsługuje filtrowanie za pośrednictwem paski, które mogą obejmować paski pojedynczego lub wielokrotnego wyboru. Zastosuj filtry, jak opisano wcześniej.

## <a name="box-plot"></a>Wykres skrzynkowy 
Pole kreślenia wąsami kolumny liczbowej.

### <a name="options"></a>Opcje 
- Grupuj według kolumny

## <a name="scatter-plot"></a>Wykres punktowy
Wykres punktowy dla dwóch kolumn liczbowych. Dane są próbkowane w dół ze względu na wydajność. Rozmiar próbki mogą zostać zastąpione w opcjach.

### <a name="options"></a>Opcje  
- Kolumna osi x
- Kolumna osi y
- Rozmiar próbki
- Grupuj według kolumny


## <a name="time-series"></a>Szeregów czasowych
Wykres liniowy z funkcją rozpoznawanie czasu na osi x.

### <a name="options"></a>Opcje
- Data kolumny
- Kolumny liczbowej
- Rozmiar próbki


### <a name="actions"></a>Akcje
Ten inspektor obsługuje filtrowanie przy użyciu metody select kliknij i przeciągnij, aby wybrać zakres na wykresie. Po zakończeniu wyboru Zastosuj filtry, jak opisano wcześniej.


## <a name="map"></a>Mapa 
Mapa punktów, które są kreślone, przy założeniu, że zostały określone współrzędne geograficzne. Najpierw należy wybrać szerokości geograficznej.

### <a name="options"></a>Opcje
- Szerokość kolumny
- Długość geograficzna kolumny
- Klastrowanie w
- Grupuj według kolumny


### <a name="actions"></a>Akcje
Ten inspektor obsługuje filtrowanie za pośrednictwem wyboru punktu na mapie. Naciśnij klawisz **Ctrl** klucza, a następnie kliknij i przeciągnij za pomocą myszy do utworzenia kwadrat wokół punktów. Następnie Zastosuj filtry, jak opisano wcześniej.

Można szybko rozmiar mapę, aby pokazać tylko te punkty możliwe przez naciśnięcie przycisku **E** po lewej stronie mapy.
