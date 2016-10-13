<properties
 pageTitle="Omówienie zarządzania urządzeniami | Microsoft Azure"
 description="Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/16/2016"
 ms.author="bzurcher"/>




# Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub (wersja zapoznawcza)

## Metoda zarządzania urządzeniami Azure IoT

Funkcja zarządzania urządzeniami w usłudze Azure IoT Hub udostępnia funkcje i model rozszerzeń dla urządzeń i zapleczy umożliwiające zarządzanie urządzeniami IoT w przypadku różnych urządzeń i protokołów IoT.  Do urządzeń IoT należą czujniki z rygorystycznymi ograniczeniami, pojedyncze mikrokontrolery przeznaczone do ściśle określonych celów oraz zaawansowane bramy włączające inne urządzenia i protokoły.  Ponadto rozwiązania IoT różnią się znacząco pod względem domen pionowych i aplikacji z unikatowymi przypadkami użycia dla operatorów w poszczególnych domenach.  Dzięki możliwościom zarządzania urządzeniami, wzorcom i bibliotekom kodu usługi IoT Hub rozwiązania IoT pozwalają na zarządzanie zróżnicowanymi zbiorami urządzeń i użytkowników.  

## Wprowadzenie

Kluczowym elementem procesu tworzenia prawidłowo działającego rozwiązania IoT jest opracowanie strategii dotyczącej metody bieżącego zarządzania flotą urządzeń przez operatorów. Operatorzy urządzeń IoT muszą mieć do dyspozycji proste oraz niezawodne narzędzia i aplikacje, dzięki którym będą mogli skoncentrować się na ważniejszych aspektach swojej pracy. Usługa Azure IoT Hub udostępnia bloki konstrukcyjne umożliwiające tworzenie aplikacji IoT, które ułatwiają stosowanie najważniejszych wzorców zarządzania urządzeniami.

Urządzenia są zarządzane przez usługę IoT Hub, gdy jest na nich uruchomiona prosta aplikacja nazywana agentem zarządzania urządzeniami, która bezpiecznie łączy urządzenie z chmurą. Kod agenta umożliwia operatorowi po stronie aplikacji zdalne potwierdzanie stanu urządzenia i wykonywanie operacji z zakresu zarządzania, takich jak stosowanie zmian konfiguracji sieci lub wdrażanie aktualizacji oprogramowania układowego.

## Zasady dotyczące zarządzania urządzeniami IoT

Środowisko IoT charakteryzuje się pewnymi problemami w zakresie zarządzania. Rozwiązania tych problemów muszą uwzględniać następujące zasady dotyczące zarządzania urządzeniami IoT:

![][img-dm_principles]

- **Skalowanie i automatyzacja**: środowisko IoT wymaga prostych narzędzi, które pozwalają zautomatyzować rutynowe zadania i umożliwiają stosunkowo małej grupie pracowników operacyjnych zarządzanie milionami urządzeń. Na co dzień operatorzy chcą zdalnie i zbiorczo obsługiwać operacje związane z urządzeniami oraz chcą być powiadamiani tylko o problemach wymagających ich bezpośredniej uwagi.

- **Otwartość i zgodność**: ekosystem urządzeń IoT jest bardzo zróżnicowany. Narzędzia do zarządzania muszą być zgodne z wieloma klasami urządzeń, platformami i protokołami. Operatorzy muszą mieć możliwość zapewnienia obsługi dla wszystkich urządzeń — od wbudowanych układów wykonujących pojedyncze procesy do zaawansowanych i w pełni funkcjonalnych komputerów.

- **Uwzględnianie kontekstu**: środowiska IoT są dynamiczne i nieustannie się zmieniają. Najważniejszą kwestią jest niezawodność usługi. Operacje z zakresu zarządzania urządzeniami muszą uwzględniać okna obsługi w ramach umowy SLA, stan sieci i zasilania, warunki podczas użycia i lokalizację geograficzną urządzenia w celu zagwarantowania, że przestój spowodowany przez konserwację nie wpływa na operacje o kluczowym znaczeniu dla firmy oraz że nie tworzy niebezpiecznych warunków.

- **Obsługa wielu ról**: obsługa unikatowych przepływów pracy i procesów ról operacji IoT jest niezwykle ważna. Pracownicy operacyjni muszą też zachować zgodność z ograniczeniami wewnętrznych działów IT oraz udostępniać nadzorcom i innym osobom odpowiedzialnym za zarządzanie informacje dotyczące odpowiednich operacji na urządzeniach.

## Cykl życia urządzeń IoT 

Projekty IoT są bardzo zróżnicowane, jednak istnieją wspólne wzorce zarządzania urządzeniami. W środowisku IoT platformy Azure te wzorce można wskazać w cyklu życia urządzeń, obejmującym pięć etapów:

![][img-device_lifecycle]

1. **Planowanie**: umożliwienie operatorom utworzenia schematu właściwości urządzenia, który pozwoli na łatwe oraz dokładne wykonanie zapytania dotyczące grupy urządzeń i użycie jej na potrzeby zbiorczych operacji zarządzania.

    *Powiązane bloki konstrukcyjne*: [Wprowadzenie do bliźniaków urządzeń][lnk-twins-getstarted], [Jak korzystać z właściwości bliźniaków][lnk-twin-properties]

2. **Aprowizacja**: bezpieczne uwierzytelnianie nowych urządzeń w usłudze IoT Hub oraz umożliwienie operatorom natychmiastowego poznania możliwości i bieżącego stanu urządzenia.

    *Powiązane bloki konstrukcyjne*: [Wprowadzenie do usługi IoT Hub][lnk-hub-getstarted], [Jak korzystać z właściwości bliźniaków][lnk-twin-properties]

3. **Konfigurowanie**: ułatwienie zbiorczego stosowania zmian konfiguracji i aktualizacji oprogramowania układowego na urządzeniach przy zachowaniu kondycji i bezpieczeństwa.

    *Powiązane bloki konstrukcyjne*: [Jak korzystać z właściwości bliźniaków][lnk-twin-properties], [Metody C2D][lnk-c2d-methods], [Planowanie/emitowanie zadań][lnk-jobs]

4. **Monitorowanie**: monitorowanie ogólnej kondycji floty urządzeń i stanu bieżącego wprowadzania aktualizacji w celu informowania operatorów o problemach, które mogą wymagać ich uwagi.

    *Powiązane bloki konstrukcyjne*: [Jak korzystać z właściwości bliźniaków][lnk-twin-properties]

5. **Wycofywanie**: wymiana lub likwidacja urządzeń po awarii albo po zakończeniu cyklu uaktualniania lub okresu istnienia usługi.

    *Powiązane bloki konstrukcyjne*:
    
## Wzorce zarządzania urządzeniami w usłudze IoT Hub

Usługa IoT Hub udostępnia przedstawiony poniżej zestaw początkowych wzorców zarządzania urządzeniami.  Jak pokazano w [samouczkach][lnk-get-started], możesz rozszerzać te wzorce, aby dopasować je dokładnie do danego scenariusza, oraz projektować nowe wzorce dla innych scenariuszy na podstawie tych wzorców podstawowych.

1. **Ponowne uruchomienie** — aplikacja zaplecza informuje urządzenie za pośrednictwem metody D2C o zainicjowaniu ponownego uruchamiania.  Urządzenie aktualizuje stan ponownego uruchomienia urządzenia za pomocą zgłoszonych właściwości bliźniaka urządzenia. 

    ![][img-reboot_pattern]

2. **Zresetowanie do ustawień fabrycznych** — aplikacja zaplecza informuje urządzenie za pośrednictwem metody D2C o zainicjowaniu resetowania do ustawień fabrycznych.  Urządzenie aktualizuje stan resetowania urządzenia do ustawień fabrycznych za pomocą zgłoszonych właściwości bliźniaka urządzenia.

    ![][img-facreset_pattern]

3. **Konfiguracja** — aplikacja zaplecza konfiguruje oprogramowanie uruchomione na urządzeniu za pomocą odpowiednich właściwości bliźniaka urządzenia.  Urządzenie aktualizuje stan konfiguracji urządzenia za pomocą zgłoszonych właściwości bliźniaka urządzenia. 

    ![][img-config_pattern]

4. **Aktualizacja oprogramowania układowego** — aplikacja zaplecza informuje urządzenie za pośrednictwem metody D2C o zainicjowaniu aktualizacji oprogramowania układowego.  Urządzenie inicjuje wieloetapowy proces pobierania pakietu oprogramowania układowego, stosowania pakietu oprogramowania układowego i ponownego nawiązywania połączenia z usługą IoT Hub.  W trakcie tego procesu urządzenie aktualizuje postęp i stan urządzenia do ustawień fabrycznych za pomocą zgłoszonych właściwości bliźniaka urządzenia. 

    ![][img-fwupdate_pattern]

5. **Raportowanie postępu i stanu** — zaplecze aplikacji wykonuje zapytania dotyczące bliźniaków urządzeń w zestawie urządzeń w celu raportowania stanu i postępu akcji na urządzeniu.

    ![][img-report_progress_pattern]

## Następne kroki

Bloki konstrukcyjne udostępniane przez usługę Azure IoT Hub umożliwiają deweloperom tworzenie aplikacji IoT, które spełniają szczególne wymagania operatorów urządzeń IoT na każdym etapie cyklu życia urządzenia.

Aby kontynuować zapoznawanie się z funkcjami zarządzania urządzeniami usługi Azure IoT Hub, zobacz samouczek [Zarządzanie urządzeniami w usłudze Azure IoT Hub][lnk-get-started].

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md


<!--HONumber=Oct16_HO1-->


