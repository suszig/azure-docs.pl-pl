---
title: "Wprowadzenie do wstępnie skonfigurowanych rozwiązań | Microsoft Docs"
description: "Wykonaj czynności opisane w tym samouczku, aby dowiedzieć się, jak wdrożyć wstępnie skonfigurowane rozwiązanie Pakiet IoT Azure."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 9e1bcba086a9f70c689a5d7d7713a8ecdc764492
ms.openlocfilehash: 8248e0a02cb0775a87f0c8130e53b98f8bcfe581
ms.lasthandoff: 02/27/2017


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Samouczek: wprowadzenie do wstępnie skonfigurowanych rozwiązań
## <a name="introduction"></a>Wprowadzenie
[Wstępnie skonfigurowane rozwiązania][lnk-preconfigured-solutions] Pakietu IoT Azure obejmują wiele usług Azure IoT, co pozwala dostarczać kompleksowe rozwiązania, które umożliwiają implementowanie typowych scenariuszy biznesowych IoT. Wstępnie skonfigurowane rozwiązanie do *monitorowania zdalnego* łączy i monitoruje urządzenia. Rozwiązanie to umożliwia analizowanie strumienia danych z urządzeń oraz poprawianie wyników biznesowych dzięki zautomatyzowaniu odpowiedzi procesów na ten strumień danych.

W tym samouczku przedstawiono sposób aprowizowania wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego. Dostępny jest również opis podstawowych funkcji wstępnie skonfigurowanego rozwiązania. Dostęp do wielu z tych funkcji można uzyskać z pulpitu nawigacyjnego rozwiązania, który jest wdrażany jako część wstępnie skonfigurowanego rozwiązania:

![Pulpit nawigacyjny wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego][img-dashboard]

Do wykonania kroków tego samouczka jest potrzebna aktywna subskrypcja platformy Azure.

> [!NOTE]
> Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].
> 
> 

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Wyświetlanie pulpitu nawigacyjnego
Pulpit nawigacyjny pozwala zarządzać wdrożonym rozwiązaniem. Można na przykład wyświetlać dane telemetryczne, dodawać urządzenia i konfigurować reguły.

1. Jeśli aprowizacja została ukończona, a na kafelku wstępnie skonfigurowanego rozwiązania jest wyświetlany stan **Gotowe**, kliknij pozycję **Uruchom**, aby otworzyć portal rozwiązania do monitorowania zdalnego na nowej karcie.
   
   ![Uruchamianie wstępnie skonfigurowanego rozwiązania][img-launch-solution]
2. Domyślnie w portalu rozwiązania jest wyświetlany jego *pulpit nawigacyjny*. W menu po lewej stronie można wybierać inne widoki.
   
   ![Pulpit nawigacyjny wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego][img-dashboard]

Pulpit nawigacyjny udostępnia następujące informacje:

* Mapa zawiera lokalizację każdego urządzenia połączonego z rozwiązaniem. Przy pierwszym uruchomieniu rozwiązania dostępne są cztery symulowane urządzenia. Są one implementowane jako zadania WebJob Azure, a wykreślanie informacji na mapie odbywa się za pomocą interfejsu API Map Bing.
* W panelu **Historia telemetrii** są wyświetlane niemal w czasie rzeczywistym dane telemetryczne dotyczące wilgotności i temperatury pochodzące z wybranego urządzenia oraz dane zagregowane, takie jak wilgotność maksymalna, minimalna i średnia.
* W panelu **Historia alarmów** są wyświetlane ostatnie zdarzenia alarmów, generowane w przypadku przekroczenia progu przez wartość telemetryczną. Wstępnie skonfigurowane rozwiązanie zawiera przykładowe alarmy, ale można też definiować własne.
* Na panelu **Zadania** są wyświetlane informacje o zaplanowanych zadaniach. Własne zadania można zaplanować na stronie **Zadania zarządzania**.

## <a name="view-the-device-list"></a>Wyświetlanie listy urządzeń
*Lista urządzeń* zawiera wszystkie zarejestrowane urządzenia należące do rozwiązania. Korzystając z listy urządzeń, można wyświetlać i edytować metadane urządzeń, dodawać lub usuwać urządzenia i wywoływać metody na urządzeniach.

1. Kliknij pozycję **Urządzenia** w menu po lewej stronie, aby wyświetlić listę urządzeń dla tego rozwiązania.
   
   ![Lista urządzeń na pulpicie nawigacyjnym][img-devicelist]
2. Na liście początkowo są widoczne cztery symulowane urządzenia utworzone w procesie aprowizacji. Do rozwiązania można dodać dodatkowe symulowane i fizyczne urządzenia.
3. Informacje wyświetlane na liście urządzeń można dostosowywać. W tym celu należy kliknąć pozycję **Edytor kolumn**. Istnieje możliwość dodawania i usuwania kolumn, w których są wyświetlane zgłaszane wartości właściwości i tagów. Można również zmieniać kolejność kolumn i ich nazwy:
   
   ![Edytor kolumn na pulpicie nawigacyjnym][img-columneditor]
4. Aby wyświetlić szczegóły dotyczące danego urządzenia, kliknij je na liście urządzeń.
   
   ![Szczegóły urządzenia na pulpicie nawigacyjnym][img-devicedetails]

Panel **Szczegóły urządzenia** zawiera sześć sekcji:

* Kolekcja linków umożliwiająca dostosowanie ikony urządzenia, wyłączenie urządzenia, dodanie reguły, wywołanie metody lub wysłanie polecenia. Porównanie poleceń (komunikatów wysyłanych z urządzenia do chmury) i metod (metod bezpośrednich) znajduje się w temacie [Wskazówki dotyczące komunikacji z chmury do urządzenia][lnk-c2d-guidance].
* Sekcja **Bliźniacza reprezentacja urządzenia — Tagi** umożliwia edytowanie wartości tagów dla urządzenia. Wartości tagów można wyświetlać na liście urządzeń i używać ich do filtrowania tej listy.
* Sekcja **Bliźniacza reprezentacja urządzenia —Żądane właściwości** umożliwia ustawianie wartości właściwości wysyłanych do urządzenia.
* Sekcja **Bliźniacza reprezentacja urządzenia — Zgłaszane właściwości** umożliwia wyświetlanie wartości właściwości wysyłanych z urządzenia.
* Sekcja **Właściwości urządzenia** zawiera informacje z rejestru tożsamości, takie jak identyfikator urządzenia i klucze uwierzytelniania.
* Sekcja **Ostatnie zadania** zawiera informacje o wszystkich zadaniach, które były ukierunkowane na to urządzenie.

## <a name="customize-the-device-icon"></a>Dostosowywanie ikony urządzenia

Ikonę urządzenia wyświetlaną na liście urządzeń można dostosować na panelu **Szczegóły urządzenia** w następujący sposób:

1. Kliknij ikonę ołówka, aby otworzyć panel **Edytowanie obrazu** dla urządzenia:
   
   ![Otwieranie edytora obrazu dla urządzenia][img-startimageedit]
2. Przekaż nowy obraz lub użyj jednego z istniejących obrazów, a następnie kliknij przycisk **Zapisz**:
   
   ![Edytowanie edytora obrazu dla urządzenia][img-imageedit]
3. Wybrany obraz jest teraz wyświetlany w kolumnie **Ikona** dla urządzenia.

> [!NOTE]
> Obraz jest przechowywany w usłudze Blob Storage. Tag w bliźniaczej reprezentacji urządzenia zawiera link do obrazu w usłudze Blob Storage.
> 
> 

## <a name="invoke-a-method-on-a-device"></a>Wywoływanie metody na urządzeniu
Korzystając z panelu **Szczegóły urządzenia**, można wywołać metody na urządzeniu. Gdy urządzenie jest uruchamiane po raz pierwszy, wysyła do rozwiązania informacje o obsługiwanych metodach.

1. Kliknij pozycję **Metody** na panelu **Szczegóły urządzenia** dla wybranego urządzenia:
   
   ![Metody urządzenia na pulpicie nawigacyjnym][img-devicemethods]
2. Z listy metod wybierz pozycję **Ponowne uruchamianie**.
3. Kliknij pozycję **Wywołaj metodę**.
4. Stan wywołania metody można obserwować w historii metody.
   
   ![Stan metody na pulpicie nawigacyjnym][img-pingmethod]

Rozwiązanie umożliwia śledzenie stanu wszystkich wywoływanych metod. Po zakończeniu metody na urządzeniu w tabeli historii metody zostanie umieszczony nowy wpis.

Niektóre metody uruchamiają zadania asynchroniczne na urządzeniu. Na przykład metoda **InitiateFirmwareUpdate** uruchamia zadanie asynchroniczne przeprowadzające aktualizację. Urządzenie używa zgłaszanych właściwości do zgłaszania stanu aktualizacji oprogramowania układowego w miarę jej postępu.

## <a name="send-a-command-to-a-device"></a>Wysyłanie polecenia do urządzenia
Korzystając z panelu **Szczegóły urządzenia**, można wysłać polecenia do urządzenia. Gdy urządzenie jest uruchamiane po raz pierwszy, wysyła do rozwiązania informacje o obsługiwanych poleceniach.

1. Kliknij pozycję **Polecenia** na panelu **Szczegóły urządzenia** dla wybranego urządzenia:
   
   ![Polecenia urządzenia na pulpicie nawigacyjnym][img-devicecommands]
2. Wybierz pozycję **PingDevice** na liście poleceń.
3. Kliknij pozycję **Wyślij polecenie**.
4. Stan polecenia można zobaczyć w historii poleceń.
   
   ![Stan polecenia na pulpicie nawigacyjnym][img-pingcommand]

Rozwiązanie umożliwia śledzenie stanu wszystkich wysyłanych poleceń. Na początku wynik ma wartość **Oczekiwanie**. Gdy urządzenie zgłosi wykonanie polecenia, wynik jest ustawiany na **Powodzenie**.

## <a name="add-a-new-simulated-device"></a>Dodawanie nowego symulowanego urządzenia
Podczas wdrażania wstępnie skonfigurowanego rozwiązania automatycznie aprowizujesz cztery przykładowe urządzenia widoczne na liście urządzeń. Są to *symulowane urządzenia* uruchamiane w zadaniu WebJob Azure. Symulowane urządzenia ułatwiają eksperymentowanie ze wstępnie skonfigurowanym rozwiązaniem bez konieczności wdrażania prawdziwych urządzeń fizycznych. Jeśli chcesz połączyć prawdziwe urządzenie z rozwiązaniem, zobacz samouczek [Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem do monitorowania zdalnego][lnk-connect-rm].

Poniższe kroki pokazują, jak dodać symulowane urządzenie do rozwiązania:

1. Wróć do listy urządzeń.
2. Aby dodać urządzenie, kliknij pozycję **+ Dodaj urządzenie** w lewym dolnym rogu.
   
   ![Dodawanie urządzenia do wstępnie skonfigurowanego rozwiązania][img-adddevice]
3. Kliknij pozycję **Dodaj nowe** na kafelku **Symulowane urządzenie**.
   
   ![Określanie szczegółów nowego urządzenia na pulpicie nawigacyjnym][img-addnew]
   
   Oprócz tworzenia nowych symulowanych urządzeń można również dodawać urządzenia fizyczne — wystarczy wybrać opcję utworzenia **niestandardowego urządzenia**. Aby dowiedzieć się więcej na temat łączenia fizycznych urządzeń z rozwiązaniem, zobacz artykuł [Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem Pakietu IoT służącym do monitorowania zdalnego][lnk-connect-rm].
4. Wybierz pozycję **Pozwól mi zdefiniować własny identyfikator urządzenia** i wprowadź unikatowy identyfikator urządzenia, np. **moje_urzadzenie_01**.
5. Kliknij przycisk **Utwórz**.
   
   ![Zapisywanie nowego urządzenia][img-definedevice]
6. W kroku 3 procedury **Dodawanie symulowanego urządzenia** kliknij pozycję **Gotowe**, aby powrócić do listy urządzeń.
7. Na liście urządzeń widać, że stan urządzenia ma wartość **Uruchomione**.
   
    ![Wyświetlanie nowego urządzenia na liście urządzeń][img-runningnew]
8. Na pulpicie nawigacyjnym można również wyświetlić symulowane dane telemetryczne pochodzące z nowego urządzenia:
   
    ![Wyświetlanie telemetrii z nowego urządzenia][img-runningnew-2]

## <a name="device-properties"></a>Właściwości urządzenia
Wstępnie skonfigurowane rozwiązanie do zdalnego monitorowania korzysta z [bliźniaczych reprezentacji urządzeń][lnk-device-twin], aby zsynchronizować metadane urządzenia przesyłane między urządzeniami i zapleczem rozwiązania. Bliźniacza reprezentacja urządzenia to dokument JSON przechowywany w usłudze IoT Hub magazynującej wartości właściwości dla poszczególnych urządzeń. Urządzenia regularnie wysyłają metadane do zaplecza rozwiązania w postaci *zgłaszanych właściwości* w celu ich zapisania w bliźniaczej reprezentacji urządzenia. Zaplecze rozwiązania może ustawić *żądane właściwości* w bliźniaczej reprezentacji urządzenia w celu wysyłania aktualizacji metadanych do urządzeń. Zgłaszane właściwości odzwierciedlają najbardziej aktualne wartości metadanych wysyłane przez urządzenie. Aby uzyskać więcej informacji, zobacz sekcję [Rejestr tożsamości urządzeń, bliźniacza reprezentacja urządzenia i baza danych DocumentDB][lnk-devicemetadata].

> [!NOTE]
> Rozwiązanie również korzysta z bazy danych DocumentDB do przechowywania danych specyficznych dla urządzenia powiązanych z poleceniami i metodami.
> 
> 

1. Wróć do listy urządzeń.
2. Wybierz nowe urządzenie na **liście urządzeń**, a następnie kliknij pozycję **Edytuj**, aby zmodyfikować sekcję **Bliźniacza reprezentacja urządzenia —Żądane właściwości**:
   
   ![Edytowanie żądanych właściwości urządzenia][img-editdevice]
3. Ustaw **nazwę żądanej właściwości** na **szerokość geograficzną** i ustaw jej wartość na **47.639521**. Następnie kliknij pozycję **Zapisz zmiany w rejestrze urządzenia**:
   
    ![Edytowanie żądanej właściwości urządzenia][img-editdevice2]
4. Na panelu **Szczegóły urządzenia** nowa wartość szerokości geograficznej początkowo jest wyświetlana jako żądana właściwość, a stara wartość szerokości geograficznej jest wyświetlana jako zgłaszana właściwość:
   
    ![Wyświetlanie zgłaszanej właściwości][img-editdevice3]
5. Aktualnie symulowane urządzenia we wstępnie skonfigurowanym rozwiązaniu przetwarzają tylko żądane właściwości **Desired.Config.TemperatureMeanValue** i **Desired.Config.TelemetryInterval**. Rzeczywiste urządzenie powinno odczytywać wszystkie żądane właściwości z usługi IoT Hub, dokonywać zmian w jej konfiguracji i zgłaszać nowe wartości do centrum jako właściwości zgłaszane.

Na panelu **Szczegóły urządzenia** można również edytować sekcję **Bliźniacza reprezentacja urządzenia — Tagi** w taki sam sposób jak sekcję **Bliźniacza reprezentacja urządzenia —Żądane właściwości**. Jednak w przeciwieństwie do żądanych właściwości tagi nie są synchronizowane z urządzeniem. Tagi istnieją tylko w bliźniaczej reprezentacji urządzenia w usłudze IoT Hub. Tagi są przydatne podczas tworzenia niestandardowych filtrów na liście urządzeń.

## <a name="sort-the-device-list"></a>Sortowanie listy urządzeń

Listę urządzeń można sortować, klikając obok nagłówka kolumny. Pierwsze kliknięcie powoduje posortowanie w kolejności rosnącej, a drugie kliknięcie sortuje w kolejności malejącej:

![Sortowanie listy urządzeń][img-sortdevices]

## <a name="filter-the-device-list"></a>Filtrowanie listy urządzeń

Na liście urządzeń można tworzyć, zapisywać i ponownie ładować filtry, co umożliwia wyświetlenie niestandardowej listy urządzeń podłączonych do centrum. Aby utworzyć filtr:

1. Kliknij ikonę edycji filtru nad listą urządzeń:
   
   ![Otwieranie edytora filtru][img-editfiltericon]
2. W **Edytorze filtrów** można dodawać pola, operatory i wartości, aby filtrować listę urządzeń. W celu doprecyzowania filtru można dodać wiele klauzul. Kliknij pozycję **Filtruj**, aby zastosować filtr:
   
   ![Tworzenie filtru][img-filtereditor]
3. W tym przykładzie lista jest filtrowana według producenta i numeru modelu:
   
   ![Filtrowana lista][img-filterelist]
4. Aby zapisać filtr pod niestandardową nazwą, kliknij ikonę **Zapisz jako**:
   
   ![Zapisywanie filtru][img-savefilter]
5. Aby ponownie zastosować uprzednio zapisany filtr, kliknij ikonę **Otwórz zapisany filtr**:
   
   ![Otwieranie filtru][img-openfilter]

Filtry można tworzyć na postawie identyfikatora urządzenia, stanu urządzenia, żądanych właściwości, zgłaszanych właściwości i tagów.

> [!NOTE]
> W **Edytorze filtrów** można użyć **Widoku zaawansowanego**, aby móc bezpośrednio edytować tekst zapytania.
> 
> 

## <a name="add-a-rule-for-the-new-device"></a>Dodawanie reguły dla nowego urządzenia
Dla nowo dodanego urządzenia nie zostały określone żadne reguły. W tej sekcji dodasz regułę, która powoduje uruchomienie alarmu, gdy temperatura zgłoszona przez nowe urządzenie przekroczy 47 stopni. Przed wykonaniem poniższych instrukcji zwróć uwagę na to, że zgodnie z historią telemetrii dla nowego urządzenia dostępną na pulpicie nawigacyjnym temperatura urządzenie nigdy nie przekracza 45 stopni.

1. Wróć do listy urządzeń.
2. Aby dodać regułę dla urządzenia, wybierz nowe urządzenie na **liście urządzeń**, a następnie kliknij pozycję **Dodaj regułę**.
3. Utwórz regułę, w której jest używane pole danych **Temperatura** i która generuje dane wyjściowe **AlarmTemp**, gdy temperatura przekroczy 47 stopni:
   
    ![Dodawanie reguły urządzenia][img-adddevicerule]
4. Aby zapisać zmiany, kliknij pozycję **Zapisz i wyświetl reguły**.
5. Kliknij pozycję **Polecenia** w okienku szczegółów nowego urządzenia.
   
   ![Dodawanie reguły urządzenia][img-adddevicerule2]
6. Wybierz pozycję **ChangeSetPointTemp** z listy poleceń i ustaw parametr **SetPointTemp** na wartość 45. Następnie kliknij pozycję **Wyślij polecenie**.
   
   ![Dodawanie reguły urządzenia][img-adddevicerule3]
7. Wróć do pulpitu nawigacyjnego rozwiązania. Po pewnym czasie w okienku **Historia alarmów** pojawi się nowy wpis, gdy temperatura urządzenia przekroczy wartość progową równą 47 stopni:
   
   ![Dodawanie reguły urządzenia][img-adddevicerule4]
8. Na stronie **Reguły** pulpitu nawigacyjnego można przeglądać i edytować wszystkie reguły.
   
    ![Wyświetlanie listy reguł urządzenia][img-rules]
9. Na stronie **Akcje** pulpitu nawigacyjnego można przeglądać i edytować wszystkie akcje, które mogą być wykonywane w odpowiedzi na działanie reguły:
   
    ![Wyświetlanie listy akcji urządzenia][img-actions]

> [!NOTE]
> Istnieje możliwość zdefiniowania akcji, które umożliwiają wysyłanie wiadomości e-mail lub SMS w odpowiedzi na działanie reguły albo pozwalają na integrację z systemem biznesowym za pośrednictwem [aplikacji logiki][lnk-logic-apps]. Aby uzyskać więcej informacji, zobacz [Łączenie aplikacji logiki ze wstępnie skonfigurowanym rozwiązaniem do zarządzania zdalnego Pakietu IoT Azure][lnk-logicapptutorial].
> 
> 

## <a name="disable-and-remove-devices"></a>Wyłączanie i usuwanie urządzeń
Można wyłączyć urządzenie, a następnie je usunąć:

![Wyłączanie i usuwanie urządzenia][img-disable]

## <a name="run-jobs"></a>Uruchamianie zadań
Zadania można tak zaplanować, aby wykonywały operacje zbiorcze na urządzeniach. W tym celu należy utworzyć zadanie dla listy urządzeń. Taka lista może zawierać wszystkie urządzenia lub może być listą filtrowaną utworzoną za pomocą [narzędzi filtrowania](#filter-the-device-list) na liście urządzeń. Zadanie może wywołać metodę na każdym urządzeniu znajdującym się na liście lub może zaktualizować bliźniaczą reprezentację urządzenia dla poszczególnych urządzeń na liście.

### <a name="create-a-job-to-invoke-a-method"></a>Tworzenie zadania na potrzeby wywołania metody

Poniżej przedstawiono sposób tworzenia zadania wywołującego metodę aktualizacji oprogramowania układowego na każdym urządzeniu znajdującym się na liście. Ta metoda jest wywoływana tylko na urządzeniach, które obsługują tę metodę:

1. Użyj narzędzi filtrowania na liście urządzeń, aby utworzyć listę urządzeń, które powinny otrzymać aktualizację oprogramowania układowego:
   
   ![Otwieranie edytora filtru][img-editfiltericon]
2. Na liście filtrowanej kliknij pozycję **Harmonogram zadań**:
   
   ![Otwieranie harmonogramu zadań][img-clickjobscheduler]
3. Na panelu **Planowanie zadania** kliknij pozycję **Wywołaj metodę**.
4. Na stronie **Wywołanie metody** wprowadź szczegóły wywoływanej metody, a następnie kliknij pozycję **Harmonogram**:
   
   ![Konfigurowanie zadania metody][img-invokemethodjob]

Metoda **InitiateFirmwareUpdate** uruchamia zadanie asynchronicznie na urządzeniu i natychmiast powraca. Następnie proces aktualizacji oprogramowania układowego używa zgłaszanych właściwości do raportowania procesu aktualizacji podczas jego trwania.

### <a name="create-a-job-to-edit-the-device-twin"></a>Tworzenie zadania do edytowania bliźniaczej reprezentacji urządzenia

Poniżej przedstawiono sposób tworzenia zadania modyfikującego bliźniaczą reprezentację urządzenia dla każdego urządzenia znajdującego się na liście:

1. Użyj narzędzi filtrowania na liście urządzeń, aby utworzyć listę urządzeń, które powinny otrzymać modyfikacje bliźniaczej reprezentacji urządzenia:
   
   ![Otwieranie edytora filtru][img-editfiltericon]
2. Na liście filtrowanej kliknij pozycję **Harmonogram zadań**:
   
   ![Otwieranie harmonogramu zadań][img-clickjobscheduler]
3. Na panelu **Planowanie zadania** kliknij pozycję **Edytuj bliźniaczą reprezentację urządzenia**.
4. Następnie na stronie **Edytowanie bliźniaczej reprezentacji urządzenia** wprowadź szczegóły **żądanych właściwości** i **tagów**, które mają zostać zmodyfikowane, a następnie kliknij przycisk **Harmonogram**:
   
   ![Konfigurowanie zadania metody][img-edittwinjob]

### <a name="monitor-the-job"></a>Monitorowanie zadania
Stan zaplanowanych zadań można monitorować na stronie **Zadania zarządzania**. Na panelu **Szczegóły zadania** są wyświetlane informacje dotyczące wybranego zadania:
   
   ![Wyświetlanie stanu zadania][img-jobstatus]

Informacje dotyczące zadań można również wyświetlać na **pulpicie nawigacyjnym**:
   
   ![Wyświetlanie zadań na pulpicie nawigacyjnym][img-jobdashboard]


## <a name="behind-the-scenes"></a>Za kulisami
Podczas wdrażania wstępnie skonfigurowanego rozwiązania jest tworzonych wiele zasobów w ramach wybranej subskrypcji platformy Azure. Można je wyświetlić w witrynie Azure [Portal][lnk-portal]. W procesie wdrażania jest tworzona **grupa zasobów** o nazwie odpowiadającej nazwie wstępnie skonfigurowanego rozwiązania:

![Wstępnie skonfigurowane rozwiązanie w portalu Azure][img-portal]

Aby wyświetlić ustawienia danego zasobu, wybierz go z listy w grupie zasobów.

Można również wyświetlić kod źródłowy wstępnie skonfigurowanego rozwiązania. Kod źródłowy wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego znajduje się w repozytorium GitHub [azure-iot-remote-monitoring][lnk-rmgithub]:

* Folder **DeviceAdministration** zawiera kod źródłowy pulpitu nawigacyjnego.
* Folder **Simulator** zawiera kod źródłowy symulowanego urządzenia.
* Folder **EventProcessor** zawiera kod źródłowy procesu zaplecza, który obsługuje przychodzące dane telemetryczne.

Gdy wszystko będzie gotowe, możesz usunąć wstępnie skonfigurowane rozwiązanie z subskrypcji platformy Azure w witrynie [azureiotsuite.com][lnk-azureiotsuite]. Ta witryna umożliwia łatwe usunięcie wszystkich zasobów, które zostały aprowizowane po utworzeniu wstępnie skonfigurowanego rozwiązania.

> [!NOTE]
> Aby mieć pewność, że zostaną usunięte wszystkie elementy powiązane ze wstępnie skonfigurowanym rozwiązaniem, nie usuwaj grupy zasobów w portalu, ale usuń rozwiązanie w witrynie [azureiotsuite.com][lnk-azureiotsuite].
> 
> 

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy zostało wdrożone wstępnie skonfigurowane rozwiązanie, które działa, możesz kontynuować poznawanie Pakietu IoT, czytając następujące artykuły:

* [Przewodnik po wstępnie skonfigurowanym rozwiązaniu monitorowania zdalnego][lnk-rm-walkthrough]
* [Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem do monitorowania zdalnego][lnk-connect-rm]
* [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-devicemethods]: media/iot-suite-getstarted-preconfigured-solutions/devicemethods.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-pingmethod]: media/iot-suite-getstarted-preconfigured-solutions/pingmethod.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-sortdevices]: media/iot-suite-getstarted-preconfigured-solutions/sortdevices.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-clickjobscheduler]: media/iot-suite-getstarted-preconfigured-solutions/clickscheduler.png
[img-invokemethodjob]: media/iot-suite-getstarted-preconfigured-solutions/invokemethodjob.png
[img-edittwinjob]: media/iot-suite-getstarted-preconfigured-solutions/edittwinjob.png
[img-jobstatus]: media/iot-suite-getstarted-preconfigured-solutions/jobstatus.png
[img-jobdashboard]: media/iot-suite-getstarted-preconfigured-solutions/jobdashboard.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-device-twin-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
