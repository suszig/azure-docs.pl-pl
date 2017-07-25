---
title: "Wprowadzenie do usługi Azure Service Fabric i interfejsu wiersza polecenia platformy Azure 2.0"
description: "Dowiedz się, jak korzystać z modułu poleceń usługi Azure Service Fabric w interfejsie wiersza polecenia platformy Azure w wersji 2.0. Dowiedz się, jak nawiązać połączenie z klastrem i zarządzać aplikacjami."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: ee3302b984ca2f5509755dc17b0a5fd06ace0afe
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---
# <a name="azure-service-fabric-and-azure-cli-20"></a>Usługa Azure Service Fabric i interfejs wiersza polecenia platformy Azure 2.0

Narzędzie wiersza polecenia platformy Azure w wersji 2.0 zawiera polecenia, które ułatwiają zarządzanie klastrami usługi Azure Service Fabric. Dowiedz się, jak rozpocząć pracę z interfejsem wiersza polecenia platformy Azure i usługą Service Fabric.

## <a name="install-azure-cli-20"></a>Instalowanie interfejsu wiersza polecenia platformy Azure 2.0

Polecenia interfejsu wiersza polecenia platformy Azure 2.0 umożliwiają interakcję z klastrami usługi Service Fabric oraz zarządzanie nimi. Aby uzyskać najnowszą wersję interfejsu wiersza polecenia platformy Azure, wykonaj kroki [standardowego procesu instalacji interfejsu wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

Aby uzyskać więcej informacji, zobacz temat [Omówienie interfejsu wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Składnia interfejsu wiersza polecenia platformy Azure

W interfejsie wiersza polecenia platformy Azure wszystkie polecenia usługi Service Fabric mają prefiks `az sf`. Aby uzyskać ogólne informacje na temat używanych poleceń, z których możesz korzystać, użyj polecenia `az sf -h`. Aby uzyskać pomoc dotyczącą pojedynczego polecenia, użyj polecenia `az sf <command> -h`.

Polecenia usługi Service Fabric w interfejsie wiersza polecenia platformy Azure są zgodne z następującym wzorcem nazewnictwa:

```azurecli
az sf <object> <action>
```

Element `<object>` jest obiektem docelowym elementu `<action>`.

## <a name="select-a-cluster"></a>Wybieranie klastra

Przed wykonaniem jakiejkolwiek operacji musisz wybrać klaster, z którym chcesz nawiązać połączenie. Jako przykład zobacz następujący kod. Kod umożliwia nawiązanie połączenia z niezabezpieczonym klastrem.

> [!WARNING]
> Nie używaj niezabezpieczonych klastrów usługi Service Fabric w środowisku produkcyjnym.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Punkt końcowy klastra musi mieć prefiks `http` lub `https`. Musi on zawierać port bramy HTTP. Port i adres są takie same jak adres URL programu Service Fabric Explorer.

W przypadku klastrów zabezpieczonych za pomocą certyfikatu można użyć niezaszyfrowanych plików PEM albo plików CRT i KEY. Na przykład:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Aby uzyskać więcej informacji, zobacz temat [Nawiązywanie połączenia z zabezpieczonym klastrem usługi Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Przed zwróceniem wartości polecenie `select` nie reaguje na żadne żądania. Aby sprawdzić, czy klaster został określony poprawnie, użyj polecenia, takiego jak `az sf cluster health`. Upewnij się, że polecenie nie zwraca żadnych błędów.

## <a name="basic-operations"></a>Operacje podstawowe

Informacje o połączeniu klastra są utrwalane w wielu sesjach interfejsu wiersza polecenia platformy Azure. Po wybraniu klastra usługi Service Fabric można uruchomić dowolne polecenie usługi Service Fabric w klastrze.

Aby na przykład uzyskać informacje o kondycji klastra usługi Service Fabric, uruchom następujące polecenie:

```azurecli
az sf cluster health
```

Polecenie zwróci następujące dane wyjściowe (przy założeniu, że dane wyjściowe JSON zostały określone w konfiguracji interfejsu wiersza polecenia platformy Azure):

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

Poniższe informacje mogą być pomocne, jeśli wystąpiły problemy podczas korzystania z poleceń usługi Service Fabric w interfejsie wiersza polecenia platformy Azure.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Konwertowanie certyfikatu z formatu PFX na PEM

Interfejs wiersza polecenia platformy Azure obsługuje certyfikaty po stronie klienta w postaci plików PEM (rozszerzenie pem). Jeśli używasz plików PFX z systemu Windows, musisz konwertować te certyfikaty na format PEM. Aby konwertować plik PFX na plik PEM, użyj następującego polecenia:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją dotyczącą protokołu OpenSSL](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Problemy z połączeniem

Niektóre operacje mogą generować następujący komunikat:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Sprawdź, czy punkt końcowy określonego klastra jest dostępny i przeprowadza nasłuchiwanie. Sprawdź również, czy interfejs użytkownika programu Service Fabric Explorer jest dostępny na tym hoście i porcie. Aby zaktualizować punkt końcowy, użyj polecenia `az sf cluster select`.

### <a name="detailed-logs"></a>Szczegółowe dzienniki

Szczegółowe dzienniki często bywają przydatne w przypadku debugowania lub zgłaszania problemu. Interfejs wiersza polecenia platformy Azure zawiera globalną flagę `--debug`, która zwiększa poziom szczegółowości plików dzienników.

### <a name="command-help-and-syntax"></a>Polecenia — pomoc i składnia

Polecenia usługi Service Fabric korzystają z tej samej konwencji, co interfejs wiersza polecenia platformy Azure. Aby uzyskać pomoc dotyczącą określonego polecenia lub grupy poleceń, użyj flagi `-h`:

```azurecli
az sf application -h
```

Oto inny przykład:

```azurecli
az sf application create -h
```

