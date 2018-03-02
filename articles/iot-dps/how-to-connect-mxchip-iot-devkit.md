---
title: "Jak połączyć się z usługą inicjowania obsługi urządzeń Centrum IoT Azure przy użyciu zestaw deweloperski IoT MXChip | Dokumentacja firmy Microsoft"
description: "Jak używać zestaw deweloperski IoT MXChip do nawiązania połączenia usługi inicjowania obsługi urządzeń Centrum IoT Azure"
services: iot-dps
keywords: 
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 697196f725f0cb8ad3054bd8336b588c17dddc3d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-hub-device-provisioning-service"></a>Zestaw deweloperski IoT MXChip połączyć się z Centrum IoT Azure urządzeniem świadczenie usługi

W tym artykule opisano sposób konfigurowania zestaw deweloperski w celu automatycznego rejestrowania do Centrum IoT przy użyciu usługi inicjowania obsługi urządzeń. Niniejszy samouczek zawiera informacje na temat wykonywania następujących czynności:

* Skonfigurować globalne punktu końcowego o właściwości punktu dystrybucji na urządzeniu
* Unikatowy klucz tajny urządzenia (UDS) umożliwia wygenerowanie certyfikatu X.509
* Rejestrowanie urządzenia
* Sprawdź, czy urządzenie jest zarejestrowane

## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby wykonać kroki opisane w tym samouczku, są potrzebne następujące elementy:

* Przygotowanie Twojego zestaw deweloperski z [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Uaktualnij do najnowszego oprogramowania układowego (> = 1.3.0) z [uaktualnienie oprogramowania układowego](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) samouczka.
* Utwórz i Połącz z wystąpieniem usługi inicjowania obsługi urządzeń z Centrum IoT [Konfigurowanie automatycznego inicjowania obsługi administracyjnej](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Ustawianie konfiguracji usługi udostępniania urządzenia na urządzeniu

Aby włączyć zestaw deweloperski połączyć się z wystąpieniem usługi inicjowania obsługi urządzeń, utworzony:

1. W portalu Azure wybierz **omówienie** usługi inicjowania obsługi urządzeń i zanotuj **punktu końcowego globalnej urządzenia** i **identyfikator zakresu** wartość.
  ![Punktu dystrybucji globalne punktu końcowego i zakres Identyfikatora](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Upewnij się, że na swojej maszynie masz zainstalowane oprogramowanie `git` i że jest ono dodane do zmiennych środowiskowych dostępnych z okna poleceń. Zobacz [narzędzia klienta Git oprogramowania swobodę zakresie ochrony jego](https://git-scm.com/download/) mieć zainstalowaną najnowszą wersję.

3. Otwórz wiersz polecenia. Klonowanie repozytorium GitHub dla punktu dystrybucji przykładowy kod:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Uruchamianie kodzie VS i zestaw deweloperski połączyć się z komputerem, otwórz folder, który zawiera kod, który można sklonować.

5. Otwórz **DevKitDPS.ino**, Znajdź i Zamień `[Global Device Endpoint]` i `[ID Scope]` przy użyciu wartości podobnie należy zanotować.
  ![Punkt końcowy punktu dystrybucji](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) można pozostawić **registrationId** jako puste, aplikacja generuje go na podstawie wersji MAC adres i oprogramowania układowego. Jeśli chcesz dostosować go identyfikator rejestracji ma alfanumeryczne, małe litery, a czas łącznika kombinacji tylko z maksymalnie 128 znaków. Aby uzyskać więcej informacji, zobacz [Zarządzanie rejestracji urządzenia z portalu Azure](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Użyj **szybkie Otwórz** w kodzie VS (Windows: `Ctrl+P`, macOS: `Cmd+P`) i typ **zadań przekazywania urządzenia** kompilacji i przekazać kod do zestaw deweloperski.

7. Sprawdź Powodzenie zadań w oknie danych wyjściowych.

## <a name="save-unique-device-secret-on-stsafe-security-chip"></a>Zapisz Unikatowy klucz tajny urządzenia w układzie zabezpieczeń STSAFE

Można skonfigurować usługi udostępniania urządzenia na urządzeniu, na podstawie jego [sprzętowego modułu zabezpieczeń (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/). Zestaw deweloperski używa [urządzenia tożsamości kompozycji aparat (GRUPOWANE)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) z [Trusted Computing Group (TCG)](https://trustedcomputinggroup.org). A **Unikatowy klucz tajny urządzenia (UDS)** zapisane w zabezpieczeń STSAFE mikroukładu na zestaw deweloperski służy do generowania unikatowych urządzeń [X.509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module) certyfikatu. Certyfikat może później użyć procesu rejestracji w usłudze udostępniania urządzeń.

Typowe **Unikatowy klucz tajny urządzenia (UDS)** to ciąg 64 znaków. Przykładowe UDS jest poniżej:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Każdego z dwóch znaków jest używany jako wartość szesnastkowa w obliczeniach zabezpieczeń. Dlatego powyższym przykładzie UDS jest rozwiązywana: "`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`".

Aby zapisać Unikatowy klucz tajny urządzenia na zestaw deweloperski:

1. Otwórz monitor szeregowe przy użyciu narzędzia, takiego jak Putty, zobacz [Użyj trybu konfiguracji](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) szczegółowe informacje.

2. Z zestaw deweloperski podłączane do komputera przytrzymaj naciśnięty przycisk A, a następnie wypychania, a następnie zwolnij przycisk resetowania do trybu konfiguracji. Ekran powinien być wyświetlony identyfikator zestaw deweloperski i **"Konfiguracja"**.

3. Pobrać ciąg UDS długi próbki powyżej i zmienić jednego lub wielu znaków innych wartości między `0` i `f`. To jest używana jako własnego UDS.

4. W oknie Monitora szeregowych, wpisz **set_dps_uds [your_own_uds_value]** i naciśnij klawisz Enter, aby zapisać go.
  > [!NOTE]
  > Na przykład jeśli ustawisz własne UDS zmieniając ostatnie dwa znaki `f`, należy wprowadzić polecenie, takie jak **set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff**.

5. Bez zamykania okna monitora szeregowych, naciśnij przycisk resetowania przycisk na zestaw deweloperski.

6. Należy zanotować **adres MAC zestaw deweloperski** i **wersja oprogramowania układowego zestaw deweloperski** wartość.
  ![Wersja oprogramowania układowego](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-x509-certificate"></a>Generowanie certyfikatu X.509

### <a name="windows"></a>Windows

1. Otwórz Eksploratora i przejdź do folderu zawierają DSP przykładowy kod sklonowany, jest **.kompilacja** folderu, Znajdź, a następnie skopiuj **DPS.ino.bin** i **DPS.ino.map** w nim.
  ![Wygenerowane pliki](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Jeśli zmieniono `built.path` konfiguracji Arduino do innego folderu. Należy znaleźć tych plików w folderze, który został skonfigurowany.

2. Wklej tych plików do **narzędzia** folderu na tym samym poziomie z **.kompilacja** folderu.

3. Uruchom **dps_cert_gen.exe**, postępuj zgodnie z monitami o wprowadzenie Twojej **UDS**, **adres MAC** dla zestaw deweloperski i **wersja oprogramowania układowego** do wygenerowania Certyfikat X.509.
  ![Run dps-cert-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Sprawdź Powodzenie generowania **PEM** certyfikatu jest zapisywany w tym samym folderze.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Tworzenie wpisu rejestracji urządzenia w usłudze Device Provisioning Service

1. W portalu Azure przejdź do inicjowania obsługi usługi. Kliknij pozycję **Zarządzanie rejestracjami** i wybierz kartę **Rejestracje indywidualne**. ![Poszczególne rejestracji](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Kliknij pozycję **Add** (Dodaj).

3. W **mechanizmu**, wybierz **X.509**.
  ![Przekazywanie certyfikatu](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. W **plik PEM lub .cer certyfikatu**, Przekaż **PEM** certyfikatu należy.

5. Pozostaw rest jako domyślny i kliknij przycisk **zapisać**.

## <a name="start-the-devkit"></a>Uruchom zestaw deweloperski

1. Uruchom kodzie VS i otwórz monitor szeregowego.

2. Naciśnij klawisz **zresetować** przycisk na Twoje zestaw deweloperski.

Powinny pojawić się zestaw deweloperski, Rozpocznij rejestrację przy użyciu usługi inicjowania obsługi urządzeń.

![Dane wyjściowe programu VS kodu](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-the-devkit-is-registered-on-iot-hub"></a>Sprawdź, czy zestaw deweloperski jest zarejestrowany w Centrum IoT

Po rozruchu z urządzenia, należy zostaną wykonane następujące czynności:

1. Urządzenie wysyła żądanie rejestracji z usługą udostępniania urządzeń.
2. Usługa inicjowania obsługi urządzeń przesyła żądanie rejestracji, na które odpowiada urządzenia.
3. Na pomyślną rejestrację usługi inicjowania obsługi urządzeń wysyła Centrum IoT identyfikatora URI, identyfikator urządzenia i zaszyfrowanego klucza z powrotem do urządzenia.
4. Centrum IoT aplikacja klienta na urządzeniu łączy do Centrum.
5. Powinna zostać wyświetlona urządzenia są wyświetlane w Eksploratorze urządzenia Centrum IoT na udane połączenie z koncentratorem.
  ![Zarejestrowane urządzenia](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-device-id"></a>Zmień identyfikator urządzenia

Domyślny identyfikator urządzenia zarejestrowane w usłudze Azure IoT Hub jest **AZ3166**. Jeśli chcesz zmodyfikować go, wykonaj [instrukcje w tym miejscu](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemy i opinie

Jeśli wystąpią problemy, zapoznaj się [— często zadawane pytania](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) lub dotrzeć do nas z następujących kanałów:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Witryna Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Kolejne kroki

Teraz, gdy wiesz już przygotować zestaw deweloperski, aby zarejestrować urządzenie do punktu dystrybucji przy użyciu GRUPOWANE, tak, aby automatycznie można zarejestrować do Centrum IoT z zero touch. W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skonfigurować globalne punktu końcowego o właściwości punktu dystrybucji na urządzeniu
> * Unikatowy klucz tajny urządzenia (UDS) umożliwia wygenerowanie certyfikatu X.509
> * Rejestrowanie urządzenia
> * Sprawdź, czy urządzenie jest zarejestrowane

Przejdź do innych samouczków, aby dowiedzieć się więcej:

> [!div class="nextstepaction"]
> [Tworzenie i przydzielanie symulowane urządzenie](./quick-create-simulated-device.md)

