---
title: "Zapewnij symulowane urządzenie X.509 z Centrum IoT Azure przy użyciu języka Java i rejestracji grup | Dokumentacja firmy Microsoft"
description: "Samouczek platformy Azure — tworzenie i przydzielanie symulowane urządzenie X.509 przy użyciu języka Java urządzenia i usługi SDK i rejestracji grup inicjowania obsługi usługi IoT Hub urządzeń"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 01/04/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 14e5e7613fd5df650625cf8997d569b754ceb689
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/20/2018
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-and-service-sdk-and-group-enrollments-for-iot-hub-device-provisioning-service"></a>Tworzenie i przydzielanie symulowane urządzenie X.509 przy użyciu urządzenia Java i zestawu SDK usług i rejestracji grupy dla usługi udostępniania urządzenia IoT Hub

Te kroki pokazują, jak symulować X.509 urządzenia na komputerze deweloperskim uruchamiania systemu operacyjnego Windows i użyj przykładowy kod, aby połączyć to symulowane urządzenie z usługi udostępniania urządzenia i Centrum IoT przy użyciu grup rejestracji. 

Pamiętaj, aby wcześniej wykonać kroki przedstawione w części [Konfigurowanie usługi IoT Hub Device Provisioning za pomocą witryny Azure Portal](./quick-setup-auto-provision.md).


## <a name="prepare-the-environment"></a>Przygotowywanie środowiska 

1. Upewnij się, że na maszynie zainstalowano środowisko [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

1. Pobierz i zainstaluj pakiet [Maven](https://maven.apache.org/install.html).

1. Upewnij się, że na swojej maszynie masz zainstalowane oprogramowanie `git` i że jest ono dodane do zmiennych środowiskowych dostępnych z okna poleceń. Zobacz stronę z [narzędziami klienckimi Git organizacji Software Freedom Conservancy](https://git-scm.com/download/), aby uzyskać najnowszą wersję narzędzi `git` do zainstalowania, które obejmują powłokę **Git Bash**, czyli aplikację wiersza polecenia, która może służyć do interakcji z lokalnym repozytorium Git. 

1. Należy użyć następującego [omówienie przez siebie](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) do tworzenia certyfikatów testu. Aby uzyskać więcej informacji na temat przyjrzeć się tworzenie certyfikatów, zobacz [skryptów programu PowerShell do zarządzania certyfikatami X.509 podpisany przez urząd certyfikacji](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-x509-create-certificates).

    > [!NOTE]
    > Ten krok wymaga [OpenSSL](https://www.openssl.org/), które można albo zbudowany i zainstalowany ze źródła lub pobrania i zainstalowania z [3 strona](https://wiki.openssl.org/index.php/Binaries) takich jak [to](https://sourceforge.net/projects/openssl/). Jeśli utworzono już Twojej _głównego_, _pośredniego_ i _urządzenia_ certyfikatów, możesz pominąć ten krok.
    >

1. Utwórz informacje o grupie rejestracji:

    1. Uruchom za pomocą **krok 1** i **krok 2** do utworzenia sieci _głównego_ i _pośredniego_ certyfikatów.

    1. Zaloguj się do witryny Azure Portal, kliknij przycisk **Wszystkie zasoby** w menu po lewej stronie, a następnie otwórz używaną usługę aprowizacji.

        1. W bloku podsumowania usługi Device Provisioning wybierz pozycję **Certyfikaty** i kliknij przycisk **Dodaj** na górze.

        1. W obszarze **Dodawanie certyfikatu** wprowadź następujące informacje:
            - Wprowadź unikatową nazwę certyfikatu.
            - Wybierz  **_RootCA.pem_**  nowo utworzonego pliku.
            - Gdy skończysz, kliknij przycisk **Zapisz**.

        ![Dodawanie certyfikatu](./media/tutorial-group-enrollments/add-certificate.png)

        1. Wybierz nowo utworzony certyfikat:
            - Kliknij pozycję **Generuj kod weryfikacyjny**. Skopiuj wygenerowany kod.
            - Uruchom za pomocą **krok 3**. Wprowadź _kod weryfikacyjny_ lub kliknij prawym przyciskiem myszy, aby wkleić w oknie programu PowerShell uruchomione.  Naciśnij klawisz **Enter**.
            - Wybierz nowo utworzoną  **_verifyCert4.pem_**  pliku w portalu Azure. Kliknij przycisk **Sprawdź**.

            ![Sprawdzanie poprawności certyfikatu](./media/tutorial-group-enrollments/validate-certificate.png)

1. Zakończ uruchamiając **krok 4** i **krok 5** utworzyć certyfikaty urządzeń i zasobów czyszczenia.

> [!NOTE]
> Podczas tworzenia certyfikatów urządzeń należy używać tylko małych alfanumeryczne i łączniki w nazwie urządzenia.
>


## <a name="create-a-device-enrollment-entry"></a>Tworzenie wpisu rejestracji urządzenia

1. Otwórz wiersz polecenia. Klonowanie repozytorium GitHub dla przykładów kodu zestawu Java SDK:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. W pobranym kodzie źródłowym przejdź do folderu przykładu **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**. Otwórz plik **_/src/main/java/samples/com/microsoft/azure/sdk/iot/ServiceEnrollmentGroupSample.java_** w dowolnym edytorze i dodaj następujące szczegółowe informacje:

    1. Dodaj ciąg `[Provisioning Connection String]` dla usługi aprowizacji z portalu, jako pokazano poniżej:

        1. Przejdź do usługi aprowizacji w witrynie [Azure Portal](https://portal.azure.com). 

        1. Otwórz **Zasady dostępu współużytkowanego** i wybierz zasadę, która ma uprawnienie *EnrollmentWrite*.
    
        1. Skopiuj **Parametry połączenia klucza podstawowego**. 

            ![Pobieranie parametrów połączenia aprowizacji z portalu](./media/tutorial-group-enrollments/provisioning-string.png)  

        1. W przykładowym pliku kodu **_ServiceEnrollmentGroupSample.java_** zastąp ciąg `[Provisioning Connection String]` **parametrami połączenia klucza podstawowego**.

            ```java
            private static final String PROVISIONING_CONNECTION_STRING = "[Provisioning Connection String]";
            ```

    1. Otwórz  **_RootCA.pem_**  plik w edytorze tekstów. Przypisz wartość **certyfikatu głównego** do parametru **PUBLIC_KEY_CERTIFICATE_STRING** w sposób przedstawiony poniżej:

        ```java
        private static final String PUBLIC_KEY_CERTIFICATE_STRING =
                "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
        ```
 
    1. Przejdź do centrum IoT połączonego z usługą aprowizacji w witrynie [Azure Portal](https://portal.azure.com). Otwórz kartę **Przegląd** centrum i skopiuj wartość pola **Nazwa hosta**. Przypisz tę **nazwę hosta** do parametru *IOTHUB_HOST_NAME*.

        ```java
        private static final String IOTHUB_HOST_NAME = "[Host name].azure-devices.net";
        ```

    1. Przejrzyj przykładowy kod. Tworzy on, aktualizuje i usuwa rejestrację grupową urządzeń X.509 oraz tworzy do niej zapytanie. Aby sprawdzić poprawność rejestracji w portalu, tymczasowo oznacz jako komentarz następujące wiersze kodu na końcu pliku _ServiceEnrollmentGroupSample.java_:

        ```java
        // ************************************** Delete info of enrollmentGroup ***************************************
        System.out.println("\nDelete the enrollmentGroup...");
        provisioningServiceClient.deleteEnrollmentGroup(enrollmentGroupId);
        ```

    1. Zapisz plik _ServiceEnrollmentGroupSample.java_. 

1. Otwórz okno polecenia i przejdź do folderu **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**.

1. Skompiluj przykładowy kod za pomocą tego polecenia:

    ```cmd\sh
    mvn install -DskipTests
    ```

1. Uruchom przykładowy kod przy użyciu tych poleceń w oknie polecenia:

    ```cmd\sh
    cd target
    java -jar ./service-enrollment-group-sample-{version}-with-deps.jar
    ```

1. Sprawdź poprawność rejestracji w oknie danych wyjściowych.

    ![pomyślnej rejestracji](./media/tutorial-group-enrollments/enrollment.png) 

1. Przejdź do usługi aprowizacji w witrynie Azure Portal. Kliknij pozycję **Zarządzanie rejestracjami**. Zauważ, że grupa urządzeń X.509 jest wyświetlana na karcie **Grupy rejestracji** z automatycznie wygenerowaną *NAZWĄ GRUPY*. 


## <a name="simulate-the-device"></a>Symulowanie urządzenia

1. W bloku podsumowania usługi Device Provisioning wybierz pozycję **Przegląd** i zanotuj _Identyfikator zakresu_ oraz _Globalny punkt końcowy usługi aprowizacji_.

    ![Informacje o usłudze](./media/tutorial-group-enrollments/extract-dps-endpoints.png)

1. Otwórz wiersz polecenia. Przejdź do folderu przykładowego projektu.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Wprowadź informacje o grupie rejestracji w następujący sposób:

    - Edytuj plik `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java`, aby uwzględnić w nim wspomniane wcześniej wartości _Identyfikatora zakresu_ i _Globalnego punktu końcowego usługi aprowizacji_. Otwórz z  **_{deviceName}-public.pem_**  plików i obejmują tej wartości jako sieci _certyfikatu klienta_. Otwórz z  **_{deviceName}-all.pem_**  plików i skopiować tekst z _---klucza prywatnego BEGIN---_ do _---koniec klucza prywatnego---_.  Użyj jako sieci _klucza prywatnego certyfikatu klienta_.

        ```java
        private static final String idScope = "[Your ID scope here]";
        private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
        private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
        private static final String leafPublicPem = "<Your Public PEM Certificate here>";
        private static final String leafPrivateKey = "<Your Private PEM Key here>";
        ```

        - Aby uwzględnić wartości certyfikatu i klucza, użyj następującego formatu:
            
            ```java
            private static final String leafPublicPem = "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
            private static final String leafPrivateKey = "-----BEGIN PRIVATE KEY-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXX\n" +
                "-----END PRIVATE KEY-----\n";
            ```

1. Skompiluj przykład. Przejdź do folderu docelowego i wykonaj utworzony plik jar.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

    ![Rejestracja zakończona powodzeniem](./media/tutorial-group-enrollments/registration.png)

1. W portalu przejdź do centrum IoT Hub połączonego z usługą aprowizacji, a następnie otwórz blok **Device Explorer**. Po pomyślnej aprowizacji symulowanego urządzenia X.509 w centrum identyfikator urządzenia jest wyświetlany w bloku **Device Explorer** z pozycją *STATUS* (stan) ustawioną na wartość **enabled** (włączone). Należy pamiętać, że konieczne może być kliknięcie przycisku **Odśwież** znajdującego się u góry, jeśli blok został otwarty przed uruchomieniem przykładowej aplikacji urządzenia. 

    ![Urządzenie jest rejestrowane w centrum IoT](./media/tutorial-group-enrollments/hub-registration.png) 


## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli planujesz dalszą pracę z przykładem klienta urządzenia i eksplorowanie go, nie czyść zasobów utworzonych w ramach tego przewodnika Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj poniższe kroki, aby usunąć wszystkie zasoby utworzone w ramach tego przewodnika Szybki start.

1. Zamknij okno danych wyjściowych przykładu klienta urządzenia na swojej maszynie.
1. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoją usługę Device Provisioning Service. Otwórz blok **Zarządzanie rejestracjami** dla usługi, a następnie kliknij kartę **Rejestracje indywidualne**. Wybierz *IDENTYFIKATOR REJESTRACJI* urządzenia zarejestrowanego w ramach tego przewodnika Szybki start i kliknij przycisk **Usuń** u góry. 
1. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoje centrum IoT. Otwórz blok **Urządzenia IoT** Twojego centrum, wybierz *IDENTYFIKATOR URZĄDZENIA* zarejestrowanego w ramach tego przewodnika Szybki start, a następnie kliknij przycisk **Usuń** u góry.


## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzeniu symulowane urządzenie X.509 na komputerze z systemem Windows i zainicjowano obsługę administracyjną do Centrum IoT przy użyciu grup rejestracji i inicjowania obsługi usługi Azure IoT Hub urządzenia. Aby dowiedzieć się więcej o urządzeniu X.509, nadal pojęcia urządzenia. 

> [!div class="nextstepaction"]
> [Pojęcia dotyczące urządzenia inicjowania obsługi usługi IoT Hub urządzeń](concepts-device.md)
