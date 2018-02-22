---
title: "Użyj przypisany użytkownik zarządzane usługi tożsamości (MSI) na maszynie Wirtualnej systemu Linux, aby uzyskać dostępu do bazy danych Azure rozwiązania Cosmos"
description: "Samouczek, który przeprowadzi Cię przez proces przy użyciu User-Assigned zarządzane usługi tożsamości (MSI) na maszynie Wirtualnej systemu Linux, można uzyskać dostępu do bazy danych Azure rozwiązania Cosmos."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2018
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2c0c3597999e80af86f079385653d94ddfcab245
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-cosmos-db"></a>Użyj przypisany użytkownik zarządzane usługi tożsamości (MSI) na maszynie Wirtualnej systemu Linux, aby uzyskać dostępu do bazy danych Azure rozwiązania Cosmos 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

W tym samouczku przedstawiono sposób tworzenia i użyj przypisany użytkownik zarządzane usługi tożsamości (MSI) z maszyny wirtualnej systemu Linux, a następnie umożliwia dostęp do bazy danych Azure rozwiązania Cosmos. Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz użytkownika przypisać tożsamość usługi zarządzanej (MSI)
> * Przypisz MSI użytkownik przypisany do maszyny wirtualnej systemu Linux
> * Udziel dostępu do pliku MSI na wystąpienie bazy danych Azure rozwiązania Cosmos
> * Uzyskaj token dostępu przy użyciu tożsamości MSI przypisane przez użytkownika i umożliwia dostęp do bazy danych Azure rozwiązania Cosmos

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Aby uruchomić przykłady skryptów interfejsu wiersza polecenia, w tym samouczku, masz dwie opcje:

- Użyj [powłoki chmury Azure](~/articles/cloud-shell/overview.md) w portalu Azure lub za pośrednictwem **spróbuj on** przycisk znajdujący się w prawym górnym rogu każdej blok kodu.
- [Zainstaluj najnowszą wersję interfejsu wiersza polecenia 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 lub nowsza), jeśli wolisz korzystać z konsoli lokalnej interfejsu wiersza polecenia.

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Utwórz maszynę wirtualną systemu Linux w nowej grupy zasobów

W tym samouczku utworzymy nową maszynę Wirtualną systemu Linux. Można również włączyć MSI na istniejącej maszyny Wirtualnej.

1. Kliknij przycisk **+ Utwórz zasób** znaleziono w lewym górnym rogu portalu Azure.
2. Wybierz **obliczeniowe**, a następnie wybierz **maszyny Wirtualnej systemu Ubuntu Server 16.04 LTS**.
3. Wprowadź informacje o maszynie wirtualnej. Aby uzyskać **typ uwierzytelniania**, wybierz pozycję **klucz publiczny SSH** lub **hasło**. Utworzony poświadczenia umożliwiają Zaloguj się do maszyny Wirtualnej.

    ![MSI Linux VM](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wybierz **subskrypcji** dla maszyny wirtualnej na liście rozwijanej.
5. W obszarze **grupy zasobów**, wybierz **Utwórz nowy** i wpisz nazwę grupy zasobów dla tej maszyny Wirtualnej. Po zakończeniu kliknij przycisk **OK**.
6. Wybierz rozmiar maszyny wirtualnej. Aby wyświetlić więcej rozmiary, wybierz **Wyświetl wszystkie** lub zmień filtr typu dysku obsługiwane. W **ustawienia** bloku, Zachowaj wartości domyślne i kliknij przycisk **OK**.

## <a name="create-a-user-assigned-msi"></a>Tworzenie pliku MSI przypisany użytkownik

1. Jeśli używana jest konsola interfejsu wiersza polecenia (zamiast sesję powłoki chmury Azure), Rozpocznij od logowanie do platformy Azure. Użyj konta, które jest skojarzone z subskrypcją platformy Azure, w którym chcesz utworzyć nowy plik MSI:

    ```azurecli
    az login
    ```

2. Tworzenie przypisany użytkownik MSI przy użyciu [utworzenia tożsamości az](/cli/azure/identity#az_identity_create). `-g` Parametr określa grupę zasobów, w której jest tworzony plik MSI, a `-n` parametr określa jego nazwę. Pamiętaj zastąpić `<RESOURCE GROUP>` i `<MSI NAME>` wartości parametrów z własne wartości:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Odpowiedź zawiera szczegóły dla utworzonego pliku MSI przypisane przez użytkownika, podobny do poniższego przykładu (Uwaga `clientId` i `id` wartości z pliku MSI, ponieważ są używane w kolejnych krokach):

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Przypisz MSI użytkownika przypisane przez użytkownika do maszyny Wirtualnej systemu Linux

W odróżnieniu od przypisane systemu pliku MSI Instalatora MSI przypisany użytkownik może służyć przez klientów na wielu zasobów platformy Azure. W tym samouczku można przypisać do jednej maszyny Wirtualnej. Można również przypisać do więcej niż jednej maszyny Wirtualnej.

Przypisz MSI użytkownik przypisany do maszyny Wirtualnej systemu Linux przy użyciu [tożsamość Przypisz az maszyny wirtualnej](/cli/azure/vm#az_vm_assign_identity). Pamiętaj zastąpić `<RESOURCE GROUP>` i `<VM NAME>` wartości parametrów z własne wartości. Użyj `id` właściwości zwracane w poprzednim kroku dla `--identities` wartość parametru:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-cosmos-db-account"></a>Tworzenie konta usługi Cosmos DB 

Jeśli nie masz jeszcze jeden, teraz utworzyć konta DB rozwiązania Cosmos. Można również pominąć ten krok i użyj istniejącego konta rozwiązania Cosmos bazy danych, jeśli wolisz. 

1. Kliknij przycisk **+/ Utwórz nową usługę** znaleziono przycisku w lewym górnym rogu portalu Azure.
2. Kliknij przycisk **baz danych**, następnie **bazy danych Azure rozwiązania Cosmos**i nowy "nowe konto" Wyświetla panelu.
3. Wprowadź **identyfikator** dla konta rozwiązania Cosmos bazy danych, które można użyć później.  
4. **Interfejs API** powinien być ustawiony na "SQL". Podejście opisane w tym samouczku mogą być używane z innych dostępnych typów interfejsu API, ale czynności opisane w tym samouczku są dla interfejsu API SQL.
5. Upewnij się, **subskrypcji** i **grupy zasobów** odpowiadały określony podczas tworzenia maszyny Wirtualnej w poprzednim kroku.  Wybierz **lokalizacji** gdzie dostępna jest opcja DB rozwiązania Cosmos.
6. Kliknij przycisk **Utwórz**.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Tworzenie kolekcji w ramach konta bazy danych rozwiązania Cosmos

Następnie dodaj zbierania danych w ramach konta rozwiązania Cosmos bazy danych, które można zbadać w kolejnych krokach.

1. Przejdź do nowo utworzonego konta DB rozwiązania Cosmos.
2. Na **omówienie** kliknij kartę **+/ Dodaj kolekcji** przycisk i "Dodaj"kolekcji panelu slajdów wychodzących.
3. Nadaj kolekcji select ID, identyfikator kolekcji bazy danych o pojemności, wprowadź klucz partycji, wprowadź wartość przepływności, a następnie kliknij **OK**.  W tym samouczku wystarczy użyć "Test", jak identyfikator bazy danych, identyfikator kolekcji, wybierz stały pojemności i najniższego przepływności (400 RU/s).

## <a name="grant-your-user-assigned-msi-access-to-the-cosmos-db-account-access-keys"></a>Przyznać uprawnienia użytkownika MSI przypisane do klucze dostępu do konta DB rozwiązania Cosmos

Rozwiązania cosmos bazy danych nie obsługuje natywnie uwierzytelniania usługi Azure AD.  Jednak można użyć Instalatora MSI można pobrać klucz dostępu DB rozwiązania Cosmos z Menedżera zasobów, a następnie za pomocą klawisza dostępu DB rozwiązania Cosmos.  W tym kroku należy przyznać użytkownika przypisane MSI dostępu do kluczy do konta DB rozwiązania Cosmos.

Najpierw udostępnienia MSI tożsamości konto bazy danych rozwiązania Cosmos w usłudze Azure Resource Manager przy użyciu wiersza polecenia platformy Azure. Zaktualizuj wartości dla `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, i `<COSMOS DB ACCOUNT NAME>` dla danego środowiska. Zastąp `<MSI PRINCIPALID>` z `principalId` właściwości zwróconej przez `az identity create` w [utworzyć instalatora MSI przypisany użytkownik](#create-a-user-assigned-msi).  Rozwiązania cosmos bazy danych obsługuje dwa poziomy szczegółowości klawiszy dostępu: odczytu/zapisu dostępu do konta, a także dostęp tylko do odczytu do konta.  Przypisz `DocumentDB Account Contributor` roli, jeśli chcesz pobrać klucze odczytu/zapisu dla konta lub przypisać `Cosmos DB Account Reader Role` roli, jeśli chcesz pobrać klucze tylko do odczytu dla konta:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

Odpowiedź zawiera szczegóły dotyczące przypisania roli, utworzyć:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-user-assigned-msi-and-use-it-to-call-azure-resource-manager"></a>Uzyskaj token dostępu przy użyciu MSI przypisany użytkownik i użyć go do wywołania usługi Azure Resource Manager

W pozostałej części tego samouczka Praca z maszyny Wirtualnej utworzone wcześniej.

Aby wykonać te kroki, należy klient SSH. Jeśli korzystasz z systemu Windows, możesz użyć klienta SSH w [podsystemu systemu Windows dla systemu Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Jeśli potrzebujesz pomocy w konfigurowaniu kluczy klienta SSH, zobacz [sposobu użycia SSH kluczy systemu Windows Azure](../../virtual-machines/linux/ssh-from-windows.md), lub [sposobu tworzenia i używania SSH publiczne i prywatne parę kluczy dla maszyn wirtualnych systemu Linux na platformie Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. W portalu Azure, przejdź do **maszyn wirtualnych**, przejdź do maszyny wirtualnej systemu Linux, następnie z **omówienie** kliknij **Connect** u góry. Skopiuj ciąg, aby nawiązać połączenie z maszyną Wirtualną. 
2. Nawiązać połączenie z maszyną Wirtualną przy użyciu klienta SSH.  
3. Następnie zostanie wyświetlony monit o wprowadź w Twojej **hasło** dodane podczas tworzenia **maszyny Wirtualnej systemu Linux**. Użytkownik powinien następnie można pomyślnie zarejestrowany.  
4. Umożliwia ZWINIĘCIE Uzyskaj token dostępu usługi Azure Resource Manager.  

    ZWINIĘCIE żądania i odpowiedzi tokenu dostępu jest niższa.  Zastąp <CLIENT ID> clientId wartość użytkownika przypisywane MSI:
    
    ```bash
    curl 'http://localhost:50342/oauth2/token?resource=https://management.azure.com/&client_id=<CLIENT ID>' -H "Metadata:true"
    ```
    
    > [!NOTE]
    > W żądaniu poprzedniej wartości parametru "zasobu" musi być dokładnego dopasowania dla oczekiwano przez usługę Azure AD. Podczas korzystania z usługi Azure Resource Manager identyfikator zasobu, należy dołączyć końcowy ukośnik w identyfikatorze URI.
    > W poniższych odpowiedzi elementu ' access_token ', jak została skrócona do skrócenia.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Uzyskaj klucze dostępu z usługi Azure Resource Manager w celu wykonywania wywołań DB rozwiązania Cosmos  

Teraz używać CURL, aby wywołać Resource Manager przy użyciu tokenu dostępu, możemy pobrany w poprzedniej sekcji, aby pobrać klucz dostępu do konta DB rozwiązania Cosmos. Gdy będziemy mieć klucz dostępu, możemy zapytania DB rozwiązania Cosmos. Pamiętaj zastąpić `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, i `<COSMOS DB ACCOUNT NAME>` wartości parametrów z własne wartości. Zastąp `<ACCESS TOKEN>` wartość przy użyciu tokenu dostępu został wcześniej pobrany.  Jeśli chcesz pobrać klucze odczytu/zapisu, użyj operacji klucza typu `listKeys`.  Jeśli chcesz pobrać klucze tylko do odczytu, użyj typu klucza operacji `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Tekst w adresie URL wcześniejsze jest rozróżniana wielkość liter, dlatego upewnij się, jeśli używasz górna małe dla grupy zasobów, uwzględnienie w związku z tym. Ponadto jest ważne dowiedzieć się, że jest to żądanie POST nie żądanie GET i upewnij się, że należy przekazać wartość do przechwytywania limit długości - d, który może mieć wartości NULL.  

Odpowiedzi CURL zawiera listę kluczy.  Na przykład, jeśli zostanie wyświetlony tylko do odczytu klucze:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Teraz, po klucz dostępu dla konta rozwiązania Cosmos bazy danych, możesz przekazać go do SDK DB rozwiązania Cosmos i wywoływać w celu uzyskania dostępu do konta.  Na przykład szybki można przekazać klucz dostępu do wiersza polecenia platformy Azure.  Możesz uzyskać <COSMOS DB CONNECTION URL> z **omówienie** kartę w bloku konta DB rozwiązania Cosmos w portalu Azure.  Zastąp <ACCESS KEY> o wartości otrzymanych powyżej:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

To polecenie interfejsu wiersza polecenia zwraca szczegółowe informacje o kolekcji:

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>Kolejne kroki

- Omówienie MSI, zobacz [zarządzane usługi tożsamości (MSI) dla zasobów Azure](msi-overview.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.