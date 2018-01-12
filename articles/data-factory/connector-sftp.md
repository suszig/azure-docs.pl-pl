---
title: "Kopiowanie danych z serwera SFTP przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat łącznika MySQL w fabryce danych Azure, która umożliwia kopiowanie danych z serwera SFTP w magazynie danych są obsługiwane jako zbiorniku."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: 0d293d3874b0cb43cee9f85c6c575e87c48ad291
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-from-sftp-server-using-azure-data-factory"></a>Kopiowanie danych z serwera SFTP przy użyciu fabryki danych Azure
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-sftp-connector.md)
> * [Wersja 2 — wersja zapoznawcza](connector-sftp.md)

Ten artykuł przedstawia sposób użycia działanie kopiowania w fabryce danych Azure umożliwia kopiowanie danych z serwera SFTP. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [SFTP łącznika w wersji 1](v1/data-factory-sftp-connector.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Możesz skopiować dane z serwera SFTP żadnych obsługiwanych ujścia magazynu danych. Lista magazynów danych, które są obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

W szczególności ten łącznik SFTP obsługuje:

- Kopiowanie plików za pomocą **podstawowe** lub **parametry SshPublicKey** uwierzytelniania.
- Kopiowanie plików jako — jest lub analizowanie plików z [obsługiwane formaty plików i kodery-dekodery kompresji](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Rozpoczęcie pracy

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do SFTP.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

Usługa SFTP połączone obsługuje następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **Sftp**. |Yes |
| host | Nazwa lub adres IP serwera SFTP. |Yes |
| port | Port, na którym nasłuchuje serwer SFTP.<br/>Dozwolone wartości to: liczba całkowita, wartość domyślna to **22**. |Nie |
| skipHostKeyValidation | Określ, czy pominąć sprawdzanie poprawności klucza hosta.<br/>Dozwolone wartości to: **true**, **false** (ustawienie domyślne).  | Nie |
| hostKeyFingerprint | Podaj odcisk palca klucza hosta. | Tak, jeśli "skipHostKeyValidation" jest ustawiona na wartość false.  |
| Typ authenticationType | Określ typ uwierzytelniania.<br/>Dozwolone wartości to: **podstawowe**, **parametry SshPublicKey**. Zapoznaj się [uwierzytelnianie podstawowe Using](#using-basic-authentication) i [przy użyciu publicznego klucza uwierzytelniania SSH](#using-ssh-public-key-authentication) odpowiednio sekcje więcej właściwości i przykłady JSON. |Yes |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. (Jeśli w magazynie danych znajduje się w sieci prywatnej), można użyć środowiska uruchomieniowego integracji Azure lub Self-hosted integracji w czasie wykonywania. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. |Nie |

### <a name="using-basic-authentication"></a>Przy użyciu uwierzytelniania podstawowego

Aby uwierzytelnianie podstawowe, ustaw właściwość "authenticationType" **podstawowe**i określ następujące właściwości poza łącznika SFTP ogólnego z nich wprowadzone w ostatniej sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| Nazwa użytkownika | Użytkownik, który ma dostęp do serwera SFTP. |Yes |
| hasło | Hasło dla użytkownika (userName). Zaznacz to pole jako SecureString. | Yes |

**Przykład:**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-ssh-public-key-authentication"></a>Przy użyciu uwierzytelniania klucza publicznego SSH

Aby używać uwierzytelniania klucza publicznego SSH, ustaw właściwość "authenticationType" jako **parametry SshPublicKey**i określ następujące właściwości poza łącznika SFTP ogólnego z nich wprowadzone w ostatniej sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| Nazwa użytkownika | Użytkownik, który ma dostęp do serwera SFTP |Yes |
| privateKeyPath | Określ ścieżkę bezwzględną do pliku klucza prywatnego środowiska uruchomieniowego integracji może uzyskać dostęp. Ma zastosowanie tylko wtedy, gdy określono siebie typu środowiska uruchomieniowego integracji w "connectVia". | Określ `privateKeyPath` lub `privateKeyContent`.  |
| privateKeyContent | Treści klucza prywatnego SSH w formacie Base64. Klucz prywatny SSH powinna być w formacie OpenSSH. Zaznacz to pole jako SecureString. | Określ `privateKeyPath` lub `privateKeyContent`. |
| Hasło | Określ przebiegu frazy/hasło do odszyfrowania klucza prywatnego, jeśli plik klucza jest chroniony przez hasło. Zaznacz to pole jako SecureString. | Tak, jeśli hasło jest chroniony plik klucza prywatnego. |

> [!NOTE]
> Łącznik SFTP obsługuje tylko klucz OpenSSH. Upewnij się, że plik klucza jest w nieprawidłowym formacie. Narzędzie Putty umożliwia konwertowanie ppk na OpenSSH format.

**Przykład 1: Parametry SshPublicKey uwierzytelnianie przy użyciu filePath klucza prywatnego**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Przykład 2: Parametry SshPublicKey uwierzytelniania za pomocą prywatnego klucza zawartości**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje zobacz artykuł zestawów danych. Ta sekcja zawiera listę właściwości obsługiwanych przez SFTP zestawu danych.

Aby skopiować dane z użyciem protokołu SFTP, ustaw właściwość Typ zestawu danych do **FileShare**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **udziału plików** |Yes |
| folderPath | Ścieżka do folderu. Na przykład: folder/podfolder / |Yes |
| fileName | Określ nazwę pliku w **folderPath** Aby kopiować z określonego pliku. Jeśli nie określono żadnej wartości dla tej właściwości, punktów zestawu danych do wszystkich plików w folderze źródłowym. |Nie |
| obiektu fileFilter | Określ filtr służący do wybierania podzbioru pliki w ścieżce folderu, a nie wszystkie pliki. Ma zastosowanie tylko wtedy, gdy nie jest określona nazwa pliku. <br/><br/>Dozwolone symbole wieloznaczne są: `*` (wielu znaków) i `?` (pojedynczy znak).<br/>— Przykład 1:`"fileFilter": "*.log"`<br/>— Przykład 2:`"fileFilter": 2017-09-??.txt"` |Nie |
| Format | Jeśli chcesz **skopiuj pliki jako — jest** między opartych na plikach magazynów (kopia binarnego), Pomiń sekcji format w obu definicji zestawu danych wejściowych i wyjściowych.<br/><br/>Jeśli chcesz przeanalizować pliki w określonym formacie, obsługiwane są następujące typy plików w formacie: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Ustaw **typu** właściwości w formacie do jednej z tych wartości. Aby uzyskać więcej informacji, zobacz [formacie tekstowym](supported-file-formats-and-compression-codecs.md#text-format), [formatu Json](supported-file-formats-and-compression-codecs.md#json-format), [Avro Format](supported-file-formats-and-compression-codecs.md#avro-format), [Orc Format](supported-file-formats-and-compression-codecs.md#orc-format), i [Parquet Format](supported-file-formats-and-compression-codecs.md#parquet-format) sekcje. |Nie (tylko w przypadku scenariusza kopiowania binarny) |
| Kompresja | Określ typ i poziom kompresji danych. Aby uzyskać więcej informacji, zobacz [obsługiwane formaty plików i kodery-dekodery kompresji](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Obsługiwane typy to: **GZip**, **Deflate**, **BZip2**, i **ZipDeflate**.<br/>Obsługiwane poziomy: **optymalna** i **najszybciej**. |Nie |

**Przykład:**

```json
{
    "name": "SFTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez SFTP źródła właściwości.

### <a name="sftp-as-source"></a>SFTP jako źródło

Aby skopiować dane z użyciem protokołu SFTP, należy ustawić typ źródła w przypadku działania kopiowania do **FileSystemSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **FileSystemSource** |Yes |
| Cykliczne | Wskazuje, czy dane są odczytywane rekursywnie z folderów sub lub tylko określonego folderu.<br/>Dozwolone wartości to: **true** (ustawienie domyślne), **false** | Nie |

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Kolejne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md##supported-data-stores-and-formats).