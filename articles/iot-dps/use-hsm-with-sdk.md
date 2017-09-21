---
title: "Instrukcje dotyczące platformy Azure — korzystanie z innego modułu HSM z zestawem SDK klienta usługi Device Provisioning Service | Microsoft Docs"
description: "Instrukcje dotyczące platformy Azure — korzystanie z innego modułu HSM z urządzeniami fizycznymi i symulatorami przy użyciu zestawu SDK klienta usługi Device Provisioning Service"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 08/28/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: 
ms.devlang: na
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: a5649ab873993d20803cb01a4b0ecc278c3ce16c
ms.contentlocale: pl-pl
ms.lasthandoff: 09/14/2017

---

# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk"></a>Jak używać różnych sprzętowych modułów zabezpieczeń z zestawem SDK klienta usługi Device Provisioning Service
Te kroki pokazują, jak używać różnych [sprzętowych modułów zabezpieczeń (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) z zestawem SDK klienta usługi Device Provisioning Service w języku C za pomocą urządzenia fizycznego i symulatora.  Usługa aprowizowania obsługuje dwa tryby uwierzytelniania: X**.**509 i moduł TPM.

## <a name="prerequisites"></a>Wymagania wstępne

Przygotuj środowisko deweloperskie zgodnie z sekcją „Prepare the development environment” (Przygotowywanie środowiska deweloperskiego) w przewodniku [Create and provision simulated device] (Tworzenie i aprowizowanie symulowanego urządzenia) (./quick-create-simulated-device.md).

## <a name="enable-authentication-with-different-hsms"></a>Włączanie uwierzytelniania przy użyciu różnych modułów HSM

Tryb uwierzytelniania (X**.**509 lub moduł TPM) musi zostać włączony dla urządzenia fizycznego lub symulatora, aby można było zarejestrować je w witrynie Azure Portal.  Przejdź do folderu głównego elementu azure-iot-sdk-c.  Uruchom odpowiednie polecenie, w zależności od wybranego trybu uwierzytelniania.

### <a name="use-x509-with-simulator"></a>Używanie trybu X**.**509 z symulatorem

Usługa aprowizowania jest publikowana z emulatorem DICE (Device Identity Composition Engine), który generuje certyfikat X**.**509 na potrzeby uwierzytelniania urządzenia.  Uruchom następujące polecenie, aby włączyć uwierzytelnianie X**.**509:

```
cmake -Ddps_auth_type=x509 ..
```

Informacje na temat sprzętu z aparatem DICE można znaleźć [tutaj](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>Używanie trybu X**.**509 ze sprzętem

Usługa aprowizowania może być używana z trybem X**.**509 na innym sprzęcie.  Do ustanowienia połączenia niezbędny jest interfejs między sprzętem i zestawem SDK.  Skontaktuj się z producentem modułu HSM, aby uzyskać informacje na temat interfejsu.

### <a name="use-tpm"></a>Używanie modułu TPM

Usługę aprowizowania można połączyć z mikroukładami modułu TPM sprzętu z systemem Windows i Linux przy użyciu tokenu sygnatury dostępu współdzielonego.  Uruchom następujące polecenie, aby włączyć uwierzytelnianie przy użyciu modułu TPM:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Używanie modułu TPM z symulatorem

Jeśli nie masz urządzenia z mikroukładami modułu TPM, możesz użyć symulatora w celach deweloperskich w systemie operacyjnym Windows.  Uruchom następujące polecenie, aby włączyć uwierzytelnianie przy użyciu modułu TPM i uruchomić symulator modułu TPM:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>Kompilowanie zestawu SDK
Skompiluj zestaw SDK przed utworzeniem rejestracji urządzeń.

### <a name="linux"></a>Linux
- Aby skompilować zestaw SDK w systemie Linux:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Aby utworzyć pliki binarne debugowania, dodaj odpowiednią opcję CMake do polecenia generowania projektu, na przykład:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Istnieje wiele [opcji konfiguracji CMake](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html) dostępnych podczas tworzenia zestawu SDK. Można na przykład wyłączyć jeden z dostępnych stosów protokołu, dodając argument do polecenia generowania projektu CMake:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Można również skompilować i uruchomić test jednostkowy:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Aby skompilować zestaw SDK w systemie Windows, wykonaj następujące kroki w celu wygenerowania plików projektu:
    - Otwórz „Wiersz polecenia dla deweloperów w programie VS2015”
    - Uruchom następujące polecenia CMake w folderze głównym repozytorium:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    To polecenie powoduje kompilowanie bibliotek x86. Aby kompilować dla wersji x64, zmodyfikuj argument generatora cmake: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Jeśli generowanie projektu zostanie ukończone pomyślnie, w folderze `cmake` powinien pojawić się plik rozwiązania programu Visual Studio (sln). Aby skompilować zestaw SDK:
    - Otwórz plik **cmake\azure_iot_sdks.sln** w programie Visual Studio i skompiluj go **LUB**
    - Uruchom następujące polecenie w wierszu polecenia używanym do generowania plików projektu:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Aby utworzyć pliki binarne debugowania, użyj odpowiedniego argumentu platformy MSBuild: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Istnieje wiele opcji konfiguracji CMake dostępnych podczas tworzenia zestawu SDK. Można na przykład wyłączyć jeden z dostępnych stosów protokołu, dodając argument do polecenia generowania projektu CMake:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Można również tworzyć i uruchamiać testy jednostkowe:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Biblioteki do uwzględnienia
- Następujące biblioteki powinny zostać uwzględnione w zestawie SDK:
    - Usługa aprowizowania: dps_http_transport, dps_client, dps_security_client
    - Zabezpieczenia IoTHub: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-dps"></a>Tworzenie wpisu rejestracji urządzenia w usłudze DPS

### <a name="tpm"></a>Moduł TPM
Jeśli korzystasz z modułu TPM, postępuj zgodnie z instrukcjami z sekcji [„Create and provision a simulated device using IoT Hub Device Provisioning Service”](./quick-create-simulated-device.md) (Tworzenie i aprowizowanie symulowanego urządzenia za pomocą usługi IoT Hub Device Provisioning Service), aby utworzyć wpis rejestracji urządzenia w usłudze DPS i symulować pierwsze uruchomienie.

### <a name="x509"></a>X**.**509
1. Aby zarejestrować urządzenie w usłudze aprowizowania, należy zanotować klucz poręczenia i identyfikator rejestracji każdego urządzenia. Te informacje są wyświetlane w narzędziu aprowizowania dostępnym w zestawie SDK klienta. Uruchom następujące polecenie, aby wydrukować certyfikat głównego urzędu certyfikacji (dla grup rejestracji) i certyfikat osoby podpisującej (dla indywidualnej rejestracji):
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Zaloguj się w witrynie Azure Portal, kliknij przycisk **Wszystkie zasoby** w menu po lewej stronie i otwórz swoją usługę DPS.
   - Indywidualna rejestracja w trybie X**.**509: w bloku podsumowania usługi aprowizowania wybierz pozycję **Zarządzaj rejestracjami**. Wybierz kartę **Indywidualne rejestracje** i kliknij u góry przycisk **Dodaj**. Wybierz pozycję **X**.**509** jako *mechanizm* zaświadczania tożsamości i przekaż certyfikat osoby podpisującej zgodnie z wymaganiami bloku. Gdy skończysz, kliknij przycisk **Zapisz**. 
   - Grupowa rejestracja w trybie X**.**509: w bloku podsumowania usługi aprowizowania wybierz pozycję **Zarządzaj rejestracjami**. Wybierz kartę **Grupowe rejestracje** i kliknij u góry przycisk **Dodaj**. Wybierz pozycję **X**.**509** jako *mechanizm* zaświadczania tożsamości, wprowadź nazwę grupy i nazwę certyfikacji, a następnie przekaż certyfikat głównego urzędu certyfikacji zgodnie z wymaganiami bloku. Gdy skończysz, kliknij przycisk **Zapisz**. 

## <a name="connecting-to-iot-hub-after-provisioning"></a>Łączenie z usługą IoT Hub po zakończeniu aprowizacji

Po zakończeniu aprowizacji urządzenia przy użyciu usługi aprowizowania ten interfejs API nawiązuje połączenie z usługą IoT Hub w trybie uwierzytelniania modułu HSM: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```

