<properties
    pageTitle="Zarządzanie urządzeniami w usłudze IoT Hub — wprowadzenie | Microsoft Azure"
    description="Samouczek zawierający wprowadzenie do usługi Azure IoT Hub do zarządzania urządzeniami w środowisku C#. Użycie usługi Azure IoT Hub i środowiska C# z zestawami SDK IoT Microsoft Azure w celu wdrożenia zarządzania urządzeniami. "
    services="iot-hub"
    documentationCenter=".net"
    authors="juanjperez"
    manager="timlt"
    editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="juanpere"/>

# Zarządzanie urządzeniami w usłudze Azure IoT Hub przy użyciu środowiska node.js (w wersji zapoznawczej) — wprowadzenie

[AZURE.INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Wprowadzenie
Aby rozpocząć zarządzanie urządzeniami w usłudze Azure IoT Hub, musisz utworzyć centrum Azure IoT Hub, aprowizować w nim urządzenia, uruchomić wiele symulowanych urządzeń i wyświetlić te urządzenia w przykładowym interfejsie użytkownika zarządzania urządzeniami. Ten samouczek zawiera odpowiednie instrukcje.

> [AZURE.NOTE]  Musisz utworzyć nowe centrum IoT Hub, aby włączyć funkcje zarządzania urządzeniami nawet wtedy, gdy masz już istniejące centrum IoT Hub, ponieważ istniejące centra IoT Hub nie dysponują jeszcze tymi funkcjami. Gdy funkcja zarządzania urządzeniami będzie ogólnie dostępna, wszystkie istniejące centra IoT zostaną uaktualnione w celu jej uzyskania.

## Wymagania wstępne

W tym samouczku przyjęto założenie, że używasz komputera deweloperskiego z systemem Ubuntu Linux.

Aby wykonać kroki opisane w tym samouczku, musisz mieć zainstalowane następujące oprogramowanie:

- Git

- gcc (wersja 4.9 lub nowsza). Wersję zainstalowaną obecnie w środowisku możesz sprawdzić przy użyciu polecenia `gcc --version`. Informacje na temat aktualizowania używanej wersji kompilatora gcc w systemie Ubuntu 14.04 zamieszczono pod adresem <http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04>.

- [CMake](https://cmake.org/download/) (wersja 2.8 lub nowsza). Wersję zainstalowaną obecnie w środowisku możesz sprawdzić przy użyciu polecenia `cmake --version`.

- Program Node.js 6.1.0 lub nowszy.  Program Node.js dla danej platformy można zainstalować pod adresem <https://nodejs.org/>.

- Aktywna subskrypcja platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].

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
  -   Zaznacz pole wyboru **Włącz zarządzanie urządzeniami**. Jeśli pole wyboru **Włącz zarządzanie urządzeniami** nie zostanie zaznaczone, przykłady nie będą działać. Zaznaczenie pola wyboru **Włącz zarządzanie urządzeniami** powoduje utworzenie wersji zapoznawczej centrum IoT Hub obsługiwanej tylko we Wschodnich stanach USA, Europie Północnej i Azji Wschodniej, która nie jest przeznaczona dla scenariuszy produkcyjnych. Urządzeń nie można migrować do i z centrów z włączonym zarządzaniem urządzeniami.
  -   W obszarze **Lokalizacja** wybierz lokalizację hostowania centrum IoT. Zarządzanie urządzeniami usługi IoT Hub w ramach publicznej wersji zapoznawczej jest dostępne tylko we wschodnich stanach USA, Europie Północnej i Azji Wschodniej. W przyszłości będzie dostępne we wszystkich regionach.

4.  Po wybraniu opcji konfiguracji centrum IoT kliknij pozycję **Utwórz**. Utworzenie centrum IoT na platformie Azure może potrwać kilka minut. Stan operacji można sprawdzić, monitorując postęp na **tablicy startowej** lub w panelu **Powiadomienia**.

    ![][img-monitor]

5.  Po pomyślnym utworzeniu centrum IoT Hub otwórz blok nowego centrum, zanotuj wartość w polu **Nazwa hosta**, a następnie kliknij ikonę **Zasady dostępu współużytkowanego**.

    ![][img-keys]

6.  Kliknij zasady **iothubowner**, następnie skopiuj i zapisz parametry połączenia w bloku **iothubowner**. Skopiuj je do lokalizacji, do której będziesz mieć później dostęp, gdyż będziesz ich potrzebować do ukończenia pozostałej części tego samouczka.

    > [AZURE.NOTE] W scenariuszach produkcyjnych koniecznie powstrzymaj się od używania poświadczeń **iothubowner**.

    ![][img-connection]

Utworzono centrum IoT z funkcją zarządzania urządzeniami. Do ukończenia pozostałej części tego samouczka potrzebne są parametry połączenia.

## Tworzenie przykładów i urządzeń do inicjowania obsługi w usłudze IoT Hub

W tej sekcji opisano uruchamianie skryptu, który służy do tworzenia symulowanego urządzenia i przykładów oraz aprowizowania zestawu nowych tożsamości urządzeń w rejestrze urządzeń usługi IoT Hub. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze urządzeń.

Aby utworzyć przykłady i aprowizować urządzenia w usłudze IoT Hub, wykonaj poniższe kroki:

1.  Otwórz powłokę.

2.  Sklonuj repozytorium github. **Pamiętaj, aby klonować do katalogu, którego nazwa nie zawiera spacji.**

      ```
      git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
      ```

3.  Z folderu głównego, do którego sklonowano repozytorium **azure-iot-sdks**, przejdź do katalogu **azure-iot-sdks/c/build_all/linux** i wykonaj następujące polecenie, aby zainstalować wstępnie wymagane pakiety i zależne biblioteki:

      ```
      ./setup.sh
      ```


4.  Z folderu głównego, do którego zostało sklonowane repozytorium **azure-iot-sdks**, przejdź do katalogu **azure-iot-sdks/node/service/samples** i wykonaj następujące polecenie, zastępując wartość symbolu zastępczego parametrami połączenia z poprzedniej sekcji:

      ```
      ./setup.sh <IoT Hub Connection String>
      ```

Ten skrypt wykonuje następujące czynności:

1.  Uruchamia polecenie **cmake** w celu utworzenia niezbędnych plików składowych do zbudowania symulowanego urządzenia. Plik wykonywalny znajduje się w katalogu **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample**. Pamiętaj, że pliki źródłowe znajdują się w katalogu **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample**.

2.  Tworzy plik wykonywalny symulowanego urządzenia **iotdm\_simple\_sample**.

3.  Uruchamia plik `npm install` w celu zainstalowania niezbędnych pakietów.

4.  Uruchamia `node generate_devices.js` w celu zainicjowania obsługi tożsamości urządzeń w usłudze IoT Hub. Urządzenia zostały opisane w pliku **sampledevices.json**. Po zainicjowaniu obsługi urządzeń poświadczenia są przechowywane w pliku **devicecreds.txt** (znajdującym się w katalogu **azure-iot-sdks/node/service/samples**).

## Uruchamianie symulowanych urządzeń

Po dodaniu urządzeń do rejestru możesz uruchomić symulowane urządzenia zarządzane. Należy uruchomić jedno symulowane urządzenie dla każdej tożsamości urządzenia aprowizowanej w usłudze Azure IoT Hub.

Za pomocą powłoki przejdź do katalogu **azure-iot-sdks/node/service/samples** i uruchom polecenie:

  ```
  ./simulate.sh
  ```

Ten skrypt generuje polecenia, które należy uruchomić, aby włączyć plik **iotdm\_simple\_sample** dla poszczególnych urządzeń wymienionych w pliku **devicecreds.txt**. Uruchom polecenia oddzielnie z osobnego okna terminalu dla poszczególnych symulowanych urządzeń. Symulowane urządzenie będzie dalej działać, dopóki nie zamkniesz okna polecenia.

Aplikację **iotdm\_simple\_sample** utworzono przy użyciu biblioteki klienta zarządzania urządzeniami w usłudze Azure IoT Hub dla języka C, co pozwala tworzyć urządzenia IoT, którymi można zarządzać przy użyciu usługi Azure IoT Hub. Producenci urządzeń mogą używać tej biblioteki do raportowania właściwości urządzeń oraz wdrażania akcji wykonywania wymaganych przez zadania urządzeń. Ta biblioteka jest składnikiem udostępnianym w ramach zestawów SDK usługi Azure IoT Hub typu open source.

Po uruchomieniu pliku **simulate.sh** w oknie danych wyjściowych zostanie wyświetlony strumień danych. Te dane wyjściowe przedstawiają ruch przychodzący i wychodzący oraz instrukcje **printf** w funkcjach wywołania zwrotnego specyficznych dla aplikacji. Dzięki temu widoczny jest ruch przychodzący i wychodzący oraz sposób obsługi zdekodowanych pakietów przez przykładową aplikację. Gdy urządzenie połączy się z usługą IoT Hub, usługa automatycznie zaczyna obserwować zasoby na urządzeniu. Biblioteka klienta DM usługi IoT Hub wywołuje następnie wywołania zwrotne urządzeń, aby pobrać z nich najnowsze wartości.

Poniżej znajdują się dane wyjściowe z przykładowej aplikacji **iotdm\_simple\_sample**. U góry widoczny jest oznaczający pomyślne zarejestrowanie komunikat **REGISTERED** (ZAREJESTROWANE), który wskazuje urządzenie o identyfikatorze **Device11-7ce4a850** nawiązujące połączenie z usługą IoT Hub.

> [AZURE.NOTE]  Aby uzyskać mniej pełne dane wyjściowe, utwórz i uruchom konfigurację sieci sprzedaży.

![][img-output]

Pamiętaj, aby na czas wykonywania kroków z poniższych sekcji pozostawić uruchomione wszystkie symulowane urządzenia.

## Uruchamianie przykładowego interfejsu użytkownika zarządzania urządzeniami

Po aprowizowaniu centrum IoT Hub oraz uruchomieniu i zarejestrowaniu do zarządzania kilku symulowanych urządzeń można wdrożyć przykładowy interfejs użytkownika zarządzania urządzeniami. Przykładowy interfejs użytkownika zarządzania urządzeniami zapewnia praktyczny przykład wykorzystania interfejsów API zarządzania urządzeniami do utworzenia interakcyjnego środowiska użytkownika.  Aby uzyskać więcej informacji na temat przykładowego interfejsu użytkownika zarządzania urządzeniami, w tym [znanych problemów](https://github.com/Azure/azure-iot-device-management#knownissues), zobacz repozytorium GitHub [interfejsu użytkownika zarządzania urządzeniami Azure IoT][lnk-dm-github].

Aby pobrać, skompilować i uruchomić przykładowy interfejs użytkownika zarządzania urządzeniami, wykonaj następujące kroki:

1. Otwórz powłokę.

2. Potwierdź, że zainstalowano program Node.js 6.1.0 lub nowszy zgodnie z sekcją Wymagania wstępne, wpisując polecenie `node --version`.

3. Sklonuj repozytorium GitHub interfejsu użytkownika zarządzania urządzeniami Azure IoT, uruchamiając następujące polecenie w powłoce:

    ```
    git clone https://github.com/Azure/azure-iot-device-management.git
    ```
    
4. W folderze głównym sklonowanej kopii repozytorium interfejsu użytkownika zarządzania urządzeniami Azure IoT uruchom następujące polecenie, aby pobrać pakiety zależne:

    ```
    npm install
    ```

5. Po zakończeniu polecenia npm install, uruchom następujące polecenie w powłoce, aby skompilować kod:

    ```
    npm run build
    ```

6. Przy użyciu edytora tekstu otwórz plik user-config.json w katalogu głównym sklonowanego folderu. Zastąp tekst „&lt;YOUR CONNECTION STRING HERE&gt;” (UMIEŚĆ TUTAJ PARAMETRY POŁĄCZENIA) parametrami połączenia z centrum IoT Hub z poprzedniej sekcji i zapisz plik.

7. W powłoce uruchom następujące polecenie, aby uruchomić aplikację środowiska użytkownika zarządzania urządzeniami:

    ```
    npm run start
    ```

8. Po zgłoszeniu w wierszu polecenia komunikatu „Usługi zostały uruchomione” otwórz przeglądarkę sieci Web i przejdź do aplikacji zarządzania urządzeniami pod poniższym adresem, aby wyświetlić symulowane urządzenia: <http://127.0.0.1:3003>.

    ![][img-dm-ui]

Pozostaw uruchomione symulowane urządzenia i aplikację zarządzania urządzeniami przy przechodzeniu do następnego samouczka dotyczącego zarządzania urządzeniami.


## Następne kroki

Aby kontynuować wprowadzenie do usługi IoT Hub, zobacz [Getting started with the Gateway SDK][lnk-gateway-SDK] (Wprowadzenie do zestawu SDK bramy).

Aby dowiedzieć się więcej na temat funkcji zarządzania urządzeniami w usłudze Azure IoT Hub, zobacz samouczek [Explore Azure IoT Hub device management using the sample UI][lnk-sample-ui] (Eksploracja zarządzania urządzeniami w usłudze Azure IoT Hub przy użyciu przykładowego interfejsu użytkownika).

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started-node/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started-node/image2.png
[img-monitor]: media/iot-hub-device-management-get-started-node/image3.png
[img-keys]: media/iot-hub-device-management-get-started-node/image4.png
[img-connection]: media/iot-hub-device-management-get-started-node/image5.png
[img-output]: media/iot-hub-device-management-get-started-node/image6.png
[img-dm-ui]: media/iot-hub-device-management-get-started-node/dmui.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Używanie grup zasobów do zarządzania zasobami platformy Azure]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-sample-ui]: iot-hub-device-management-ui-sample.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md


<!--HONumber=sep16_HO1-->


