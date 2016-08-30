<properties
    pageTitle="Wprowadzenie do wstępnie skonfigurowanych rozwiązań | Microsoft Azure"
    description="Wykonaj czynności opisane w tym samouczku, aby dowiedzieć się, jak wdrożyć wstępnie skonfigurowane rozwiązanie Pakiet IoT Azure."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="05/25/2016"
     ms.author="dobett"/>

# Samouczek: wprowadzenie do wstępnie skonfigurowanych rozwiązań

## Wprowadzenie

[Wstępnie skonfigurowane rozwiązania][lnk-preconfigured-solutions] Pakietu IoT Azure obejmują wiele usług Azure IoT, co pozwala dostarczać kompleksowe rozwiązania, które umożliwiają implementowanie typowych scenariuszy biznesowych IoT.

W tym samouczku przedstawiono sposób aprowizowania wstępnie skonfigurowanego rozwiązania do *monitorowania zdalnego*. Dostępny jest również opis podstawowych funkcji wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego.

Do wykonania kroków tego samouczka jest potrzebna aktywna subskrypcja platformy Azure.

> [AZURE.NOTE]  Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].

## Aprowizowanie wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego

1.  Zaloguj się w witrynie [azureiotsuite.com][lnk-azureiotsuite] przy użyciu poświadczeń konta platformy Azure i kliknij pozycję **+**, aby utworzyć nowe rozwiązanie.

    > [AZURE.NOTE] Jeśli wystąpią problemy z uprawnieniami wymaganymi do przeprowadzenia aprowizacji rozwiązania, poszukaj wskazówek w temacie [Permissions on the azureiotsuite.com site][lnk-permissions] (Uprawnienia w witrynie azureiotsuite.com).

2.  Kliknij pozycję **Wybierz** na kafelku **Zdalne monitorowanie**.

3.  W polu **Nazwa rozwiązania** wprowadź nazwę wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego.

4.  W polach **Region** i **Subskrypcja** wybierz wartości, których chcesz użyć do aprowizacji rozwiązania.

5.  Kliknij pozycję **Utwórz rozwiązanie**, aby rozpocząć proces aprowizowania. Zwykle trwa to kilka minut.

## Oczekiwanie na ukończenie procesu aprowizowania

1. Kliknij kafelek swojego rozwiązania zawierający informację o stanie **aprowizacji**.
 
2. Zwróć uwagę na informację o **stanach aprowizacji** podczas wdrażania usług Azure w Twojej subskrypcji platformy Azure.

3. Po ukończeniu aprowizowania stan zmieni się na wartość **Gotowe**.

4. Kliknij kafelek, aby wyświetlić szczegóły rozwiązania w prawym okienku.

> [AZURE.NOTE] Jeśli podczas wdrażania wstępnie skonfigurowanego rozwiązania pojawią się problemy, zapoznaj się z tematami [Permissions on the azureiotsuite.com site][lnk-permissions] (Uprawnienia w witrynie azureiotsuite.com) i [FAQ][lnk-faq] (Często zadawane pytania). Jeśli problemy będą się powtarzać, utwórz żądanie pomocy w [portalu][lnk-portal].

Czy istnieją jakieś szczegóły dotyczące Twojego rozwiązania, które nie są wyświetlane, a Twoim zdaniem powinny być widoczne? Prześlij nam swoje propozycje dotyczące funkcji, korzystając ze strony [User Voice](https://feedback.azure.com/forums/321918-azure-iot) (Opinie użytkowników).

## Wyświetlanie pulpitu nawigacyjnego rozwiązania do monitorowania zdalnego

Pulpit nawigacyjny pozwala zarządzać wdrożonym rozwiązaniem. Można na przykład wyświetlać dane telemetryczne, dodawać urządzenia i konfigurować reguły.

1.  Jeśli aprowizacja została ukończona, a na kafelku wstępnie skonfigurowanego rozwiązania jest wyświetlany stan **Gotowe**, kliknij pozycję **Uruchom**, aby otworzyć portal rozwiązania do monitorowania zdalnego na nowej karcie.

    ![][img-launch-solution]

2.  Domyślnie w portalu rozwiązania jest wyświetlany jego *pulpit nawigacyjny*. W menu po lewej stronie można wybierać inne widoki.

    ![][img-dashboard]

Pulpit nawigacyjny udostępnia następujące informacje:

- Mapa zawiera lokalizację każdego urządzenia połączonego z rozwiązaniem. Przy pierwszym uruchomieniu rozwiązania dostępne są cztery symulowane urządzenia, które są implementowane jako zadania WebJob Azure. Wykreślanie informacji na mapie odbywa się za pomocą interfejsu API Map Bing.
- W panelu **Historia telemetrii** są wyświetlane niemal w czasie rzeczywistym dane telemetryczne dotyczące wilgotności i temperatury pochodzące z wybranego urządzenia oraz dane zagregowane, takie jak wilgotność maksymalna, minimalna i średnia.
- W panelu **Historia alarmów** są wyświetlane ostatnie zdarzenia alarmów, generowane w przypadku przekroczenia progu przez wartość telemetryczną. Wstępnie skonfigurowane rozwiązanie zawiera przykładowe alarmy, ale można też definiować własne.

## Wyświetlanie listy urządzeń w rozwiązaniu

Lista urządzeń zawiera wszystkie zarejestrowane urządzenia należące do rozwiązania. Urządzenia można dodawać i usuwać oraz wysyłać do nich polecenia, a także wyświetlać i edytować ich metadane.

1.  Kliknij pozycję **Urządzenia** w menu po lewej stronie, aby wyświetlić *listę urządzeń* dla tego rozwiązania.

    ![][img-devicelist]

2.  Na liście są widoczne cztery symulowane urządzenia utworzone w procesie aprowizacji.

3.  Aby wyświetlić szczegóły dotyczące danego urządzenia, kliknij odpowiednią pozycję na liście.

    ![][img-devicedetails]

W panelu **Szczegóły urządzenia** są widoczne trzy sekcje:

- Sekcja **Akcje** zawiera listę akcji, które można zastosować do urządzenia. Wyłączenie urządzenia spowoduje, że nie będzie ono mogło wysyłać danych telemetrycznych ani odbierać poleceń. Wyłączone urządzenie można ponownie włączyć. Z urządzeniem można skojarzyć regułę, która spowoduje wyzwolenie alarmu w przypadku przekroczenia progu przez wartość telemetryczną. Do urządzenia można także wysyłać polecenia. Gdy urządzenie po raz pierwszy łączy się z rozwiązaniem, wysyła listę obsługiwanych poleceń.
- W sekcji **Właściwości urządzenia** są wyświetlane metadane urządzenia. Niektóre metadane pochodzą z urządzenia (na przykład informacje o producencie), a inne są generowane przez rozwiązanie (na przykład godzina utworzenia). Metadane urządzeń można edytować.
- Sekcja **Klucze uwierzytelniania** zawiera listę kluczy używanych przez urządzenie do uwierzytelniania w rozwiązaniu.

## Wysyłanie polecenia do urządzenia

Okienko szczegółów urządzenia zawiera wszystkie polecenia obsługiwane przez to urządzenie oraz umożliwia wysyłanie poleceń do urządzenia. Gdy urządzenie jest uruchamiane po raz pierwszy, wysyła do rozwiązania informacje o obsługiwanych poleceniach.

1.  Kliknij pozycję **Polecenia** w okienku szczegółów wybranego urządzenia.

    ![][img-devicecommands]

2.  Wybierz pozycję **PingDevice** na liście poleceń.

3.  Kliknij pozycję **Wyślij polecenie**.

4.  Stan polecenia można zobaczyć w historii poleceń.

    ![][img-pingcommand]

Rozwiązanie umożliwia śledzenie stanu wszystkich wysyłanych poleceń. Na początku wynik ma wartość **Oczekiwanie**. Gdy urządzenie zgłosi wykonanie polecenia, wynik jest ustawiany na **Powodzenie**.

## Dodawanie nowego symulowanego urządzenia

1.  Wróć do listy urządzeń.

2.  Kliknij pozycję **+ Dodaj urządzenie** w lewym dolnym rogu, aby dodać nowe urządzenie.

    ![][img-adddevice]

3.  Kliknij pozycję **Dodaj nowe** na kafelku **Symulowane urządzenie**.

    ![][img-addnew]
    
    Oprócz tworzenia nowych symulowanych urządzeń można również dodawać urządzenia fizyczne — wystarczy wybrać opcję utworzenia **niestandardowego urządzenia**. Aby dowiedzieć się więcej na ten temat, zobacz artykuł [Connect your device to the IoT Suite remote monitoring preconfigured solution][lnk-connecting-devices] (Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem Pakietu IoT służącym do monitorowania zdalnego).

4.  Wybierz pozycję **Pozwól mi zdefiniować własny identyfikator urządzenia** i wprowadź unikatowy identyfikator urządzenia, np. **moje_urzadzenie_01**.

5.  Kliknij pozycję **Utwórz**.

    ![][img-definedevice]

6. W kroku 3 procedury **Dodawanie symulowanego urządzenia** kliknij pozycję **Gotowe**, aby powrócić do listy urządzeń.

7. Na liście urządzeń widać, że stan urządzenia ma wartość **Uruchomione**.

    ![][img-runningnew]

8. Na pulpicie nawigacyjnym można również wyświetlić symulowane dane telemetryczne pochodzące z nowego urządzenia:

    ![][img-runningnew-2]

## Edytowanie metadanych urządzenia

1.  Wróć do listy urządzeń.

2.  Wybierz nowe urządzenie na **liście urządzeń**, a następnie kliknij pozycję **Edytuj**, aby zmodyfikować **właściwości urządzenia**:

    ![][img-editdevice]

3. Przewiń w dół i wprowadź zmiany w wartościach szerokości i długości geograficznej. Następnie kliknij pozycję **Zapisz zmiany w rejestrze urządzenia**.

    ![][img-editdevice2]

4. Wróć do pulpitu nawigacyjnego i zwróć uwagę, że lokalizacja urządzenia na mapie zmieniła się:

    ![][img-editdevice3]

## Dodawanie reguły dla nowego urządzenia

Dla nowo dodanego urządzenia nie zostały określone żadne reguły. W tej sekcji zostanie dodana reguła, która powoduje uruchomienie alarmu, gdy temperatura zgłoszona przez nowe urządzenie przekroczy 47 stopni. Przed wykonaniem poniższych instrukcji zwróć uwagę na to, że zgodnie z historią telemetrii dla nowego urządzenia dostępną na pulpicie nawigacyjnym temperatura urządzenie nigdy nie przekracza 45 stopni.

1.  Wróć do listy urządzeń.

2.  Wybierz nowe urządzenie na **liście urządzeń**, a następnie kliknij pozycję **Dodaj regułę**, aby dodać nową regułę dla urządzenia.

3. Utwórz regułę, w której jest używane pole danych **Temperatura** i która generuje dane wyjściowe **AlarmTemp**, gdy temperatura przekroczy 47 stopni:

    ![][img-adddevicerule]

4. Aby zapisać zmiany, kliknij pozycję **Zapisz i wyświetl reguły**.

5.  Kliknij pozycję **Polecenia** w okienku szczegółów nowego urządzenia.

    ![][img-adddevicerule2]

6.  Wybierz pozycję **ChangeSetPointTemp** z listy poleceń i ustaw parametr **SetPointTemp** na wartość 45. Następnie kliknij pozycję **Wyślij polecenie**.

    ![][img-adddevicerule3]

7.  Wróć do pulpitu nawigacyjnego rozwiązania. Po pewnym czasie w okienku **Historia alarmów** pojawi się nowy wpis, gdy temperatura urządzenia przekroczy wartość progową równą 47 stopni:

    ![][img-adddevicerule4]

8. Na stronie **Reguły** pulpitu nawigacyjnego można przeglądać i edytować wszystkie reguły.

    ![][img-rules]

9. Na stronie **Akcje** pulpitu nawigacyjnego można przeglądać i edytować wszystkie akcje, które mogą być wykonywane w odpowiedzi na działanie reguły:

    ![][img-actions]

> [AZURE.NOTE] Istnieje możliwość zdefiniowania akcji, które umożliwiają wysyłanie wiadomości e-mail lub SMS w odpowiedzi na działanie reguły albo pozwalają na integrację z systemem biznesowym za pośrednictwem [aplikacji logiki][lnk-logic-apps].

## Za kulisami

Podczas wdrażania wstępnie skonfigurowanego rozwiązania jest tworzonych wiele zasobów w ramach wybranej subskrypcji platformy Azure. Można je wyświetlić w [portalu][lnk-portal] Azure. W procesie wdrażania jest tworzona **grupa zasobów** o nazwie odpowiadającej nazwie wstępnie skonfigurowanego rozwiązania:

![][img-portal]

Aby wyświetlić ustawienia danego zasobu, wybierz go z listy w grupie zasobów. Na powyższym zrzucie ekranu przedstawiono ustawienia centrum IoT używanego we wstępnie skonfigurowanym rozwiązaniu.

Można również wyświetlić kod źródłowy wstępnie skonfigurowanego rozwiązania. Kod źródłowy wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego znajduje się w repozytorium [azure-iot-remote-monitoring][lnk-rmgithub]:

- Folder **DeviceAdministration** zawiera kod źródłowy pulpitu nawigacyjnego.
- Folder **Simulator** zawiera kod źródłowy symulowanego urządzenia.
- Folder **EventProcessor** zawiera kod źródłowy procesu zaplecza, który obsługuje przychodzące dane telemetryczne.

Na koniec można usunąć wstępnie skonfigurowane rozwiązanie z subskrypcji platformy Azure w witrynie [azureiotsuite.com][lnk-azureiotsuite]. Pozwoli to łatwo usunąć wszystkie zasoby, dla których przeprowadzono aprowizację podczas tworzenia wstępnie skonfigurowanego rozwiązania.

> [AZURE.NOTE] Aby mieć pewność, że zostaną usunięte wszystkie elementy powiązane ze wstępnie skonfigurowanym rozwiązaniem, nie usuwaj grupy zasobów w portalu, lecz usuń rozwiązanie w witrynie [azureiotsuite.com][lnk-azureiotsuite].

## Następne kroki

Po utworzeniu działającego wstępnie skonfigurowanego rozwiązania można zapoznać się z następującymi przewodnikami:

-   [Guidance on customizing preconfigured solutions][lnk-customize] (Wskazówki dotyczące dostosowywania wstępnie skonfigurowanych rozwiązań)
-   [Predictive maintenance preconfigured solution overview][lnk-predictive] (Omówienie wstępnie skonfigurowanego rozwiązania do konserwacji predykcyjnej)

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

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-predictive]: iot-suite-predictive-overview.md
[lnk-connecting-devices]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-faq]: iot-suite-faq.md



<!--HONumber=jun16_HO2-->


