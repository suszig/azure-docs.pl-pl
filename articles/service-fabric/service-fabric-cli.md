---
title: "Wprowadzenie do interfejsu wiersza polecenia usługi Azure Service Fabric (sfctl)"
description: "Dowiedz się, jak korzystać z interfejsu wiersza polecenia usługi Azure Service Fabric. Dowiedz się, jak nawiązać połączenie z klastrem i zarządzać aplikacjami."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 2faca2887f25b45d833dea7b2259277466290670
ms.contentlocale: pl-pl
ms.lasthandoff: 08/29/2017

---
# <a name="azure-service-fabric-command-line"></a>Wiersz polecenia usługi Azure Service Fabric

Interfejs wiersza polecenia usługi Azure Service Fabric (sfctl) jest narzędziem wiersza polecenia do interakcji z jednostkami usługi Azure Service Fabric i zarządzania nimi. Interfejs sfctl może być używany z klastrami systemu Windows lub Linux. Interfejs sfctl działa na dowolnej platformie, która obsługuje język Python.

## <a name="prerequisites"></a>Wymagania wstępne

Przed instalacją upewnij się, że w środowisku zainstalowano zarówno język Python, jak i narzędzie pip. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją szybkiego startu narzędzia pip](https://pip.pypa.io/en/latest/quickstart/) i oficjalną [dokumentacją dotyczącą instalowania języka Python](https://wiki.python.org/moin/BeginnersGuide/Download).

Choć obsługiwana jest zarówno wersja 2.7, jak i 3.6 języka Python, zaleca się korzystanie z wersji 3.6. W poniższej sekcji pokazano, jak zainstalować wszystkie wstępnie wymagane składniki i interfejs wiersza polecenia.

## <a name="install-pip-python-and-sfctl"></a>Instalowanie narzędzia pip, środowiska python i interfejsu sfctl

Środowisko python i narzędzie pip można zainstalować na platformie na wiele sposobów. Poniżej pokazano, jak szybko skonfigurować środowisko python 3.6 i narzędzie pip dla najczęściej używanych systemów operacyjnych:

### <a name="windows"></a>Windows

W systemach Windows 10, Windows Server 2016 i Windows Server 2012 R2 można skorzystać ze standardowych oficjalnych instrukcji dotyczących instalacji. Instalator środowiska python domyślnie instaluje też narzędzie pip.

- Przejdź do oficjalnej [strony pobierania środowiska python](https://www.python.org/downloads/) i pobierz najnowszą wersję środowiska python 3.6
- Uruchom instalatora
- Wybierz opcję w dolnej części monitu (`Add Python 3.6 to PATH`)
- Wybierz pozycję `Install Now`
- Ukończ instalację

Teraz powinno być możliwe otwarcie nowego okna polecenia i pobranie odpowiedniej wersji środowiska python i narzędzia pip:

```bat
python --version
pip --version
```

Następnie uruchom następujący element, aby zainstalować interfejs wiersza polecenia usługi Service Fabric

```
pip install sfctl
sfctl -h
```

### <a name="ubuntu"></a>Ubuntu

W przypadku systemu Ubuntu 16.04 Desktop można zainstalować środowisko python 3.6 przy użyciu osobistych archiwów pakietów innych firm:

W terminalu uruchom następujące polecenia:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Aby zainstalować interfejs sfctl tylko dla Twojej instalacji środowiska python 3.6, uruchom następujące polecenie:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Te kroki nie mają wpływu na środowisko python 3.5 i 2.7 zainstalowane w systemie. Nie modyfikuj tych instalacji, jeśli nie znasz dobrze systemu Ubuntu.

### <a name="macos"></a>MacOS

W przypadku systemu MacOS zaleca się użycie [menedżera pakietów HomeBrew](https://brew.sh). Zainstaluj program HomeBrew, jeśli jeszcze go nie zainstalowano. W tym celu uruchom następujące polecenie:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Następnie z poziomu terminala zainstaluj środowisko python 3.6, narzędzie pip i interfejs sfctl

```bash
brew install python3
pip3 install sfctl
sfctl -h
```

Te czynności nie powodują zmodyfikowania instalacji środowiska python 2.7 w systemie.

## <a name="cli-syntax"></a>Składnia interfejsu wiersza polecenia

Polecenia mają zawsze prefiks `sfctl`. Aby uzyskać ogólne informacje na temat wszystkich poleceń, z których możesz korzystać, użyj polecenia `sfctl -h`. Aby uzyskać pomoc dotyczącą pojedynczego polecenia, użyj polecenia `sfctl <command> -h`.

Polecenia mają powtarzalną strukturę, w której element docelowy poprzedza zlecenie (akcję):

```azurecli
sfctl <object> <action>
```

W tym przykładzie `<object>` to element docelowy akcji `<action>`.

## <a name="select-a-cluster"></a>Wybieranie klastra

Przed wykonaniem jakiejkolwiek operacji musisz wybrać klaster, z którym chcesz nawiązać połączenie. Na przykład uruchom następujące polecenie, aby wybrać klaster o nazwie `testcluster.com` i nawiązać z nim połączenie.

> [!WARNING]
> Nie używaj niezabezpieczonych klastrów usługi Service Fabric w środowisku produkcyjnym.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Punkt końcowy klastra musi mieć prefiks `http` lub `https`. Musi on zawierać port bramy HTTP. Port i adres są takie same jak adres URL programu Service Fabric Explorer.

W przypadku klastrów zabezpieczonych certyfikatem możesz określić certyfikat zakodowany w formacie PEM. Certyfikat może być określony jako pojedynczy plik lub certyfikat i para kluczy.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Aby uzyskać więcej informacji, zobacz temat [Nawiązywanie połączenia z zabezpieczonym klastrem usługi Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Operacje podstawowe

Informacje o połączeniu z klastrem są utrwalane między sesjami interfejsu sfctl. Po wybraniu klastra usługi Service Fabric można uruchomić dowolne polecenie usługi Service Fabric w klastrze.

Aby na przykład uzyskać informacje o kondycji klastra usługi Service Fabric, uruchom następujące polecenie:

```azurecli
sfctl cluster health
```

Polecenie powoduje zwrócenie następujących danych wyjściowych:

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a>Porady i rozwiązywanie problemów

Kilka sugestii i porad dotyczących rozwiązywania typowych problemów.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Konwertowanie certyfikatu z formatu PFX na PEM

Interfejs wiersza polecenia usługi Service Fabric obsługuje certyfikaty po stronie klienta w postaci plików PEM (rozszerzenie pem). Jeśli używasz plików PFX z systemu Windows, musisz konwertować te certyfikaty na format PEM. Aby konwertować plik PFX na plik PEM, użyj następującego polecenia:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją dotyczącą protokołu OpenSSL](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Problemy z połączeniem

Niektóre operacje mogą generować następujący komunikat:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Sprawdź, czy punkt końcowy określonego klastra jest dostępny i przeprowadza nasłuchiwanie. Sprawdź również, czy interfejs użytkownika programu Service Fabric Explorer jest dostępny na tym hoście i porcie. Aby zaktualizować punkt końcowy, użyj polecenia `sfctl cluster select`.

### <a name="detailed-logs"></a>Szczegółowe dzienniki

Szczegółowe dzienniki często bywają przydatne w przypadku debugowania lub zgłaszania problemu. Dostępna jest globalna flaga `--debug`, która zwiększa szczegółowość plików dzienników.

### <a name="command-help-and-syntax"></a>Polecenia — pomoc i składnia

Aby uzyskać pomoc dotyczącą określonego polecenia lub grupy poleceń, użyj flagi `-h`:

```azurecli
sfctl application -h
```

Inny przykład:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Następne kroki

* [Deploy an application with the Azure Service Fabric CLI (Wdrażanie aplikacji przy użyciu interfejsu wiersza polecenia usługi Azure Service Fabric)](service-fabric-application-lifecycle-sfctl.md)
* [Wprowadzenie do usługi Service Fabric w systemie Linux](service-fabric-get-started-linux.md)

