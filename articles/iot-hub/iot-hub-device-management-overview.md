---
title: "Omówienie zarządzania urządzeniami za pomocą usługi IoT Hub | Microsoft Docs"
description: "Ten artykuł zawiera omówienie zarządzania urządzeniami w usłudze Azure IoT Hub: cykl życia urządzenia w przedsiębiorstwie, ponowne uruchamianie, resetowanie do ustawień fabrycznych, aktualizacja oprogramowania układowego, konfiguracja, bliźniacze reprezentacje urządzeń, zapytania, zadania"
services: iot-hub
documentationcenter: 
author: bzurcher
manager: timlt
editor: 
ms.assetid: a367e715-55f6-4593-bd68-7863cbf0eb81
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: bzurcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c62ef9126f29350c559063b1e65e8bad4dd09af0


---
# <a name="overview-of-device-management-with-iot-hub-preview"></a>Omówienie zarządzania urządzeniami za pomocą usługi IoT Hub (wersja zapoznawcza)
## <a name="introduction"></a>Wprowadzenie
Usługa Azure IoT Hub udostępnia funkcje i model rozszerzeń, który umożliwia deweloperom urządzeń i deweloperom zaplecza tworzenie niezawodnych rozwiązań służących do zarządzania urządzeniami IoT. Istnieje wiele urządzeń IoT — od czujników z rygorystycznymi ograniczeniami i pojedynczych mikrokontrolerów przeznaczonych do ściśle określonych celów po zaawansowane bramy służące do kierowania komunikacją grup urządzeń.  Ponadto przypadki użycia i wymagania dla operatorów IoT znacznie się różnią w zależności od branży.  Pomimo tych różnic zarządzanie urządzeniami usługi IoT Hub udostępnia możliwości, wzorce i biblioteki kodu, które spełniają potrzeby różnego zestawu urządzeń i użytkowników końcowych.

Kluczowym elementem procesu tworzenia prawidłowo działającego rozwiązania IoT w przedsiębiorstwie jest opracowanie strategii dotyczącej metody bieżącego zarządzania kolekcją urządzeń przez operatorów. Operatorzy urządzeń IoT muszą mieć do dyspozycji proste oraz niezawodne narzędzia i aplikacje, dzięki którym będą mogli skoncentrować się na ważniejszych aspektach swojej pracy. Ten artykuł zawiera:

* Krótkie omówienie podejścia do zarządzania urządzeniami w usłudze Azure IoT Hub.
* Opis typowych zasad dotyczących zarządzania urządzeniami.
* Opis cyklu życia urządzenia.
* Przegląd typowych wzorców zarządzania urządzeniami.

## <a name="device-management-principles"></a>Zasady zarządzania urządzeniami
Środowisko IoT charakteryzuje się pewnymi problemami w zakresie zarządzania urządzeniami i każde rozwiązanie klasy korporacyjnej musi uwzględniać następujące zasady:

![Ilustracja dotycząca zasad zarządzania urządzeniami][img-dm_principles]

* **Skalowanie i automatyzacja**: rozwiązania IoT wymagają prostych narzędzi, które pozwalają zautomatyzować rutynowe zadania i umożliwiają stosunkowo małej grupie pracowników operacyjnych zarządzanie milionami urządzeń. Na co dzień operatorzy chcą zdalnie i zbiorczo obsługiwać operacje związane z urządzeniami oraz chcą być powiadamiani tylko o problemach wymagających ich bezpośredniej uwagi.
* **Otwartość i zgodność**: ekosystem urządzeń IoT jest bardzo zróżnicowany. Narzędzia do zarządzania muszą być zgodne z wieloma klasami urządzeń, platformami i protokołami. Operatorzy muszą mieć możliwość zapewnienia obsługi dla wielu typów urządzeń — od wbudowanych układów wykonujących pojedyncze procesy po zaawansowane i w pełni funkcjonalne komputery.
* **Uwzględnianie kontekstu**: środowiska IoT są dynamiczne i nieustannie się zmieniają. Najważniejszą kwestią jest niezawodność usługi. Operacje z zakresu zarządzania urządzeniami muszą uwzględniać okna obsługi w ramach umowy SLA, stan sieci i zasilania, warunki podczas użycia i lokalizację geograficzną urządzenia w celu zagwarantowania, że przestój spowodowany przez konserwację nie wpływa na operacje o kluczowym znaczeniu dla firmy oraz że nie tworzy niebezpiecznych warunków.
* **Obsługa wielu ról**: obsługa unikatowych przepływów pracy i procesów ról operacji IoT jest niezwykle ważna. Pracownicy operacyjni muszą zachować zgodność z ograniczeniami wewnętrznych działów IT.  Muszą również znaleźć niezawodne sposoby udostępniania nadzorcom i innym osobom odpowiedzialnym za zarządzanie informacji dotyczących odpowiednich operacji na urządzeniach.

## <a name="device-lifecycle"></a>Cykl życia urządzenia
Istnieje zestaw ogólnych etapów zarządzania urządzeniami, które są wspólne dla wszystkich projektów IoT. W usłudze Azure IoT istnieje pięć etapów w ramach cyklu życia urządzenia IoT:

![Pięć faz cyklu życia urządzenia usługi Azure IoT: planowanie, aprowizowanie, konfigurowanie, monitorowanie, wycofywanie][img-device_lifecycle]

W każdym z tych pięciu etapów istnieje kilka wymagań dotyczących operatora urządzenia, które powinny zostać spełnione, aby zapewnić kompletne rozwiązanie:

* **Planowanie**: umożliwienie operatorom utworzenia schematu metadanych urządzenia, który pozwoli na łatwe oraz dokładne wykonanie zapytania dotyczącego grupy urządzeń i użycie jej na potrzeby zbiorczych operacji zarządzania. Bliźniaczej reprezentacji urządzenia można użyć do przechowywania metadanych tego urządzenia w postaci tagów i właściwości.
  
    *Dalsze informacje*: [Wprowadzenie do zarządzania bliźniaczymi reprezentacjami urządzeń][lnk-twins-getstarted], [Opis bliźniaczych reprezentacji urządzeń][lnk-twins-devguide], [Jak korzystać z właściwości bliźniaczych reprezentacji urządzeń][lnk-twin-properties]
* **Aprowizacja**: bezpieczna aprowizacja nowych urządzeń w usłudze IoT Hub oraz umożliwienie operatorom natychmiastowego wykrywania możliwości urządzeń.  Za pomocą rejestru urządzeń usługi IoT Hub można tworzyć elastyczne tożsamości i poświadczenia urządzeń. Aprowizacja może być wykonywana zbiorczo przy użyciu zadania. Możliwe jest tworzenie urządzeń w taki sposób, aby raportowały swoje możliwości i warunki za pośrednictwem właściwości urządzeń w bliźniaczej reprezentacji urządzenia.
  
    *Dalsze informacje*: [Zarządzanie tożsamościami urządzeń][lnk-identity-registry], [Zbiorcze zarządzanie tożsamościami urządzeń][lnk-bulk-identity], [Jak korzystać z właściwości bliźniaczych reprezentacji urządzeń][lnk-twin-properties]
* **Konfigurowanie**: ułatwienie zbiorczego stosowania zmian konfiguracji i aktualizacji oprogramowania układowego na urządzeniach przy zachowaniu kondycji i bezpieczeństwa. Wykonaj te operacje zarządzania urządzeniami zbiorczo, używając odpowiednich właściwości lub bezpośrednich metod i zadań emisji.
  
    *Dalsze informacje*: [Używanie metod bezpośrednich][lnk-c2d-methods], [Wywoływanie metody bezpośredniej względem urządzenia][lnk-methods-devguide], [Jak korzystać z właściwości bliźniaczych reprezentacji urządzeń][lnk-twin-properties], [Planowanie i emitowanie zadań][lnk-jobs], [Planowanie zadań na wielu urządzeniach][lnk-jobs-devguide]
* **Monitorowanie**: monitorowanie ogólnej kondycji kolekcji urządzeń i stanu trwających operacji w celu ostrzegania operatorów o problemach, które mogą wymagać ich uwagi.  Zastosuj bliźniaczą reprezentacją urządzenia, aby umożliwić urządzeniom raportowanie w czasie rzeczywistym warunków pracy i stanu operacji aktualizacji. Twórz zaawansowane raporty pulpitu nawigacyjnego, które służą do udostępniania najważniejszych w danym momencie problemów przy użyciu zapytań bliźniaczych reprezentacji urządzeń.
  
    *Dalsze informacje*: [Jak korzystać z właściwości bliźniaczych reprezentacji urządzeń][lnk-twin-properties], [Język zapytań usługi IoT Hub dla bliźniaczych reprezentacji i zadań][lnk-query-language]
* **Wycofywanie**: wymiana lub likwidacja urządzeń po awarii albo po zakończeniu cyklu uaktualniania lub okresu istnienia usługi.  Użyj bliźniaczej reprezentacji urządzenia, aby zarządzać informacjami o urządzeniu, jeśli trwa zastępowanie urządzenia fizycznego lub jeśli jest ono archiwizowane w przypadku wycofywania. Za pomocą rejestru urządzeń usługi IoT Hub można bezpiecznie odwołać tożsamości i poświadczenia urządzeń.
  
    *Dalsze informacje*: [Jak korzystać z właściwości bliźniaczych reprezentacji urządzeń][lnk-twin-properties], [Zarządzanie tożsamościami urządzeń][lnk-identity-registry]

## <a name="device-management-patterns"></a>Wzorce zarządzania urządzeniami
Usługa IoT Hub udostępnia przedstawiony poniżej zestaw wzorców zarządzania urządzeniami.  W [samouczkach dotyczących zarządzania urządzeniami][lnk-get-started] znajduje się bardziej szczegółowy opis sposobu rozszerzenia tych wzorców w celu dopasowania ich do danego scenariusza oraz sposobu projektowania nowych wzorców na podstawie tych szablonów podstawowych.

* **Ponowne uruchomienie** — aplikacja zaplecza informuje urządzenie za pośrednictwem metody bezpośredniej o zainicjowaniu ponownego uruchamiania.  Urządzenie aktualizuje stan ponownego uruchomienia urządzenia za pomocą zgłoszonych właściwości bliźniaka urządzenia.
  
    ![Ilustracja dotycząca wzorca ponownego uruchamiania zarządzania urządzeniami][img-reboot_pattern]
* **Zresetowanie do ustawień fabrycznych** — aplikacja zaplecza informuje urządzenie za pośrednictwem metody bezpośredniej o zainicjowaniu resetowania do ustawień fabrycznych.  Urządzenie aktualizuje stan resetowania urządzenia do ustawień fabrycznych za pomocą zgłoszonych właściwości bliźniaka urządzenia.
  
    ![Ilustracja dotycząca wzorca resetowania urządzenia do ustawień fabrycznych zarządzania urządzeniami][img-facreset_pattern]
* **Konfiguracja** — aplikacja zaplecza konfiguruje oprogramowanie uruchomione na urządzeniu za pomocą odpowiednich właściwości bliźniaka urządzenia.  Urządzenie aktualizuje stan konfiguracji urządzenia za pomocą zgłoszonych właściwości bliźniaka urządzenia.
  
    ![Ilustracja dotycząca wzorca konfiguracji zarządzania urządzeniami][img-config_pattern]
* **Aktualizacja oprogramowania układowego** — aplikacja zaplecza informuje urządzenie za pośrednictwem metody bezpośredniej o zainicjowaniu aktualizacji oprogramowania układowego.  Urządzenie inicjuje wieloetapowy proces pobierania obrazu oprogramowania układowego, stosowania obrazu oprogramowania układowego i ponownego nawiązywania połączenia z usługą IoT Hub.  W trakcie tego procesu urządzenie aktualizuje postęp i stan urządzenia do ustawień fabrycznych za pomocą zgłoszonych właściwości bliźniaka urządzenia.
  
    ![Ilustracja dotycząca wzorca aktualizacji oprogramowania układowego zarządzania urządzeniami][img-fwupdate_pattern]
* **Raportowanie postępu i stanu** — zaplecze aplikacji wykonuje zapytania dotyczące bliźniaczych reprezentacji urządzeń w zestawie urządzeń w celu raportowania stanu i postępu akcji na urządzeniu.
  
    ![Ilustracja dotycząca postępu i stanu raportowania zarządzania urządzeniami][img-report_progress_pattern]

## <a name="next-steps"></a>Następne kroki
Funkcji, wzorców i bibliotek kodu udostępnianych przez usługę IoT Hub na potrzeby zarządzania urządzeniami możesz użyć do tworzenia aplikacji IoT, które spełniają wymagania operatora IoT przedsiębiorstwa na każdym etapie cyklu życia urządzenia.

Aby kontynuować zapoznawanie się z funkcjami zarządzania urządzeniami usługi IoT Hub, zobacz samouczek [Wprowadzenie do zarządzania urządzeniami][lnk-get-started].

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md



<!--HONumber=Nov16_HO2-->


