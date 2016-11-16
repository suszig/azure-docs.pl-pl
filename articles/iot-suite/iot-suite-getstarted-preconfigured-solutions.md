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
ms.date: 08/16/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8ec86ebefccaf74c67cb7917ccf7d538bc95ae07


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Samouczek: wprowadzenie do wstępnie skonfigurowanych rozwiązań
## <a name="introduction"></a>Wprowadzenie
[Wstępnie skonfigurowane rozwiązania][lnk-preconfigured-solutions] Pakietu IoT Azure obejmują wiele usług Azure IoT, co pozwala dostarczać kompleksowe rozwiązania, które umożliwiają implementowanie typowych scenariuszy biznesowych IoT. Wstępnie skonfigurowane rozwiązanie do *monitorowania zdalnego* łączy i monitoruje urządzenia. Rozwiązanie to umożliwia analizowanie strumienia danych z urządzeń oraz poprawianie wyników biznesowych dzięki zautomatyzowaniu odpowiedzi procesów na ten strumień danych.

W tym samouczku przedstawiono sposób aprowizowania wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego. Dostępny jest również opis podstawowych funkcji rozwiązania do monitorowania zdalnego. Dostęp do wielu z tych funkcji można uzyskać za pośrednictwem pulpitu nawigacyjnego rozwiązania, który jest wdrażany razem ze wstępnie skonfigurowanym rozwiązaniem:

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

## <a name="view-the-device-list"></a>Wyświetlanie listy urządzeń
Lista urządzeń zawiera wszystkie zarejestrowane urządzenia należące do rozwiązania. Urządzenia można dodawać i usuwać oraz wysyłać do nich polecenia, a także wyświetlać i edytować ich metadane.

1. Kliknij pozycję **Urządzenia** w menu po lewej stronie, aby wyświetlić *listę urządzeń* dla tego rozwiązania.
   
   ![Lista urządzeń na pulpicie nawigacyjnym][img-devicelist]
2. Na liście są widoczne cztery symulowane urządzenia utworzone w procesie aprowizacji.
3. Aby wyświetlić szczegóły dotyczące danego urządzenia, kliknij odpowiednią pozycję na liście.
   
   ![Szczegóły urządzenia na pulpicie nawigacyjnym][img-devicedetails]

W panelu **Szczegóły urządzenia** są widoczne trzy sekcje:

* Sekcja **Akcje** zawiera listę akcji, które można zastosować do urządzenia. Wyłączenie urządzenia powoduje, że nie może ono wysyłać danych telemetrycznych ani odbierać poleceń. Wyłączone urządzenie można ponownie włączyć. Z urządzeniem można skojarzyć regułę, która spowoduje wyzwolenie alarmu w przypadku przekroczenia progu przez wartość telemetryczną. Do urządzenia można także wysyłać polecenia. Gdy urządzenie po raz pierwszy łączy się z rozwiązaniem, wysyła listę obsługiwanych poleceń.
* W sekcji **Właściwości urządzenia** są wyświetlane metadane urządzenia. Niektóre metadane pochodzą z urządzenia (na przykład informacje o producencie), a inne są generowane przez rozwiązanie (na przykład godzina utworzenia). Metadane urządzeń można edytować.
* Sekcja **Klucze uwierzytelniania** zawiera listę kluczy używanych przez urządzenie do uwierzytelniania w rozwiązaniu.

## <a name="send-a-command-to-a-device"></a>Wysyłanie polecenia do urządzenia
Okienko szczegółów urządzenia zawiera wszystkie polecenia obsługiwane przez to urządzenie oraz umożliwia wysyłanie poleceń do urządzenia. Gdy urządzenie jest uruchamiane po raz pierwszy, wysyła do rozwiązania informacje o obsługiwanych poleceniach.

1. Kliknij pozycję **Polecenia** w okienku szczegółów wybranego urządzenia.
   
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
2. Kliknij pozycję **+ Dodaj urządzenie** w lewym dolnym rogu, aby dodać urządzenie.
   
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

## <a name="edit-the-device-metadata"></a>Edytowanie metadanych urządzenia
Kiedy urządzenie po raz pierwszy łączy się z rozwiązaniem, wysyła do rozwiązania swoje metadane. Gdy metadane urządzenia zostaną zmienione za pośrednictwem pulpitu nawigacyjnego rozwiązania, pulpit wysyła nowe wartości metadanych do urządzenia i zapisuje je w bazie danych DocumentDB rozwiązania. Aby uzyskać więcej informacji, zobacz [Rejestr tożsamości urządzeń i baza danych DocumentDB][lnk-devicemetadata].

1. Wróć do listy urządzeń.
2. Wybierz nowe urządzenie na **liście urządzeń**, a następnie kliknij pozycję **Edytuj**, aby zmodyfikować **właściwości urządzenia**:
   
   ![Edytowanie metadanych urządzenia][img-editdevice]
3. Przewiń w dół i wprowadź zmiany w wartościach szerokości i długości geograficznej. Następnie kliknij pozycję **Zapisz zmiany w rejestrze urządzenia**.
   
    ![Edytowanie metadanych urządzenia][img-editdevice2]
4. Wróć do pulpitu nawigacyjnego i zwróć uwagę, że lokalizacja urządzenia na mapie zmieniła się:
   
    ![Edytowanie metadanych urządzenia][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Dodawanie reguły dla nowego urządzenia
Dla nowo dodanego urządzenia nie zostały określone żadne reguły. W tej sekcji dodasz regułę, która powoduje uruchomienie alarmu, gdy temperatura zgłoszona przez nowe urządzenie przekroczy 47 stopni. Przed wykonaniem poniższych instrukcji zwróć uwagę na to, że zgodnie z historią telemetrii dla nowego urządzenia dostępną na pulpicie nawigacyjnym temperatura urządzenie nigdy nie przekracza 45 stopni.

1. Wróć do listy urządzeń.
2. Wybierz nowe urządzenie na **liście urządzeń**, a następnie kliknij pozycję **Dodaj regułę**, aby dodać regułę dla urządzenia.
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

## <a name="other-features"></a>Pozostałe funkcje
Portal rozwiązania pozwala wyszukiwać urządzenia o konkretnych cechach, takich jak numer modelu:

![Wyszukiwanie urządzenia][img-search]

Można wyłączyć urządzenie, a następnie je usunąć:

![Wyłączanie i usuwanie urządzenia][img-disable]

## <a name="behind-the-scenes"></a>Za kulisami
Podczas wdrażania wstępnie skonfigurowanego rozwiązania jest tworzonych wiele zasobów w ramach wybranej subskrypcji platformy Azure. Można je wyświetlić w [portalu][lnk-portal] Azure. W procesie wdrażania jest tworzona **grupa zasobów** o nazwie odpowiadającej nazwie wstępnie skonfigurowanego rozwiązania:

![Wstępnie skonfigurowane rozwiązanie w portalu Azure][img-portal]

Aby wyświetlić ustawienia danego zasobu, wybierz go z listy w grupie zasobów.

Można również wyświetlić kod źródłowy wstępnie skonfigurowanego rozwiązania. Kod źródłowy wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego znajduje się w repozytorium GitHub [azure-iot-remote-monitoring][lnk-rmgithub]:

* Folder **DeviceAdministration** zawiera kod źródłowy pulpitu nawigacyjnego.
* Folder **Simulator** zawiera kod źródłowy symulowanego urządzenia.
* Folder **EventProcessor** zawiera kod źródłowy procesu zaplecza, który obsługuje przychodzące dane telemetryczne.

Gdy wszystko będzie gotowe, możesz usunąć wstępnie skonfigurowane rozwiązanie z subskrypcji platformy Azure w witrynie [azureiotsuite.com][lnk-azureiotsuite]. Ta witryna umożliwia łatwe usunięcie wszystkich zasobów, które zostały aprowizowane po utworzeniu wstępnie skonfigurowanego rozwiązania.

> [!NOTE]
> Aby mieć pewność, że zostaną usunięte wszystkie elementy powiązane ze wstępnie skonfigurowanym rozwiązaniem, nie usuwaj grupy zasobów w portalu, lecz usuń rozwiązanie w witrynie [azureiotsuite.com][lnk-azureiotsuite].
> 
> 

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy zostało wdrożone wstępnie skonfigurowane rozwiązanie, które działa, możesz kontynuować poznawanie Pakietu IoT, czytając następujące artykuły:

* [Przewodnik po wstępnie skonfigurowanym rozwiązaniu do monitorowania zdalnego][lnk-rm-walkthrough]
* [Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem do monitorowania zdalnego][lnk-connect-rm]
* [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
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
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Nov16_HO2-->


