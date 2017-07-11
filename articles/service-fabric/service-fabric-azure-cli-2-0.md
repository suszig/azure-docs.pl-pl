---
title: "Wprowadzenie do usługi Service Fabric i interfejsu wiersza polecenia platformy Azure 2.0"
description: "Jak używać modułu poleceń usługi Service Fabric w wierszu polecenia platformy Azure 2.0, w tym jak połączyć się z klastrem aplikacji i zarządzać aplikacjami"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: c5cc6e54acf27456185eeb48858c4d981aa46b4b
ms.contentlocale: pl-pl
ms.lasthandoff: 07/01/2017

---
<a id="service-fabric-and-azure-cli-20" class="xliff"></a>

# Usługa Service Fabric i interfejs wiersza polecenia platformy Azure 2.0

Nowy interfejs wiersza polecenia platformy Azure 2.0 obejmuje teraz polecenia umożliwiające zarządzanie klastrami usługi Service Fabric. W tej dokumentacji przedstawiono czynności umożliwiające rozpoczęcie korzystania z interfejsu wiersza polecenia platformy Azure.

<a id="install-azure-cli-20" class="xliff"></a>

## Instalowanie interfejsu wiersza polecenia platformy Azure 2.0

Interfejs wiersza polecenia platformy Azure obejmuje teraz polecenia umożliwiające interakcję z klastrami usługi Service Fabric i zarządzanie nimi. Możesz przeprowadzić [standardowy proces instalacji](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), aby pobrać najnowszy interfejs wiersza polecenia platformy Azure.

Więcej informacji można znaleźć w [dokumentacji interfejsu wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/overview)

<a id="cli-syntax" class="xliff"></a>

## Składnia interfejsu wiersza polecenia

Wszystkie polecenia usługi Azure Service Fabric mają prefiks `az sf` w interfejsie wiersza polecenia platformy Azure. Aby uzyskać więcej informacji na temat dostępnych poleceń, można uruchomić polecenie `az sf -h` w celu uzyskania ogólnych informacji. Można również uruchomić polecenie `az sf <command> -h`, aby uzyskać szczegółową pomoc dotyczącą jednego polecenia.

Polecenia usługi Azure Service Fabric w interfejsie wiersza polecenia są zgodne ze wzorcem nazewnictwa

```azurecli
az sf <object> <action>
```

W tym przypadku element `<object>` jest obiektem docelowym elementu `<action>`.

<a id="selecting-a-cluster" class="xliff"></a>

## Wybieranie klastra

Przed wykonaniem jakiejkolwiek operacji musisz wybrać klaster, z którym chcesz nawiązać połączenie. Aby na przykład nawiązać połączenie z niezabezpieczonym klastrem, zapoznaj się z poniższym fragmentem kodu.

> [!WARNING]
> Nie używaj niezabezpieczonych klastrów usługi Service Fabric w przypadku środowisk produkcyjnych

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Punkt końcowy klastra musi mieć prefiks `http` lub `https` i musi obejmować port bramy HTTP. Ten port i adres jest taki sam jak adres URL narzędzia Service Fabric Explorer.

W przypadku klastrów zabezpieczonych przy użyciu certyfikatu są obsługiwane niezaszyfrowane pliki `pem` lub `crt` i pliki `key`.

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Więcej informacji można znaleźć w [szczegółowym dokumencie dotyczącym łączenia się z zabezpieczonymi klastrami](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Wybrane polecenie nie wykonuje żadnych żądań przed zwróceniem wyniku. Aby sprawdzić, czy poprawnie określono klaster, uruchom polecenie, takie jak `az sf cluster health`, i upewnij się, że polecenie nie zwraca żadnych błędów.

<a id="performing-basic-operations" class="xliff"></a>

## Wykonywanie podstawowych operacji

Informacje o połączeniu klastra są utrwalane w różnych sesjach interfejsu wiersza polecenia platformy Azure. Po wybraniu klastra usługi Service Fabric można uruchomić dowolne polecenie usługi Service Fabric.

Aby na przykład uzyskać informacje o kondycji klastra usługi Service Fabric, uruchom następujące polecenie

```azurecli
az sf cluster health
```

Polecenie zwróci następujące dane wyjściowe przy założeniu, że określono dane wyjściowe JSON w konfiguracji interfejsu wiersza polecenia platformy Azure

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

<a id="tips-and-faq" class="xliff"></a>

## Porady i często zadawane pytania

Poniżej przedstawiono informacje, które mogą być pomocne w razie problemów podczas używania poleceń usługi Service Fabric w interfejsie wiersza polecenia platformy Azure

<a id="converting-a-certificate-from-pfx-to-pem" class="xliff"></a>

### Konwertowanie certyfikatu z formatu PFX na format PEM

Interfejs wiersza polecenia platformy Azure obsługuje certyfikaty po stronie klienta w postaci plików PEM (rozszerzenie `.pem`). Jeśli korzystasz z plików PFX z systemu Windows, musisz przekonwertować te certyfikaty na format PEM. Aby przekonwertować plik PFX na plik PEM, użyj następującego polecenia:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Aby uzyskać szczegółowe informacje, zapoznaj się z [dokumentacją rozwiązania OpenSSL](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html).

<a id="connection-issues" class="xliff"></a>

### Problemy z połączeniem

Podczas wykonywania operacji może wystąpić następujący błąd:

> `Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

W takim przypadku sprawdź dokładnie, czy określony punkt końcowy klastra jest dostępny i nasłuchuje. Ponadto sprawdź, czy interfejs użytkownika narzędzia Service Fabric Explorer jest dostępny na tym hoście i porcie. Zaktualizuj punkt końcowy przy użyciu polecenia `az sf cluster select`.

<a id="getting-detailed-logs" class="xliff"></a>

### Uzyskiwanie szczegółowych dzienników

W przypadku debugowania lub zgłaszania problemu warto dołączyć szczegółowe dzienniki. Interfejs wiersza polecenia platformy Azure zawiera globalną flagę `--debug`, która zwiększa poziom szczegółowości dzienników.

<a id="command-help-and-syntax" class="xliff"></a>

### Polecenia — pomoc i składnia

Polecenia usługi Service Fabric korzystają z tej samej konwencji co interfejs wiersza polecenia platformy Azure. Określ flagę `-h`, aby uzyskać pomoc dotyczącą polecenia lub grupy poleceń. Na przykład:

```azurecli
az sf application -h
```

lub

```azurecli
az sf application create -h
```

