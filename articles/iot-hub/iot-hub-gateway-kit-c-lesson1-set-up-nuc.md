---
title: "Urządzeń Sensor tag & Azure IoT bramy - Lekcja 1: Konfigurowanie Intel NUC | Dokumentacja firmy Microsoft"
description: "Skonfigurować Intel NUC do pracy jako bramy IoT między czujników i Centrum IoT Azure do zbierania informacji z czujnika i wysyłania go do Centrum IoT."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: iot bramy intel nuc nuc komputera, DE3815TYKE
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Konfigurowanie Intel NUC jako brama IoT
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Będzie wykonywać

- Ustawienie Intel NUC jako bramę IoT.
- Zainstaluj pakiet Azure IoT Edge na Intel NUC.
- Uruchom przykładową aplikację "hello_world" na NUC firmy Intel, aby sprawdzić funkcjonalność bramy.

  > Jeśli masz problemy, poszukaj rozwiązania [Rozwiązywanie problemów z strony](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co dowiesz się

W tej lekcji dowiesz się:

- Jak nawiązać połączenia z urządzeń peryferyjnych Intel NUC.
- Sposób instalacji i aktualizacji wymaganych pakietów na NUC Intel pomocą inteligentne Menedżera pakietów.
- Jak uruchomić "hello_world" przykładowej aplikacji, aby sprawdzić funkcjonalność bramy.

## <a name="what-you-need"></a>Co jest potrzebne

- Intel NUC Kit DE3815TYKE z pakietem oprogramowania bramy IoT firmy Intel (Linux rzeki knie * 7.0.0.13) preinstalowany. [Kliknij tutaj, aby kupić Grove IoT komercyjnych bramy zestaw](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Kabla Ethernet.
- Klawiatury.
- Kabel HDMI lub VGA.
- Monitor portu HDMI lub VGA.
- Opcjonalnie: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Zestaw bramy](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Połącz z urządzeniami peryferyjnymi Intel NUC

Na poniższym obrazie przedstawiono przykładowy NUC firmy Intel, który jest połączony z różnych urządzeń peryferyjnych:

1. Podłączona do klawiatury.
2. Połączona z monitorem za pomocą kabla VGA lub kabla HDMI.
3. Podłączony do sieci przewodowej za pomocą kabla Ethernet.
4. Podłączone do źródła zasilania, za pomocą kabla zasilania.

![Połączone dla urządzeń peryferyjnych NUC Intel](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Połączyć się z systemem Intel NUC z komputera hosta za pośrednictwem protokołu Secure Shell (SSH)

Konieczne będzie, klawiatury i monitora, aby uzyskać adres IP urządzenia Intel NUC. Jeśli znasz już adres IP, możesz przejść od razu do kroku 3 w tej sekcji.

1. Włącz NUC firmy Intel, naciskając przycisk zasilania, a następnie zaloguj.

   Domyślna nazwa użytkownika i hasło są `root`.

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Uzyskaj adres IP NUC firmy Intel, uruchamiając `ifconfig` polecenia na urządzeniu Intel NUC.

   Oto przykład danych wyjściowych polecenia.

   ![dane wyjściowe ifconfig przedstawiający Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   W tym przykładzie wartość następujący `inet addr:` to adres IP, do którego należy, gdy nawiązać Intel NUC z komputera-hosta.

3. Do nawiązania połączenia NUC firmy Intel, użyj jednej z następujących klientów SSH z komputera hosta.

    - [PuTTY](http://www.putty.org/) dla systemu Windows.
    - Wbudowane SSH klient Ubuntu lub macOS.

   Jest bardziej wydajny i produktywności działanie NUC Intel z komputera-hosta. Będziesz potrzebować Intel NUC adres IP, nazwę użytkownika i hasło, aby połączyć przy użyciu klienta SSH. Oto przykład, które używa klienta SSH w macOS.
   ![Klient SSH systemem macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Zainstaluj pakiet Azure IoT krawędzi

Pakiet Azure IoT krawędzi zawiera wstępnie skompilowanych plików binarnych krawędzi IoT i jego zależności. Te pliki binarne są Azure IoT krawędzi, zestaw SDK usługi Azure IoT i odpowiednie narzędzia. Pakiet zawiera również "hello_world" przykładowej aplikacji jest używany do sprawdzania poprawności funkcje bramy. Krawędź IoT jest główną część bramy. 

Wykonaj następujące kroki, aby zainstalować pakiet.

1. Dodaj repozytorium chmury IoT, uruchamiając następujące polecenia w oknie terminala:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > Wprowadź "y", gdy monit o "Zawierać ten kanał?"
   
   Jeśli zostanie wyświetlony `import read failed(-1)` błędu, aby rozwiązać ten problem należy użyć następujących poleceń:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   `rpm` Polecenie importuje klucz obr. / min. `smart channel` Polecenie dodaje kanału obr. / min na inteligentne Menedżera pakietów. Przed uruchomieniem `smart update` polecenia, zostanie wyświetlony wyjścia, takich jak poniżej.

   ![obr. / min i dane wyjściowe polecenia inteligentne kanału](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Wykonanie polecenia update inteligentnych:

   ```bash
   smart update
   ```

3. Zainstaluj pakiet Azure IoT bramy, uruchamiając następujące polecenie:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`to nazwa pakietu. `smart install` Polecenie służy do zainstalowania pakietu.

    > Jeśli zostanie wyświetlony ten błąd, uruchom następujące polecenie: "klucz publiczny nie jest dostępna"

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Ponowny rozruch NUC firmy Intel, jeśli zostanie wyświetlony ten błąd: "nie zawiera util-linux deweloperów"

   Po zainstalowaniu pakietu Intel NUC jest gotowy do działania jako brama.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>Uruchom przykładową aplikację "hello_world" krawędzi IoT Azure

Następujące przykładowej aplikacji tworzy bramę z `hello_world.json` pliku i używa podstawowych składników architektury Azure IoT Edge, aby wiadomość hello world w pliku dziennika (log.txt) co 5 sekund.

Przykład Witaj świecie można uruchomić, wykonując następujące polecenia:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Zezwalaj aplikacji Hello World Uruchom kilka minut, a następnie naciśnij klawisz Enter, aby ją zatrzymać.
![dane wyjściowe aplikacji](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> Możesz zignorować wszelkie błędy "handle(NULL) nieprawidłowy argument", które są wyświetlane po naciśnięciu przycisku Enter.

Możesz sprawdzić, czy brama zostało wykonane pomyślnie, otwierając plik log.txt, która jest teraz w folderze hello_world ![log.txt widok katalogu](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Otwórz log.txt za pomocą następującego polecenia:

```bash
vim log.txt
```

Zostanie wtedy wyświetlone zawartość log.txt, który zostanie zapisany formatu JSON, wiadomości rejestrowanie, które zostały napisane co 5 sekund, przez moduł Hello World bramy.
![Widok katalogu log.txt](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Jeśli masz problemy, poszukaj rozwiązania [Rozwiązywanie problemów z strony](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Podsumowanie

Gratulacje! Po zakończeniu konfigurowania Intel NUC jako brama. Teraz możesz przejść do następnej lekcji, aby skonfigurować komputer hosta, tworzenia Centrum IoT Azure i zarejestrować urządzenie logiczne Centrum IoT Azure.

## <a name="next-steps"></a>Następne kroki
[Użyj bramy IoT połączenia z urządzeniem z Centrum IoT Azure](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

