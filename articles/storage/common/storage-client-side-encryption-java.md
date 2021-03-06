---
title: "Szyfrowanie po stronie klienta z językiem Java dla magazynu Microsoft Azure | Dokumentacja firmy Microsoft"
description: "Biblioteka klienta magazynu Azure dla języka Java obsługuje szyfrowanie po stronie klienta i integracja z usługą Azure Key Vault dla zapewnienia maksymalnego poziomu bezpieczeństwa dla aplikacji usługi Azure Storage."
services: storage
documentationcenter: java
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 3df49907-554c-404a-9b0c-b3e3269ad04f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: b4f3814ac2dbc8b74cef8f5fcb0540b7509efa0d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="client-side-encryption-and-azure-key-vault-with-java-for-microsoft-azure-storage"></a>Szyfrowanie po stronie klienta i usługi Azure Key Vault z językiem Java dla magazynu Microsoft Azure
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Przegląd
[Biblioteki klienta usługi Azure Storage dla języka Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) obsługuje szyfrowanie danych w aplikacjach klienckich przed przekazaniem do usługi Azure Storage i odszyfrowywania danych podczas pobierania do klienta. Biblioteka obsługuje również integrację z [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) zarządzania kluczami konta magazynu.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Szyfrowanie i odszyfrowywanie za pomocą techniki koperty
Procesy szyfrowania i odszyfrowywania wykonaj technika koperty.  

### <a name="encryption-via-the-envelope-technique"></a>Szyfrowanie za pomocą techniki koperty
Szyfrowanie za pomocą techniki koperty działa w następujący sposób:  

1. Biblioteka klienta magazynu Azure generuje klucz szyfrowania zawartości (CEK), jest jeden jednorazowej użycia klucza symetrycznego.  
2. Dane użytkownika są szyfrowane przy użyciu tego CEK.   
3. CEK jest następnie opakowane (zaszyfrowane) przy użyciu klucza szyfrowania klucza (KEK). KEK jest identyfikowany przez identyfikator klucza i można parę klucza asymetrycznego lub klucza symetrycznego i można być zarządzany lokalnie lub przechowywane w magazynach klucza usługi Azure.  
   Biblioteka klienta magazynu się nigdy nie ma dostęp do KEK. Biblioteka wywołuje algorytm klucza zawijania zapewnianej przez usługi Key Vault. Użytkownicy mogą wybierać dostawców niestandardowych dla klucza zawijania/odkodowywania w razie potrzeby.  
4. Zaszyfrowane dane są następnie przekazywane do usługi Magazyn Azure. Opakowana klucz wraz ze niektóre metadane dodatkowego szyfrowania jest przechowywane jako metadanych (dla obiektu blob) albo interpolowane z zaszyfrowanych danych (wiadomości w kolejce i jednostek tabeli).

### <a name="decryption-via-the-envelope-technique"></a>Odszyfrowywanie przy użyciu techniki koperty
Odszyfrowywanie przy użyciu techniki koperty działa w następujący sposób:  

1. Biblioteka klienta przyjęto założenie, czy użytkownik zarządza klucz szyfrowania klucza (KEK) lokalnie lub w magazynach klucza usługi Azure. Użytkownik nie musi wiedzieć określonego klucza, który był używany do szyfrowania. Zamiast tego klucza programu rozpoznawania nazw, który jest rozpoznawany jako różne identyfikatory klucza kluczy można skonfigurować i używane.  
2. Biblioteka klienta pobierze zaszyfrowanych danych wraz z materiał szyfrowania, który jest przechowywany w usłudze.  
3. Klucz szyfrowania zawartości opakowana (CEK) znajduje się bez otoki (odszyfrowane) przy użyciu klucza szyfrowania klucza (KEK). W tym miejscu ponownie, Biblioteka klienta nie ma dostępu do KEK. Wywołuje po prostu niestandardowej lub dostawcy magazynu kluczy odszyfrowania algorytmu.  
4. Klucz szyfrowania zawartości (CEK) jest następnie używany do odszyfrowywania danych zaszyfrowanych użytkownika.

## <a name="encryption-mechanism"></a>Mechanizm szyfrowania
Biblioteka klienta magazynu używa [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) w celu zaszyfrowania danych użytkownika. W szczególności [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) trybu z użyciem standardu AES. Każdy usługi działa nieco inaczej, dlatego omówimy każdego z nich w tym miejscu.

### <a name="blobs"></a>Obiekty blob
Biblioteka klienta obecnie obsługuje szyfrowanie całego obiektów blob tylko. W szczególności, szyfrowanie jest obsługiwane, gdy użytkownicy używają **przekazać*** metody lub **openOutputStream** metody. Obsługiwane są pliki do pobrania, zarówno pełne i pobieranie zakresu.  

Podczas szyfrowania biblioteki klienckiej generowania losowego inicjowania wektora (IV) 16 bajtów oraz klucz szyfrowania zawartości (CEK) 32 bajtów i wykonać koperty szyfrowanie danych obiektów blob, korzystając z tych informacji. Opakowana CEK i niektóre metadane szyfrowania dodatkowe następnie są przechowywane jako obiektów blob metadane wraz z zaszyfrowanego obiektu blob w usłudze.

> [!WARNING]
> W przypadku edytowania lub przekazywanie metadanych dla obiektu blob, należy upewnij się, że te metadane są zachowywane. Po wysłaniu nowej metadanych bez takich metadanych opakowana CEK, IV i inne metadane zostaną utracone i zawartości obiektu blob nigdy nie być pobieranie.
> 
> 

Pobieranie zaszyfrowanej blob polega na pobraniu zawartości przy użyciu całego obiektu blob **pobierania * / openInputStream** podręczne metody. Opakowana CEK następnie rozpakować i używać razem z IV (przechowywane jako metadane obiektu blob w tym przypadku), aby przywrócić odszyfrowane dane użytkowników.

Pobieranie z dowolnego zakresu (**downloadRange*** metody) w obiekcie blob zaszyfrowane obejmuje dopasowywanie dostarczone przez użytkowników, aby uzyskać niewielkie dodatkowych danych, które można pomyślnie odszyfrować żądanego zakresu zakres.  

Wszystkie typy obiektu blob (blokowe obiekty BLOB, stronicowe i uzupełnialne) mogły być szyfrowane/odszyfrowane przy użyciu tego systemu.

### <a name="queues"></a>Kolejki
Ponieważ wiadomości w kolejce może być dowolnym formacie, Biblioteka klienta definiuje formatu niestandardowego, który obejmuje wektor inicjowania (IV) oraz klucza zaszyfrowanego szyfrowania zawartości (CEK) w treści wiadomości.  

Podczas szyfrowania biblioteki klienckiej generuje losowe IV 16 bajtów wraz z losowe CEK 32 bajtów i wykonuje koperty szyfrowania treści wiadomości kolejki, korzystając z tych informacji. Opakowana CEK i niektóre metadane dodatkowego szyfrowania jest następnie dodawana do komunikatu w kolejce zaszyfrowane. Ten komunikat modyfikacji (pokazana poniżej) są przechowywane w usłudze.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Podczas odszyfrowywania opakowana klucz zostaje wyodrębniony z komunikatu w kolejce i nie opakowanych. IV jest również wyodrębniony z komunikatu w kolejce i używać razem z kluczem odkodowany do odszyfrowania danych kolejki wiadomości. Należy pamiętać, że metadane szyfrowania mały (w obszarze 500 bajtów), więc podczas jego wliczane limit 64KB komunikatu w kolejce, wpływ powinno być łatwe w zarządzaniu.

### <a name="tables"></a>Tabele
Biblioteka klienta obsługuje szyfrowanie właściwości obiektów w operacjach wstawiania i zamienianie operacji.

> [!NOTE]
> Scalanie nie jest obecnie obsługiwane. Ponieważ podzbiór właściwości może być zaszyfrowany wcześniej za pomocą innego klucza, po prostu scalanie nowych właściwości i aktualizowania metadanych spowoduje utratę danych. Scalanie albo wymaga wywołań usługi dodatkowe odczytać istniejące jednostki z usługi lub przy użyciu nowego klucza dla właściwości, które nie są odpowiednie ze względu na wydajność.
> 
> 

Szyfrowanie danych tabeli działa w następujący sposób:  

1. Użytkownicy określić właściwości, które mają być szyfrowane.  
2. Biblioteka klienta generuje losowe inicjowania wektora (IV) 16 bajtów oraz klucza szyfrowania zawartości (CEK) 32 bajtów dla każdej jednostki i wykonuje szyfrowanie koperty na poszczególnych właściwości w celu zaszyfrowania przez wyprowadzanie nowy IV dla właściwości. Zaszyfrowane właściwości są przechowywane jako dane binarne.  
3. Opakowana CEK i niektóre metadane szyfrowania dodatkowe następnie są przechowywane jako dwa dodatkowe właściwości zastrzeżone. Pierwszą właściwością zarezerwowane (_ClientEncryptionMetadata1) jest właściwości ciągu, która przechowuje informacje dotyczące IV, wersji i klucz opakowana. Właściwość drugi zarezerwowane (_ClientEncryptionMetadata2) jest właściwość binarną, która przechowuje informacje o właściwościach, które są szyfrowane. Informacje przedstawione w tej drugiej właściwości (_ClientEncryptionMetadata2) jest zaszyfrowany.  
4. Z powodu te dodatkowe właściwości zastrzeżone wymagana na potrzeby szyfrowania użytkownicy mogą teraz mieć tylko 250 właściwości niestandardowych zamiast 252. Całkowity rozmiar jednostki musi być mniejszy niż 1MB.  
   
   Należy pamiętać, że tylko właściwości parametrów mogą być szyfrowane. Inne typy właściwości mają być szyfrowane, musi zostać przekonwertowana na ciągi. Zaszyfrowane ciągi są przechowywane w usłudze jako właściwości binarnych i są one konwertowana z powrotem do ciągów po odszyfrowywania.
   
   W przypadku tabel, oprócz zasad szyfrowania użytkownicy muszą określić właściwości, które mają być szyfrowane. Można to zrobić, określając atrybutu [Szyfruj] (dla jednostki POCO, które pochodzą z TableEntity) lub szyfrowania, program rozpoznawania nazw w opcjach żądania. Rozwiązywanie problemów z szyfrowania jest delegata, który ma klucz partycji, klucz wiersza i nazwy właściwości i zwraca wartość boolean wskazującą, czy ma być szyfrowana tej właściwości. Podczas szyfrowania biblioteki klienckiej użyje tych informacji do określania, czy właściwości powinny być szyfrowane podczas zapisywania można przewodowo. Delegat także możliwość logiki wokół jak właściwości są szyfrowane. (Na przykład, jeśli X, następnie szyfrować właściwości A; w przeciwnym razie szyfrowania właściwości A i B.) Należy pamiętać, że nie jest niezbędne do zapewnienia tych informacji podczas odczytywania lub zapytanie jednostki.

### <a name="batch-operations"></a>Operacje w trybie wsadowym
W operacji wsadowych tego samego klucza KEK zostanie użyty przez wszystkie wiersze w tej operacji zbiorczej ponieważ biblioteki klienta umożliwia tylko jeden obiekt opcje (i dlatego jednej zasady/KEK) dla operacji zbiorczej. Jednak biblioteka klienta wewnętrznie wygeneruje nowy losowych IV i losowe CEK wierszu w partii. Użytkowników można również zaszyfrować inne właściwości dla każdej operacji w partii, definiując to zachowanie w szyfrowania programu rozpoznawania nazw.

### <a name="queries"></a>Zapytania
> [!NOTE]
> Ponieważ jednostek są szyfrowane, nie można uruchomić zapytania, które filtrować we właściwości zaszyfrowanej.  Jeśli spróbujesz, wyniki będą niepoprawne, ponieważ usługa będzie próby porównania zaszyfrowane dane z niezaszyfrowanych danych.
> 
>
Aby wykonać operacje zapytań, należy określić klucza programu rozpoznawania nazw, która jest w stanie rozwiązać wszystkich kluczy w zestawie wyników. Jeśli nie można rozpoznać jednostki zawarty w wyniku zapytania do dostawcy, biblioteki klienckiej zgłosi błąd. Dla dowolnego zapytania, który wykonuje projekcje po stronie serwera biblioteki klienckiej doda właściwości metadanych szyfrowania specjalne (_ClientEncryptionMetadata1 i _ClientEncryptionMetadata2) domyślnie do zaznaczonych kolumnach.

## <a name="azure-key-vault"></a>W usłudze Azure Key Vault
Usługa Azure Key Vault ułatwia ochronę kluczy kryptograficznych i kluczy tajnych używanych przez aplikacje i usługi w chmurze. Za pomocą usługi Azure Key Vault, użytkownicy mogą szyfrować klucze i klucze tajne (takie jak klucze uwierzytelniania, klucze konta magazynu, klucze szyfrowania danych. Pliki PFX oraz hasła) przy użyciu kluczy chronionych przez sprzętowe moduły zabezpieczeń (HSM). Aby uzyskać więcej informacji, zobacz [co to jest usługa Azure Key Vault?](../../key-vault/key-vault-whatis.md).

Biblioteka klienta magazynu używa podstawowej biblioteki usługi Key Vault w celu zapewnienia wspólnej struktury całej Azure do zarządzania kluczami. Użytkownicy otrzymują również dodatkową korzyścią z używania biblioteki rozszerzenia usługi Key Vault. Biblioteka rozszerzeń zawiera przydatne funkcje wokół łatwego i Symmetric/RSA lokalnych i chmurze dostawców klucza, a także z agregacji i buforowania.

### <a name="interface-and-dependencies"></a>Interfejs i zależności
Istnieją trzy pakiety usługi Key Vault:  

* Azure keyvault podstawowe zawiera IKey i IKeyResolver. Jest mała pakietu bez zależności. Biblioteki klienta usługi storage dla języka Java definiuje ją jako zależności.
* Azure keyvault zawiera klienta REST magazynu kluczy.  
* Azure — keyvault rozszerzenia zawiera rozszerzenia kod, który obejmuje implementacje algorytmów kryptograficznych i RSAKey i SymmetricKey. Zależy od podstawowej i KeyVault przestrzeni nazw, a funkcje do definiowania agregacji rozpoznawania nazw (gdy użytkownicy chcą korzystać z wielu dostawców klucza) i buforowania rozpoznawania klucza. Mimo że biblioteki klienta usługi storage nie bezpośrednio zależy od tego pakietu, jeśli użytkownicy chcą za pomocą usługi Azure Key Vault do przechowywania ich kluczy lub używanie rozszerzeń usługi Key Vault korzystać z lokalnej i w chmurze dostawcy usług kryptograficznych, potrzebują tego pakietu.  
  
  Key Vault jest zaprojektowana dla kluczy głównych wysokiej wartości i ograniczania przepustowości limity dla usługi Key Vault zostały zaprojektowane z tym pamiętać. Podczas wykonywania szyfrowania po stronie klienta z magazynu kluczy, model preferowany jest do używania kluczy symetrycznych wzorca przechowywane jako kluczy tajnych w magazynie kluczy i buforowane lokalnie. Użytkownicy, wykonaj następujące czynności:  

1. Utwórz klucz tajny w trybie offline i przekaż go do magazynu kluczy.  
2. Użyj identyfikatora podstawowego klucza tajnego jako parametr rozwiązać bieżącą wersję tajnego klucza szyfrowania i pamięci podręcznej te informacje lokalnie. Użyj CachingKeyResolver do buforowania; Użytkownicy nie powinny implementować własnej logiki buforowania.  
3. Użyj buforowania rozpoznawania jako dane wejściowe podczas tworzenia zasady szyfrowania.
   Więcej informacji na temat użycia usługi Key Vault znajduje się w przykładach kodu szyfrowania. <fix URL>  

## <a name="best-practices"></a>Najlepsze praktyki
Obsługa szyfrowania jest dostępna tylko w przypadku biblioteki klienta usługi storage dla języka Java.

> [!IMPORTANT]
> Należy pamiętać o tych punktów ważne przy użyciu szyfrowania po stronie klienta:
> 
> * Gdy odczyt lub zapis do obiektu blob zaszyfrowane, użyj całego obiektu blob przekazywania oraz poleceń pobieranie obiektu blob zakresu/całości. Należy unikać pisania do obiektu blob zaszyfrowane za pomocą protokołu operacji, takich jak Put bloku, umieść listy zablokowanych, zapisu stron, wyczyść strony lub Dołącz bloku; w przeciwnym razie możesz uszkodzony zaszyfrowany obiekt blob i stał się niemożliwe do odczytania.
> * W przypadku tabel istnieje ograniczenie podobne. Należy zachować ostrożność nie aktualizować zaszyfrowane właściwości bez aktualizowania metadanych szyfrowania.  
> * Jeśli ustawisz metadanych w obiekcie blob zaszyfrowane, mogą zastąpić metadane związane z szyfrowaniem wymaganych do odszyfrowania, ponieważ ustawienie metadanych nie jest dodatek. Dotyczy to również migawki; Unikaj określania metadanych podczas tworzenia migawki obiektu blob zaszyfrowane. Jeśli metadane muszą być ustawione, należy wywołać **downloadAttributes** metodę najpierw uzyskać bieżące metadane szyfrowania i uniknąć równoczesnych zapisów podczas ustawiania metadanych.  
> * Włącz **requireEncryption** flagi w domyślnych opcji żądania dla użytkowników, którzy powinien działać wyłącznie z zaszyfrowanych danych. Aby uzyskać więcej informacji, zobacz poniżej.  
> 
> 

## <a name="client-api--interface"></a>Interfejs API klienta / interfejsu
Podczas tworzenia obiektu EncryptionPolicy, użytkownicy mogą podać tylko klucz (Implementowanie IKey), tylko program rozpoznawania nazw (Implementowanie IKeyResolver) lub obie. IKey jest podstawowy typ klucza, który jest identyfikowany przy użyciu identyfikatora klucza i zapewnia logiki zawijania/odkodowywania. IKeyResolver jest używany do rozpoznawania klucza podczas odszyfrowywania. Definiuje metodę ResolveKey, która zwraca IKey podany identyfikator klucza. Zapewnia to użytkownikom wybór między wiele kluczy, które są zarządzane na wiele lokalizacji.

* Dla celów szyfrowania jest on używany zawsze i Brak klucza spowoduje błąd.  
* Do odszyfrowywania:  
  
  * Mechanizm rozpoznawania klucza jest wywoływana, jeśli określono, aby pobrać klucz. Jeśli program rozpoznawania nazw jest określony, ale nie ma mapowania dla identyfikatora klucza, zostanie zgłoszony błąd.  
  * Jeśli nie określono programu rozpoznawania nazw, ale nie określono klucza, klucz zostanie użyty, jeśli jego identyfikator odpowiada wymagany identyfikator klucza. Jeśli identyfikator nie jest zgodne, zostanie zgłoszony błąd.  
    
    [Przykłady szyfrowania](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>pokazują scenariusza bardziej szczegółowe end-to-end dla obiektów blob, kolejek i tabel, wraz z Integracja magazynu kluczy.

### <a name="requireencryption-mode"></a>Tryb RequireEncryption
Użytkownicy mogą opcjonalnie włączyć tryb działania, w której wszystkie przekazywania i pobierania muszą być szyfrowane. W tym trybie próby przekazania danych bez zasady szyfrowania lub pobrać dane, które nie są szyfrowane w usłudze zakończy się niepowodzeniem na kliencie. **RequireEncryption** flagi obiektu opcje żądania kontroluje to zachowanie. Jeśli aplikacja będzie szyfrowania wszystkich obiektów przechowywanych w usłudze Azure Storage, a następnie można ustawić **requireEncryption** właściwości domyślnych opcji żądania dla obiekt klienta usługi.   

Na przykład użyć **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** umożliwia Wymaganie szyfrowania dla wszystkich operacji wykonywanych za pośrednictwem tego obiektu klienta obiektu blob.

### <a name="blob-service-encryption"></a>Szyfrowanie usługi blob
Utwórz **BlobEncryptionPolicy** obiektu i ustaw go w opcji żądania (dla interfejsu API lub na poziomie klienta za pomocą **DefaultRequestOptions**). Wszystkie inne będzie obsługiwany przez bibliotekę klienta wewnętrznie.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set the encryption policy on the request options.
BlobRequestOptions options = new BlobRequestOptions();
options.setEncryptionPolicy(policy);

// Upload the encrypted contents to the blob.
blob.upload(stream, size, null, options, null);

// Download and decrypt the encrypted contents from the blob.
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
blob.download(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Szyfrowanie usługi kolejki
Utwórz **QueueEncryptionPolicy** obiektu i ustaw go w opcji żądania (dla interfejsu API lub na poziomie klienta za pomocą **DefaultRequestOptions**). Wszystkie inne będzie obsługiwany przez bibliotekę klienta wewnętrznie.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

// Add message
QueueRequestOptions options = new QueueRequestOptions();
options.setEncryptionPolicy(policy);

queue.addMessage(message, 0, 0, options, null);

// Retrieve message
CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);
```

### <a name="table-service-encryption"></a>Szyfrowanie usługi tabel
Oprócz tworzenia zasady szyfrowania i ustawienie jej na żądanie opcji, należy określić **EncryptionResolver** w **TableRequestOptions**, lub ustaw dla atrybutu [Szyfruj] jednostki metody pobierającej i ustawiającej.

### <a name="using-the-resolver"></a>Za pomocą programu rozpoznawania nazw

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

TableRequestOptions options = new TableRequestOptions()
options.setEncryptionPolicy(policy);
options.setEncryptionResolver(new EncryptionResolver() {
    public boolean encryptionResolver(String pk, String rk, String key) {
        if (key == "foo")
        {
            return true;
        }
        return false;
    }
});

// Insert Entity
currentTable.execute(TableOperation.insert(ent), options, null);

// Retrieve Entity
// No need to specify an encryption resolver for retrieve
TableRequestOptions retrieveOptions = new TableRequestOptions()
retrieveOptions.setEncryptionPolicy(policy);

TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
TableResult result = currentTable.execute(operation, retrieveOptions, null);
```

### <a name="using-attributes"></a>Przy użyciu atrybutów
Jak wspomniano powyżej, gdy jednostka implementuje TableEntity, następnie właściwości pobierającej i ustawiającej może korzystać z atrybutem [Szyfruj] zamiast określania **EncryptionResolver**.

```java
private string encryptedProperty1;

@Encrypt
public String getEncryptedProperty1 () {
    return this.encryptedProperty1;
}

@Encrypt
public void setEncryptedProperty1(final String encryptedProperty1) {
    this.encryptedProperty1 = encryptedProperty1;
}
```

## <a name="encryption-and-performance"></a>Szyfrowanie i wydajności
Należy pamiętać, że szyfrowania z magazynu danych spowoduje zmniejszenie wydajności. Musi zostać wygenerowany klucz zawartości i IV, muszą być szyfrowane samej zawartości i dodatkowe metadane muszą być sformatowane i przekazać. Ten narzut będą się różnić w zależności od ilości danych szyfrowany. Zaleca się, że klienci zawsze testują wydajności w czasie projektowania.

## <a name="next-steps"></a>Kolejne kroki
* Pobierz [biblioteki klienta usługi Azure Storage dla języka Java Maven pakietu](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
* Pobierz [biblioteki klienta magazynu Azure do kodu źródłowego języka Java z usługi GitHub](https://github.com/Azure/azure-storage-java)   
* Pobierz biblioteki Maven Azure klucza magazynu w przypadku Java Maven pakietów:
  * [Podstawowe](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) pakietu
  * [Klient](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) pakietu
* Odwiedź stronę [dokumentacji usługi Azure Key Vault](../../key-vault/key-vault-whatis.md)
