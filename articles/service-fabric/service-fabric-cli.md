---
title: "Wprowadzenie do interfejsu wiersza polecenia usługi Azure Service Fabric"
description: "Dowiedz się, jak korzystać z interfejsu wiersza polecenia usługi Azure Service Fabric. Dowiedz się, jak nawiązać połączenie z klastrem i zarządzać aplikacjami."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 4f77c7a615aaf5f87c0b260321f45a4e7129f339
ms.openlocfilehash: f246ee8aaecf3a398182debdea07832c75c1bd9c
ms.contentlocale: pl-pl
ms.lasthandoff: 09/22/2017

---
# <a name="azure-service-fabric-cli"></a>Interfejs wiersza polecenia usługi Azure Service Fabric

Interfejs wiersza polecenia usługi Azure Service Fabric jest narzędziem wiersza polecenia służącym do interakcji z jednostkami usługi Service Fabric i zarządzania nimi. Interfejs wiersza polecenia usługi Service Fabric może być używany z klastrami systemu Windows lub Linux. Interfejs wiersza polecenia usługi Service Fabric działa na dowolnej platformie, która obsługuje język Python.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

## <a name="prerequisites"></a>Wymagania wstępne

Przed instalacją upewnij się, że w środowisku zainstalowano zarówno język Python, jak i narzędzie pip. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją Szybki start dotyczącą narzędzia pip](https://pip.pypa.io/en/latest/quickstart/) i oficjalną [dokumentacją dotyczącą instalowania języka Python](https://wiki.python.org/moin/BeginnersGuide/Download).

Choć obsługiwana jest zarówno wersja 2.7, jak i 3.6 języka Python, zaleca się korzystanie z wersji 3.6. W poniższej sekcji pokazano, jak zainstalować wszystkie wstępnie wymagane składniki i interfejs wiersza polecenia.

## <a name="install-pip-python-and-the-service-fabric-cli"></a>Instalowanie narzędzia pip, środowiska Python i interfejsu wiersza polecenia usługi Service Fabric

 Dostępnych jest szereg sposobów instalowania narzędzia pip i środowiska Python na używanej platformie. Poniżej przedstawiono kroki konfiguracji narzędzia pip i środowiska Python 3.6 w najważniejszych systemach operacyjnych.

### <a name="windows"></a>Windows

W przypadku systemów Windows 10, Windows Server 2016 i Windows Server 2012 R2 należy postępować zgodnie z oficjalnymi instrukcjami standardowej instalacji. Instalator środowiska Python domyślnie instaluje też narzędzie pip.

1. Przejdź do oficjalnej [strony pobierania środowiska Python](https://www.python.org/downloads/) i pobierz najnowszą wersję środowiska Python 3.6.

2. Uruchom instalatora.

3. W dolnej części monitu wybierz pozycję **Add Python 3.6 to PATH (Dodaj środowisko Python 3.6 do ścieżki)**.

4. Wybierz pozycję **Install Now (Zainstaluj teraz)** i dokończ instalację.

Możesz teraz otworzyć nowe okno polecenia i pobrać odpowiednią wersję środowiska Python i narzędzia pip.

```bat
python --version
pip --version
```

Następnie uruchom następujące polecenie, aby zainstalować interfejs wiersza polecenia usługi Service Fabric:

```
pip install sfctl
sfctl -h
```

Jeśli pojawi się komunikat o błędzie z informacją o nieznalezieniu interfejsu `sfctl`, uruchom następujące polecenia:

```bash
export PATH=$PATH:~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> .bashrc
```

### <a name="ubuntu"></a>Ubuntu

W przypadku systemu Ubuntu 16.04 Desktop można zainstalować środowisko Python 3.6 przy użyciu osobistych archiwów pakietów innych firm.

W terminalu uruchom następujące polecenia:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Aby zainstalować interfejs wiersza polecenia usługi Service Fabric tylko dla Twojej instalacji środowiska Python 3.6, uruchom następujące polecenie:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Jeśli pojawi się komunikat o błędzie z informacją o nieznalezieniu interfejsu `sfctl`, uruchom następujące polecenia:

```bash
export PATH=$PATH:~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> .bashrc
```

Te kroki nie mają wpływu na środowiska Python 3.5 i 2.7 zainstalowane w systemie. Nie modyfikuj tych instalacji, jeśli nie znasz dobrze systemu Ubuntu.

### <a name="macos"></a>MacOS

W przypadku systemu MacOS zaleca się użycie [menedżera pakietów HomeBrew](https://brew.sh). Zainstaluj program HomeBrew, jeśli jeszcze go nie zainstalowano. W tym celu uruchom następujące polecenie:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Następnie z poziomu terminala zainstaluj środowisko Python 3.6, narzędzie pip i interfejs wiersza polecenia usługi Service Fabric za pomocą poniższych poleceń:

```bash
brew install python3
pip3 install sfctl
sfctl -h
```


Jeśli pojawi się komunikat o błędzie z informacją o nieznalezieniu interfejsu `sfctl`, uruchom następujące polecenia:

```bash
export PATH=$PATH:~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> .bashrc
```


Te kroki nie powodują zmodyfikowania instalacji środowiska Python 2.7 w systemie.

## <a name="cli-syntax"></a>Składnia interfejsu wiersza polecenia

Polecenia mają zawsze prefiks `sfctl`. Aby uzyskać ogólne informacje na temat wszystkich dostępnych poleceń, użyj polecenia `sfctl -h`. Aby uzyskać pomoc dotyczącą pojedynczego polecenia, użyj polecenia `sfctl <command> -h`.

Polecenia mają powtarzalną strukturę, w której element docelowy poprzedza zlecenie (akcję).

```azurecli
sfctl <object> <action>
```

W tym przykładzie `<object>` to element docelowy akcji `<action>`.

## <a name="select-a-cluster"></a>Wybieranie klastra

Przed wykonaniem jakiejkolwiek operacji musisz wybrać klaster, z którym chcesz nawiązać połączenie. Aby na przykład wybrać klaster o nazwie `testcluster.com` i nawiązać z nim połączenie, uruchom następujące polecenie:

> [!WARNING]
> Nie używaj niezabezpieczonych klastrów usługi Service Fabric w środowisku produkcyjnym.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Punkt końcowy klastra musi mieć prefiks `http` lub `https`. Musi on zawierać port bramy HTTP. Port i adres są takie same jak adres URL programu Service Fabric Explorer.

W przypadku klastrów zabezpieczonych certyfikatem możesz określić certyfikat zakodowany w formacie PEM. Certyfikat można określić jako pojedynczy plik lub jako parę obejmującą certyfikat i klucz. W przypadku certyfikatu z podpisem własnym, który nie jest podpisany przez urząd certyfikacji, można użyć opcji `--no-verify`, aby pominąć weryfikację urzędu certyfikacji.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Aby uzyskać więcej informacji, zobacz temat [Nawiązywanie połączenia z zabezpieczonym klastrem usługi Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Operacje podstawowe

Informacje o połączeniu klastra są utrwalane w wielu sesjach interfejsu wiersza polecenia usługi Service Fabric. Po wybraniu klastra usługi Service Fabric można uruchomić dowolne polecenie usługi Service Fabric w klastrze.

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

Podobnie aby przekonwertować plik PEM do pliku PFX, można użyć następującego polecenia (nie wymaga ono podawania hasła):

```bash
openssl  pkcs12 -export -out Certificates.pfx -inkey Certificates.pem -in Certificates.pem -passout pass:'' 
```

Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją dotyczącą protokołu OpenSSL](https://www.openssl.org/docs/).

### <a name="connection-problems"></a>Problemy z połączeniem

Niektóre operacje mogą generować następujący komunikat:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Sprawdź, czy punkt końcowy określonego klastra jest dostępny i przeprowadza nasłuchiwanie. Sprawdź również, czy interfejs użytkownika programu Service Fabric Explorer jest dostępny na tym hoście i porcie. Aby zaktualizować punkt końcowy, użyj polecenia `sfctl cluster select`.

### <a name="detailed-logs"></a>Szczegółowe dzienniki

Szczegółowe dzienniki często bywają przydatne w przypadku debugowania lub zgłaszania problemu. Globalna flaga `--debug` zwiększa szczegółowość plików dzienników.

### <a name="command-help-and-syntax"></a>Polecenia — pomoc i składnia

Aby uzyskać pomoc dotyczącą określonego polecenia lub grupy poleceń, użyj flagi `-h`.

```azurecli
sfctl application -h
```

Oto inny przykład:

```azurecli
sfctl application create -h
```

## <a name="updating-the-service-fabric-cli"></a>Aktualizowanie interfejsu wiersza polecenia usługi Service Fabric 

Aby zaktualizować interfejs wiersza polecenia usługi Service Fabric, uruchom następujące polecenia (element `pip` zastąp elementem `pip3` w zależności od opcji wybranej podczas oryginalnej instalacji):

```bash
pip uninstall sfctl 
pip install sfctl 
```


## <a name="next-steps"></a>Następne kroki

* [Deploy an application with the Azure Service Fabric CLI (Wdrażanie aplikacji przy użyciu interfejsu wiersza polecenia usługi Azure Service Fabric)](service-fabric-application-lifecycle-sfctl.md)
* [Wprowadzenie do usługi Service Fabric w systemie Linux](service-fabric-get-started-linux.md)

