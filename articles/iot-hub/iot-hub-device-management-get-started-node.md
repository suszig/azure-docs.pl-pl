<properties
    pageTitle="Zarządzanie urządzeniami w usłudze IoT Hub — wprowadzenie | Microsoft Azure"
    description="Samouczek zawierający wprowadzenie do usługi Azure IoT Hub do zarządzania urządzeniami w środowisku C#. Użycie usługi Azure IoT Hub i środowiska C# z zestawami SDK IoT Microsoft Azure w celu wdrożenia zarządzania urządzeniami. "
    services="iot-hub"
    documentationCenter=".net"
    authors="ellenfosborne"
    manager="timlt"
    editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="elfarber"/>

# Zarządzanie urządzeniami w usłudze Azure IoT Hub przy użyciu środowiska node.js (w wersji zapoznawczej) — wprowadzenie

[AZURE.INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Wprowadzenie
Aby rozpocząć zarządzanie urządzeniami w usłudze Azure IoT Hub, musisz utworzyć centrum IoT na platformie Azure, zainicjować w nim obsługę urządzeń i uruchomić wiele symulowanych urządzeń. Ten samouczek zawiera odpowiednie instrukcje.

> [AZURE.NOTE]  Musisz utworzyć nowe centrum IoT, aby włączyć funkcje zarządzania urządzeniami, nawet jeśli masz już istniejące centrum IoT, ponieważ istniejące centra IoT nie mają jeszcze funkcji zarządzania urządzeniami. Gdy funkcja zarządzania urządzeniami będzie ogólnie dostępna, wszystkie istniejące centra IoT zostaną uaktualnione w celu jej uzyskania.

## Wymagania wstępne

Aby wykonać kroki opisane w tym samouczku, trzeba zainstalować następujące elementy:

- Git
- węzeł
- npm
- CMake (wersja 2.8 lub nowsza). Zainstaluj program CMake z witryny <https://cmake.org/download/>. Koniecznie zaznacz pole wyboru, aby dodać program CMake do bieżącej zmiennej PATH użytkownika.
- Aktywna subskrypcja platformy Azure.

    Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].

## Tworzenie centrum IoT z funkcją zarządzania urządzeniami

Trzeba utworzyć centrum IoT, z którym będą mogły się połączyć symulowane urządzenia. W poniższych krokach przedstawiono sposób wykonania tego zadania przy użyciu portalu Azure.

1.  Zaloguj się do [Azure portal].
2.  Na pasku przechodzenia kliknij przycisk **Nowe**, kliknij opcję **Internet rzeczy**, a następnie kliknij opcję **Azure IoT Hub**.

    ![][img-new-hub]

3.  W bloku **Centrum IoT** wybierz konfigurację centrum IoT.

    ![][img-configure-hub]

  -   W polu **Nazwa** wprowadź nazwę centrum IoT. Jeśli **nazwa** jest prawidłowa i dostępna, w polu **Nazwa** widoczny jest zielony znacznik.
  -   Wybierz pozycję **Warstwa cenowa i warstwa skali**. W tym samouczku można użyć dowolnej warstwy.
  -   W obszarze **Grupa zasobów** utwórz nową grupę zasobów lub wybierz istniejącą. Więcej informacji można znaleźć w temacie [Using resource groups to manage your Azure resources] (Używanie grup zasobów do zarządzania zasobami platformy Azure).
  -   Zaznacz pole wyboru **Włącz zarządzanie urządzeniami**.
  -   W obszarze **Lokalizacja** wybierz lokalizację hostowania centrum IoT. Zarządzanie urządzeniami usługi IoT Hub w ramach publicznej wersji zapoznawczej jest dostępne tylko we wschodnich stanach USA, Europie Północnej i Azji Wschodniej. W przyszłości będzie dostępne we wszystkich regionach.

  > [AZURE.NOTE]  Jeśli pole wyboru **Włącz zarządzanie urządzeniami** nie zostanie zaznaczone, przykłady nie będą działać.

4.  Po wybraniu opcji konfiguracji centrum IoT kliknij pozycję **Utwórz**. Utworzenie centrum IoT na platformie Azure może potrwać kilka minut. Stan operacji można sprawdzić, monitorując postęp na **tablicy startowej** lub w panelu **Powiadomienia**.

    ![][img-monitor]

5.  Po pomyślnym utworzeniu centrum IoT otwórz blok nowego centrum, zanotuj **nazwę hosta**, a następnie kliknij ikonę **Klucze**.

    ![][img-keys]

6.  Kliknij zasady **iothubowner**, następnie skopiuj i zapisz parametry połączenia w bloku **iothubowner**. Skopiuj je do lokalizacji, do której będziesz mieć później dostęp, gdyż będziesz ich potrzebować do ukończenia pozostałej części tego samouczka.

    > [AZURE.NOTE] W scenariuszach produkcyjnych koniecznie powstrzymaj się od używania poświadczeń **iothubowner**.

    ![][img-connection]

Utworzono centrum IoT z funkcją zarządzania urządzeniami. Do ukończenia pozostałej części tego samouczka potrzebne będą parametry połączenia.

## Tworzenie przykładów i urządzeń do inicjowania obsługi w usłudze IoT Hub

W tej sekcji opisano uruchamianie skryptu, który służy do tworzenia symulowanego urządzenia oraz przykładów i inicjowania obsługi zestawu nowych tożsamości urządzeń w rejestrze urządzeń usługi IoT Hub. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze urządzeń.

Aby utworzyć przykłady i zainicjować obsługę urządzeń w usłudze IoT Hub, wykonaj kroki poniżej:

1.  Otwórz terminal.

2.  Sklonuj repozytorium github. **Pamiętaj, aby klonować do katalogu, którego nazwa nie zawiera spacji.**

      ```
      git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
      ```

3.  Z folderu głównego, do którego zostało sklonowane repozytorium **azure-iot-sdks**, przejdź do katalogu **azure-iot-sdks/node/service/samples** i uruchom następujący skrypt, zastępując wartość symbolu zastępczego parametrami połączenia z poprzedniej sekcji:

      ```
      setup.bat <IoT Hub Connection String>
      ```

Ten skrypt wykonuje następujące czynności:

1.  Uruchamia polecenie **cmake** w celu utworzenia niezbędnych plików składowych do zbudowania symulowanego urządzenia. Plik wykonywalny znajduje się w katalogu **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample**. Pamiętaj, że pliki źródłowe znajdują się w katalogu **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample**.

2.  Tworzy plik wykonywalny symulowanego urządzenia **iotdm\_simple\_sample**.

3.  Uruchamia plik ``` npm install ``` w celu zainstalowania niezbędnych pakietów.

4.  Uruchamia ```node generate_devices.js``` w celu zainicjowania obsługi tożsamości urządzeń w usłudze IoT Hub. Urządzenia zostały opisane w pliku **sampledevices.json**. Po zainicjowaniu obsługi urządzeń poświadczenia są przechowywane w pliku **devicecreds.txt** (znajdującym się w katalogu **azure-iot-sdks/node/service/samples**).

## Uruchamianie symulowanych urządzeń

Po dodaniu urządzeń do rejestru możesz uruchomić symulowane urządzenia zarządzane. Należy uruchomić jedno symulowane urządzenie dla każdej tożsamości urządzenia obsługiwanej w usłudze Azure IoT Hub.

Za pomocą terminalu w katalogu **azure-iot-sdks/node/service/samples** uruchom następujący skrypt:

  ```
  simulate.sh
  ```

Ten skrypt generuje polecenia, które należy uruchomić, aby włączyć plik **iotdm\_simple\_sample** dla poszczególnych urządzeń wymienionych w pliku **devicecreds.txt**. Uruchom polecenia oddzielnie z osobnego okna terminalu dla poszczególnych symulowanych urządzeń. Symulowane urządzenie będzie dalej działać, dopóki nie zamkniesz okna polecenia.

Aplikację **iotdm\_simple\_sample** utworzono przy użyciu biblioteki klienta zarządzania urządzeniami w usłudze Azure IoT Hub dla języka C, co pozwala tworzyć urządzenia IoT, którymi można zarządzać przy użyciu usługi Azure IoT Hub. Producenci urządzeń mogą używać tej biblioteki do raportowania właściwości urządzeń oraz wdrażania akcji wykonywania wymaganych przez zadania urządzeń. Ta biblioteka jest składnikiem udostępnianym w ramach zestawów SDK usługi Azure IoT Hub typu open source.

Po uruchomieniu pliku **simulate.sh** w oknie danych wyjściowych zostanie wyświetlony strumień danych. Te dane wyjściowe przedstawiają ruch przychodzący i wychodzący oraz instrukcje **printf** w funkcjach wywołania zwrotnego określonych dla aplikacji. Dzięki temu widoczny jest ruch przychodzący i wychodzący oraz sposób obsługi zdekodowanych pakietów przez przykładową aplikację. Gdy urządzenie połączy się z usługą IoT Hub, usługa automatycznie zaczyna obserwować zasoby na urządzeniu. Biblioteka klienta DM usługi IoT Hub wywołuje następnie wywołania zwrotne urządzeń, aby pobrać z nich najnowsze wartości.

Poniżej znajdują się dane wyjściowe z przykładowej aplikacji **iotdm\_simple\_sample**. U góry widoczny jest pomyślny komunikat **ZAREJESTROWANE** zawierający urządzenie o identyfikatorze **Device11-7ce4a850** nawiązujące połączenie z usługą IoT Hub.

> [AZURE.NOTE]  Aby uzyskać mniej pełne dane wyjściowe, utwórz i uruchom konfigurację sieci sprzedaży.

![][img-output]

Pamiętaj, aby po zakończeniu samouczków w sekcji „Następne kroki” pozostawić wszystkie symulowane urządzenia uruchomione.

## Następne kroki

Więcej informacji na temat funkcji zarządzania urządzeniami w usłudze Azure IoT Hub znajduje się w samouczkach poniżej:

- [How to use the device twin] (Jak używać bliźniaka urządzenia)[lnk-tutorial-twin]

- [How to find device twins using queries] (Jak znaleźć bliźniaki urządzenia przy użyciu zapytań)[lnk-tutorial-queries]

- [How to use device jobs to update device firmware] (Jak używać zadań urządzenia do aktualizacji jego sprzętu)[lnk-tutorial-jobs]

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started/image2.png
[img-monitor]: media/iot-hub-device-management-get-started/image3.png
[img-keys]: media/iot-hub-device-management-get-started/image4.png
[img-connection]: media/iot-hub-device-management-get-started/image5.png
[img-output]: media/iot-hub-device-management-get-started/image6.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Używanie grup zasobów do zarządzania zasobami platformy Azure]: ../azure-portal/resource-group-portal.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-tutorial-queries]: iot-hub-device-management-device-query.md
[lnk-tutorial-jobs]: iot-hub-device-management-device-jobs.md



<!--HONumber=jun16_HO2-->


