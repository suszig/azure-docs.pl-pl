---
title: Pakiet Azure IoT i Logic Apps | Dokumentacja firmy Microsoft
description: "Samouczek dotyczący sposobu podłączanie aplikacji logiki, aby pakiet IoT Azure dla procesu biznesowego."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 4a1db86f4b715533dfea545365eaf66de0574c5e
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Samouczek: Łączenie aplikacji logiki do rozwiązania Azure IoT pakiet monitorowania zdalnego wstępnie
[Pakiet IoT Microsoft Azure] [ lnk-internetofthings] zdalnego wstępnie skonfigurowane rozwiązanie monitorujące, jest to dobry sposób na szybkie rozpoczęcie pracy z zestawem funkcji na trasie exemplifies rozwiązania IoT. W tym samouczku przedstawiono sposób dodawania aplikacji logiki do firmy Microsoft Azure IoT pakietu zdalne monitorowanie wstępnie skonfigurowanych rozwiązań. Te kroki pokazują, jak może potrwać jeszcze bardziej rozwiązania IoT, łącząc go z procesu biznesowego.

*Jeśli szukasz wskazówki na temat sposobu monitorowania zdalnego wstępnie skonfigurowane rozwiązanie udostępniania, zobacz [samouczek: rozpoczynanie pracy z rozwiązania IoT wstępnie][lnk-getstarted].*

Przed rozpoczęciem tego samouczka, wykonaj następujące czynności:

* Monitorowania zdalnego należy wstępnie skonfigurowane rozwiązanie w Twojej subskrypcji platformy Azure.
* Utwórz konto SendGrid umożliwia wysłanie wiadomości e-mail, które wyzwala procesów biznesowych. Użytkownik może Załóż bezpłatne konto próbne w [SendGrid](https://sendgrid.com/) klikając **spróbuj bezpłatnie**. Po zarejestrowaniu bezpłatne konto próbne, musisz utworzyć [klucz interfejsu API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) w SendGrid, który przyznaje uprawnienia do wysyłania wiadomości e-mail. Należy ten klucz interfejsu API w dalszej części tego samouczka.

Do ukończenia tego samouczka, potrzebujesz programu Visual Studio 2015 lub Visual Studio 2017 r, aby zmodyfikować akcje w wewnętrznej wstępnie skonfigurowane rozwiązanie.

Zakładając, że została już przydzielona zdalne monitorowanie wstępnie skonfigurowane rozwiązanie, przejdź do grupy zasobów dla rozwiązania w [portalu Azure][lnk-azureportal]. Grupa zasobów ma taką samą nazwę jak nazwa rozwiązania została wybrana opcja podczas obsługi administracyjnej zdalnego rozwiązanie monitorowania. W tej grupie zasobów zostaną wyświetlone wszystkie aprowizowane zasoby platformy Azure rozwiązania. Poniższy zrzut ekranu przedstawia przykład **grupy zasobów** bloku zdalnego monitorowania wstępnie skonfigurowane rozwiązanie:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Aby rozpocząć, skonfigurować aplikację logiki, do korzystania z wstępnie skonfigurowanych rozwiązań.

## <a name="set-up-the-logic-app"></a>Konfigurowanie aplikacji logiki
1. Kliknij przycisk **Dodaj** w górnej części bloku grupy zasobów, korzystając z portalu Azure.
2. Wyszukaj **aplikacji logiki**, zaznacz go, a następnie kliknij przycisk **Utwórz**.
3. Wypełnianie **nazwa** i używać tego samego **subskrypcji** i **grupy zasobów** użyto podczas przydzielania zdalnego rozwiązanie monitorowania. Kliknij przycisk **Utwórz**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Po zakończeniu wdrożenia, można zauważyć, że aplikację logiki jest wymieniony jako zasób w grupie zasobów.
5. Kliknij aplikację logiki, aby przejść do bloku aplikacji logiki, wybierz opcję **pustą aplikację logiki** szablonu, aby otworzyć **projektanta aplikacji logiki**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Wybierz **żądania**. Ta akcja określa, czy przychodzące żądanie HTTP o określonych JSON w formacie ładunku czynności wyzwalacza.
7. Wklej następujący kod do schematu JSON treści żądania:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Po zapisaniu aplikację logiki, ale najpierw należy dodać akcję, możesz skopiować adres URL HTTP post.
   > 
   > 
8. Kliknij przycisk **+ nowy krok** w obszarze ręczne wyzwalacz. Następnie kliknij przycisk **Dodaj akcję**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Wyszukaj **SendGrid — wysyłanie wiadomości e-mail** i kliknij ją.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Wprowadź nazwę połączenia, na przykład **SendGridConnection**, wprowadź **klucz interfejsu API SendGrid** zostały utworzone podczas konfiguracji konta SendGrid i kliknij przycisk **Utwórz**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. Dodaj adresy e-mail, musisz być właścicielem zarówno do **z** i **do** pola. Dodaj **zdalnego alert monitorowania [DeviceId]** do **podmiotu** pola. W **treść wiadomości E-mail** pól, Dodaj **urządzenia [DeviceId] zgłosił [measurementName] o wartości [measuredValue].** Możesz dodać **[DeviceId]**, **[measurementName]**, i **[measuredValue]** , klikając w **można wstawić danych z poprzednich kroków** sekcja.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Kliknij przycisk **zapisać** w menu u góry.
13. Kliknij przycisk **żądania** wyzwalacza i skopiuj **Http Post do tego adresu URL** wartość. Ten adres URL jest potrzebne w dalszej części tego samouczka.

> [!NOTE]
> Aplikacje logiki umożliwiają uruchamianie [wiele różnych typów akcji] [ lnk-logic-apps-actions] łącznie z działaniami w usłudze Office 365. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>Konfigurowanie EventProcessor zadanie sieci Web
W tej sekcji łączysz wstępnie skonfigurowanego rozwiązania do tworzenia aplikacji logiki. Aby wykonać to zadanie, należy dodać adres URL do aplikacji logiki do akcji, która generowane, gdy wartość czujnik urządzenia przekracza próg wyzwolenia.

1. Klonowanie najnowszej wersji za pomocą klienta programu git [azure iot — zdalnego monitorowania repozytorium github][lnk-rmgithub]. Na przykład:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. W programie Visual Studio Otwórz **RemoteMonitoring.sln** z lokalną kopię repozytorium.
3. Otwórz **ActionRepository.cs** w pliku **infrastruktury\\repozytorium** folderu.
4. Aktualizacja **actionIds** słownik z **Http Post do tego adresu URL** zanotowane aplikację logiki w następujący sposób:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. Zapisz zmiany w rozwiązaniu i zamknij Visual Studio.

## <a name="deploy-from-the-command-line"></a>Wdrażanie z wiersza polecenia
W tej sekcji możesz wdrożyć aktualizowaną wersję zdalnego rozwiązanie monitorowania, aby zamienić wersję aktualnie uruchomione na platformie Azure.

1. Po [konfiguracji deweloperów] [ lnk-devsetup] instrukcje dotyczące konfigurowania środowiska do wdrożenia.
2. Aby wdrożyć lokalnie, wykonaj [lokalnego wdrożenia] [ lnk-localdeploy] instrukcje.
3. Aby wdrażać w chmurze i zaktualizować istniejące wdrożenia chmury, wykonaj [wdrożenie w chmurze] [ lnk-clouddeploy] instrukcje. Użyj nazwy oryginalnego wdrożenia jako nazwa wdrożenia. Na przykład jeśli została wywołana oryginalnego wdrożenia **demologicapp**, użyj następującego polecenia:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Po uruchomieniu skryptu kompilacji, należy użyć tego samego konta platformy Azure, subskrypcji, regionu i wystąpieniem usługi Active Directory, które jest używane podczas przydzielania rozwiązania.

## <a name="see-your-logic-app-in-action"></a>Wyświetlanie aplikacji logiki w akcji
Zdalny wstępnie skonfigurowane rozwiązanie monitorowania ma dwie reguły konfigurowane domyślnie podczas obsługi administracyjnej rozwiązania. Obie zasady są na **SampleDevice001** urządzenia:

* Temperatury > 38.00
* Wilgotność > 48.00

Wyzwalacze Reguły temperatury **podnieść Alarm** akcji i wilgotność reguły wyzwalaczy **SendMessage** akcji. Zakładając, że używasz tego samego adresu URL dla obu akcje **ActionRepository** klasy z wyzwalaczy aplikacji logiki dla każdej reguły. Obie reguły umożliwia wysłanie wiadomości e-mail do SendGrid **do** adres ze szczegółami alertu.

> [!NOTE]
> Aplikację logiki w dalszym ciągu wyzwolenia za każdym razem, ze osiągnięty jest próg. Aby uniknąć niepotrzebnych wiadomości e-mail, można wyłączyć reguły w portalu rozwiązania lub wyłączyć aplikację logiki w [portalu Azure][lnk-azureportal].
> 
> 

Oprócz odbierania wiadomości e-mail, zostanie również wyświetlony po uruchomieniu aplikacji logiki w portalu:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Następne kroki
Teraz, gdy aplikacja logiki już używane do połączenia z wstępnie skonfigurowane rozwiązanie proces biznesowy, użytkownik może dowiedzieć się więcej o opcje dostosowywania wstępnie skonfigurowanych rozwiązań:

* [Dynamiczne telemetrii za pomocą zdalnego wstępnie skonfigurowane rozwiązanie monitorowania][lnk-dynamic]
* [Urządzenie informacji metadanych w zdalnym wstępnie skonfigurowane rozwiązanie monitorowania][lnk-devinfo]

[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
