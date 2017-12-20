---
title: "Aprowizowanie symulowanego urządzenia X.509 dla usługi Azure IoT Hub | Microsoft Docs"
description: "Przewodnik Szybki start platformy Azure — tworzenie i aprowizowanie symulowanego urządzenia X.509 za pomocą usługi Azure IoT Hub Device Provisioning Service"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/08/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: a51debfc80c28cd7d7e271448e30619f2bc72f86
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-and-provision-an-x509-simulated-device-using-iot-hub-device-provisioning-service"></a>Tworzenie i aprowizowanie symulowanego urządzenia X.509 za pomocą usługi IoT Hub Device Provisioning
> [!div class="op_single_selector"]
> * [TPM](quick-create-simulated-device.md)
> * [X.509](quick-create-simulated-device-x509.md)

Te kroki pokazują, jak zasymulować urządzenie X.509 na maszynie deweloperskiej z systemem operacyjnym Windows OS i użyć przykładowego kodu do połączenia tego symulowanego urządzenia z usługą Device Provisioning Service i Twoim centrum IoT Hub. 

Pamiętaj, aby wcześniej wykonać kroki przedstawione w części [Konfigurowanie usługi IoT Hub Device Provisioning Service za pomocą witryny Azure Portal](./quick-setup-auto-provision.md).

<a id="setupdevbox"></a>
## <a name="prepare-the-development-environment"></a>Przygotowywanie środowiska deweloperskiego 

1. Upewnij się, że masz na swojej maszynie zainstalowany program Visual Studio 2015 lub [Visual Studio 2017](https://www.visualstudio.com/vs/). Musisz mieć włączony pakiet roboczy „Programowanie aplikacji klasycznych w języku C++” dla instalacji programu Visual Studio.

2. Pobierz i zainstaluj [system kompilacji CMake](https://cmake.org/download/).

3. Upewnij się, że na swojej maszynie masz zainstalowane oprogramowanie `git` i że jest ono dodane do zmiennych środowiskowych dostępnych z okna poleceń. Zobacz stronę z [narzędziami klienckimi Git organizacji Software Freedom Conservancy](https://git-scm.com/download/), aby uzyskać najnowszą wersję narzędzi `git` do zainstalowania, które obejmują powłokę **Git Bash**, czyli aplikację wiersza polecenia, która może służyć do interakcji z lokalnym repozytorium Git. 

4. Otwórz wiersz polecenia lub powłokę Git Bash. Sklonuj repozytorium GitHub dla przykładu kodu symulacji urządzenia:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

5. Utwórz folder w swojej lokalnej kopii tego repozytorium GitHub dla procesu kompilacji CMake. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

6. Uruchom następujące polecenie, aby utworzyć rozwiązanie programu Visual Studio służące do aprowizacji klienta.

    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON ..
    ```


## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Tworzenie wpisu rejestracji urządzenia w usłudze Device Provisioning Service

1. Otwórz rozwiązanie wygenerowane w folderze programu *cmake* o nazwie `azure_iot_sdks.sln` i skompiluj je w programie Visual Studio.

2. Kliknij prawym przyciskiem myszy projekt **dice\_device\_enrollment** w folderze **Provision\_Samples**, a następnie wybierz pozycję **Ustaw jako projekt startowy**. Uruchom rozwiązanie. W oknie danych wyjściowych po wyświetleniu monitu wprowadź wartość `i`, aby przeprowadzić rejestrację indywidualną. W oknie danych wyjściowych zostanie wyświetlony lokalnie wygenerowany certyfikat X.509 dla symulowanego urządzenia. Skopiuj do schowka dane wyjściowe rozpoczynające się od wiersza *-----BEGIN CERTIFICATE-----* i kończące się wierszem *-----END PUBLIC KEY-----*, a następnie upewnij się, że oba te wiersze również zostały skopiowane. 
 
3. Utwórz plik o nazwie **_X509testcertificate.pem_** na maszynie z systemem Windows, otwórz go w wybranym edytorze, a następnie skopiuj zawartość schowka do tego pliku. Zapisz plik. 

4. Zaloguj się do witryny Azure Portal, kliknij przycisk **Wszystkie zasoby** w menu po lewej stronie, a następnie otwórz używaną usługę aprowizacji.

4. W bloku podsumowania usługi Device Provisioning Service wybierz pozycję **Zarządzaj rejestracjami**. Wybierz kartę **Indywidualne rejestracje** i kliknij u góry przycisk **Dodaj**. 

5. W obszarze **Dodaj wpis listy rejestracji** wprowadź następujące informacje:
    - Wybierz opcję **X.509** jako *Mechanizm* poświadczania tożsamości.
    - W obszarze *Plik PEM lub CER certyfikatu* wybierz plik certyfikatu **_X509testcertificate.pem_** utworzony w poprzednich krokach za pomocą widżetu *Eksploratora plików*.
    - Opcjonalnie można podać następujące informacje:
        - Wybierz centrum IoT połączone z Twoją usługą aprowizacji.
        - Wprowadź unikatowy identyfikator urządzenia. Nadając nazwę urządzeniu, unikaj korzystania z danych poufnych. 
        - Zaktualizuj pole **Początkowy stan bliźniaczej reprezentacji urządzenia** za pomocą wybranej konfiguracji początkowej dla urządzenia.
    - Gdy skończysz, kliknij przycisk **Zapisz**. 

    ![Wprowadzanie informacji o rejestracji urządzenia X.509 w bloku portalu](./media/quick-create-simulated-device-x509/enter-device-enrollment.png)  

   Po pomyślnej rejestracji urządzenie X.509 jest wyświetlane jako **riot-device-cert** w kolumnie *Identyfikator rejestracji* na karcie *Indywidualne rejestracje*. 



<a id="firstbootsequence"></a>
## <a name="simulate-first-boot-sequence-for-the-device"></a>Symulowanie sekwencji pierwszego uruchamiania dla urządzenia

1. W witrynie Azure Portal wybierz blok **Przegląd** dla swojej usługi Device Provisioning Service, a następnie zapisz wartość **_Identyfikator zakresu_**.

    ![Wyodrębnianie informacji o punkcie końcowym usługi DPS z bloku portalu](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. W programie Visual Studio na używanej maszynie przejdź do przykładowego projektu o nazwie **prov\_dev\_client\_sample** w folderze **Provision\_Samples**, a następnie otwórz plik **prov\_dev\_client\_sample.c**.

3. Przypisz wartość _Identyfikator zakresu_ do zmiennej `scope_id`. 

    ```c
    static const char* scope_id = "[ID Scope]";
    ```

4. Kliknij prawym przyciskiem myszy projekt **prov\_dev\_client\_sample**, a następnie wybierz pozycję **Ustaw jako projekt startowy**. Uruchom przykład. Zwróć uwagę na komunikaty symulujące uruchamianie urządzenia i łączenie z usługą Device Provisioning Service w celu pobrania informacji z Twojego centrum IoT. Wyszukaj komunikat informujący o pomyślnej rejestracji w centrum: *Informacje o rejestracji otrzymane od usługi: adres_URL_Twojego_centrum!*. Zamknij okno po wyświetleniu monitu.

5. W portalu przejdź do centrum IoT Hub połączonego z usługą aprowizacji, a następnie otwórz blok **Device Explorer**. Po pomyślnej aprowizacji symulowanego urządzenia X.509 w centrum identyfikator urządzenia jest wyświetlany w bloku **Device Explorer** z pozycją *STATUS* (stan) ustawioną na wartość **enabled** (włączone). Należy pamiętać, że konieczne może być kliknięcie przycisku **Odśwież** znajdującego się u góry, jeśli blok został otwarty przed uruchomieniem przykładowej aplikacji urządzenia. 

    ![Urządzenie jest rejestrowane w centrum IoT](./media/quick-create-simulated-device/hub-registration.png) 

> [!NOTE]
> Jeśli zmienisz wartość w polu *Początkowy stan bliźniaczej reprezentacji urządzenia* z domyślnej na inną we wpisie rejestracji dla Twojego urządzenia, może to spowodować pobranie z centrum żądanego stanu reprezentacji bliźniaczej i odpowiednie do niego działanie. Aby uzyskać więcej informacji, zobacz [Opis bliźniaczej reprezentacji urządzenia w usłudze IoT Hub oraz sposoby jej używania](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli planujesz dalszą pracę z przykładem klienta urządzenia i eksplorowanie go, nie czyść zasobów utworzonych w ramach tego przewodnika Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj poniższe kroki, aby usunąć wszystkie zasoby utworzone w ramach tego przewodnika Szybki start.

1. Zamknij okno danych wyjściowych przykładu klienta urządzenia na swojej maszynie.
1. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoją usługę Device Provisioning Service. U góry bloku **Wszystkie zasoby** kliknij pozycję **Usuń**.  
1. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoje centrum IoT. U góry bloku **Wszystkie zasoby** kliknij pozycję **Usuń**.  

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start na Twojej maszynie z systemem Windows utworzono symulowane urządzenie X.509, które zostało następnie zaaprowizowane do Twojego centrum IoT Hub przy użyciu usługi Azure IoT Hub Device Provisioning Service. Aby uzyskać dokładne informacje na temat aprowizowania urządzeń, przejdź do samouczka poświęconego konfiguracji usługi Device Provisioning Service w witrynie Azure portal. 

> [!div class="nextstepaction"]
> [Samouczki dla usługi Azure IoT Hub Device Provisioning Service](./tutorial-set-up-cloud.md)
