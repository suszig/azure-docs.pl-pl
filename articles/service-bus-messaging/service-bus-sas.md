---
title: "Azure Service Bus uwierzytelniania za pomocą sygnatury dostępu współdzielonego | Dokumentacja firmy Microsoft"
description: "Omówienie uwierzytelniania magistrali usług przy użyciu sygnatury dostępu współdzielonego Przegląd, szczegółowe informacje dotyczące uwierzytelniania sygnatury dostępu Współdzielonego za pomocą usługi Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a2760072acb7c62204759f3ec0d3cb9899460f2d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>Uwierzytelnianie usługi Service Bus za pomocą sygnatury dostępu współdzielonego

*Sygnatury dostępu współdzielonego* (SAS) są mechanizmu podstawowego zabezpieczeń komunikatów usługi Service Bus. W tym artykule omówiono SAS, jak działają i sposobu ich używania w sposób niezależny od platformy.

Uwierzytelniania sygnatury dostępu Współdzielonego umożliwia aplikacjom uwierzytelnić się przy użyciu klawisza dostępu skonfigurowana w przestrzeni nazw lub komunikatów jednostki (kolejka lub temat), z którym są skojarzone określonych praw usługi Service Bus. Można następnie użyć tego klucza do wygenerowania tokenu sygnatury dostępu Współdzielonego, której klienci mogą z kolei używać do uwierzytelniania usługi Service Bus.

Obsługę uwierzytelniania sygnatury dostępu Współdzielonego znajduje się w zestawie SDK Azure w wersji 2.0 lub nowszej.

## <a name="overview-of-sas"></a>Omówienie SAS

Udostępniony sygnatur dostępu są mechanizmu uwierzytelniania na podstawie bezpiecznego wartości skrótu SHA-256 lub identyfikatorów URI. Jest bardzo zaawansowanym mechanizmem, który jest używany przez wszystkie usługi Service Bus. W rzeczywistości SAS ma dwa składniki: *udostępnionych zasad dostępu* i *udostępnionego sygnatury dostępu* (często nazywane *tokenu*).

Skojarzenia zabezpieczeń uwierzytelniania w usłudze Service Bus obejmuje konfigurację klucza kryptograficznego ze skojarzonymi prawami do zasobu usługi Service Bus. Klienci oświadczeń dostęp do zasobów usługi Service Bus, z uwzględnieniem tokenu sygnatury dostępu Współdzielonego. Token ten składa się z identyfikatora URI uzyskują dostęp do zasobu, a wygaśnięcia podpisany przy użyciu klucza skonfigurowany.

Można konfigurować reguły autoryzacji sygnatura dostępu współdzielonego usługi Service Bus [przekazuje](service-bus-fundamentals-hybrid-solutions.md#relays), [kolejek](service-bus-fundamentals-hybrid-solutions.md#queues), i [tematy](service-bus-fundamentals-hybrid-solutions.md#topics).

Uwierzytelniania sygnatury dostępu Współdzielonego przy użyciu następujących elementów:

* [Reguły autoryzacji dostępu do udostępnionych](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): A 256-bitowego szyfrowania klucza podstawowego w reprezentacji Base64, opcjonalny klucz pomocniczy i nazwę klucza i skojarzonymi prawami (kolekcja *nasłuchiwania*, *wysyłania*, lub *Zarządzaj* praw).
* [Udostępnione sygnatury dostępu](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) tokenu: wygenerowanych przy użyciu HMAC-SHA256 ciągu zasobu, składające się z identyfikatora URI zasobu, który jest dostępny i wygaśnięcia z klucza kryptograficznego. Podpis i inne elementy opisane w poniższych sekcjach są sformatowane jako ciąg do utworzenia tokenu sygnatury dostępu Współdzielonego.

## <a name="shared-access-policy"></a>Zasady dostępu współużytkowanego

Ważne jest, aby zrozumieć temat sygnatury dostępu Współdzielonego jest, że rozpoczyna się od zasad. Dla każdej zasady zdecydujesz się na trzy informacje: **nazwa**, **zakres**, i **uprawnienia**. **Nazwa** jest po prostu; unikatową nazwę w ramach tego zakresu. Zakres jest dość proste: jest identyfikator URI zasobu. W przypadku przestrzeni nazw usługi Service Bus zakres jest w pełni kwalifikowaną nazwę (FQDN), takich jak `https://<yournamespace>.servicebus.windows.net/`.

Przede wszystkim wyjaśnień uprawnienia dostępne dla zasad:

* Send
* Nasłuchiwanie
* Zarządzanie

Po utworzeniu zasad jest przypisany *klucza podstawowego* i *klucza pomocniczego*. Są to silną kryptograficznie kluczy. Nie utracone lub ich wyciek — zawsze będzie dostępna w [portalu Azure][Azure portal]. Możesz użyć dowolnej wygenerowane klucze i można je odtworzyć w dowolnym momencie. Jednak jeśli ponownie wygenerować lub zmienić klucza podstawowego w zasadach, żadnych sygnatur dostępu współdzielonego z niej utworzyć zostaną unieważnione.

Po utworzeniu przestrzeni nazw usługi Service Bus automatycznie utworzone zasady dla całej przestrzeni nazw o nazwie **RootManageSharedAccessKey**, a ta zasada ma wszystkie uprawnienia. Nie Zaloguj się jako **głównego**, dlatego nie używaj tej zasady, chyba że naprawdę powody. Możesz utworzyć dodatkowe zasady w **Konfiguruj** kartę dla przestrzeni nazw w portalu. Należy zauważyć, że poziomu drzewa pojedynczego w usłudze Service Bus (przestrzeń nazw, kolejki, itp.) może mieć tylko do 12 zasady do niego dołączony.

## <a name="configuration-for-shared-access-signature-authentication"></a>Konfiguracja uwierzytelniania sygnatura dostępu współdzielonego
Można skonfigurować [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) reguły w przestrzeni nazw usługi Service Bus, kolejki i tematy. Konfigurowanie [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) usługi Service Bus subskrypcji nie jest obecnie obsługiwane, ale zasady skonfigurowane w przestrzeni nazw lub temat umożliwia bezpieczny dostęp do subskrypcji. Dla przykładu pracy, która ilustruje tę procedurę, zobacz [uwierzytelniania przy użyciu dostępu sygnatury dostępu Współdzielonego z subskrypcjami magistrali usługi](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) próbki.

Maksymalnie 12 tych zasad można skonfigurować dla przestrzeni nazw usługi Service Bus, kolejka lub temat. Zasady, które są skonfigurowane w przestrzeni nazw usługi Service Bus dotyczą wszystkich jednostek w tej przestrzeni nazw.

![SYGNATURY DOSTĘPU WSPÓŁDZIELONEGO](./media/service-bus-sas/service-bus-namespace.png)

Na tym rysunku *manageRuleNS*, *sendRuleNS*, i *listenRuleNS* reguł autoryzacji dotyczą zarówno kolejka K1 i tematu T1, podczas gdy *listenRuleQ*  i *sendRuleQ* mają zastosowanie tylko do kolejki K1 i *sendRuleT* ma zastosowanie tylko do tematu T1.

Parametry klucza [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) są następujące:

| Parametr | Opis |
| --- | --- |
| *Nazwa klucza* |Ciąg opisujący reguły autoryzacji. |
| *PrimaryKey* |Algorytmem Base64 256-bitowego klucza podstawowego dla podpisywania i sprawdzania poprawności tokenu sygnatury dostępu Współdzielonego. |
| *Klucz pomocniczy* |Algorytmem Base64 256-bitowego klucza pomocniczego do podpisywania i sprawdzania poprawności tokenu sygnatury dostępu Współdzielonego. |
| *AccessRights* |Lista prawa dostępu przyznane przez regułę autoryzacji. Te prawa można dowolną kolekcję nasłuchiwania, wysyłania i Zarządzanie prawami. |

Po zainicjowaniu obsługi przestrzeni nazw usługi Service Bus, [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), z [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) ustawioną **RootManageSharedAccessKey**, domyślnie zostanie utworzona.

## <a name="generate-a-shared-access-signature-token"></a>Generowanie sygnaturą dostępu współdzielonego (token)

Same zasady nie jest tokenem dostępu dla usługi Service Bus. Jest obiekt, z której token dostępu jest generowany — przy użyciu zarówno klucz podstawowy lub pomocniczy. Każdy klient, który ma dostęp do kluczy podpisywania określona w regule autoryzacji dostępu współdzielonego, można wygenerować tokenu sygnatury dostępu Współdzielonego. Token jest generowany przez dokładnie obsługuje tworzenie ciąg w następującym formacie:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Gdzie `signature-string` jest skrótu SHA-256 zakresu tokenu (**zakres** zgodnie z opisem w poprzedniej sekcji) z znaku CRLF dołączany i czas wygaśnięcia (w sekundach od epoka: `00:00:00 UTC` na 1 stycznia 1970). 

> [!NOTE]
> Aby uniknąć czas wygaśnięcia tokenu krótkie, zaleca się kodowania wartość czasu wygaśnięcia jako liczbę całkowitą bez znaku przynajmniej 32-bitowej lub najlepiej całkowitą long (64-bitowe).  
> 
> 

Skrót wygląda podobnie do poniższego kodu pseudo i zwraca 32 bajtów.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Wartości nie mieszane znajdują się w **SharedAccessSignature** ciąg, dzięki czemu odbiorcy można obliczyć skrótu z takimi samymi parametrami, należy upewnić się, że zwracany jest ten sam rezultat. Identyfikator URI określa zakres, a nazwa klucza identyfikuje zasady, która ma być używany do obliczania skrótu. Jest to ważne z punktu widzenia zabezpieczeń. Jeśli podpis nie odpowiada, której obliczana adresata (usługi Service Bus), nastąpi odmowa dostępu. W tym momencie można się, że nadawca ma dostęp do klucza i powinny mieć przyznane prawa określonym w zasadach.

Należy pamiętać, że należy używać zakodowany identyfikator URI zasobu dla tej operacji. Identyfikator URI zasobu jest pełny identyfikator URI zasobu usługi Service Bus, do którego dostęp jest określona. Na przykład `http://<namespace>.servicebus.windows.net/<entityPath>` lub `sb://<namespace>.servicebus.windows.net/<entityPath>`, `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

Reguły autoryzacji dostępu współdzielonego, używany do podpisywania musi być skonfigurowany w jednostce określona przez ten identyfikator URI lub za pomocą jednej z jej nadrzędnych hierarchicznej. Na przykład `http://contoso.servicebus.windows.net/contosoTopics/T1` lub `http://contoso.servicebus.windows.net` w poprzednim przykładzie.

Token sygnatury dostępu Współdzielonego jest prawidłowa dla wszystkich zasobów w ramach `<resourceURI>` używane w `signature-string`.

[KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) w sygnatury dostępu Współdzielonego token odnosi się do **keyName** reguły autoryzacji dostępu współdzielonego służący do generowania tokenu.

*Adresu URL-zakodowane resourceURI* musi być taki sam jak identyfikator URI używany w ciągu na do logowania podczas obliczania sygnatury. Powinien być [procent, kodowane](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Zalecane jest okresowe ponowne generowanie kluczy używanych w [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) obiektu. Zwykle należy używać aplikacji [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) do generowania tokenu sygnatury dostępu Współdzielonego. Podczas ponownego generowania kluczy, należy zastąpić [klucz pomocniczy](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) z stary serwer podstawowy, a dla klucza wygenerowanie nowego klucza jako klucz podstawowy. Dzięki temu można kontynuować z użyciem tokenów do autoryzacji, które zostały wydane z starego klucza podstawowego, a które nie zostały jeszcze wygasnąć.

Jeśli klucz zostanie naruszony, należy Odwołaj klucze można odtworzyć zarówno [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) i [klucz pomocniczy](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) z [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), zastępując je za pomocą nowych kluczy. Ta procedura powoduje unieważnienie wszystkie tokeny podpisane przy użyciu starego klawiszy.

## <a name="how-to-use-shared-access-signature-authentication-with-service-bus"></a>Jak korzystać z usługi Service Bus uwierzytelniania sygnatura dostępu współdzielonego

Następujące scenariusze obejmują konfigurowanie reguł autoryzacji, generowanie tokeny sygnatury dostępu Współdzielonego i uwierzytelnianiem klienta.

Dla pełnej pracy przykładowej aplikacji usługi Service Bus, która ilustruje konfiguracji i używa autoryzacji sygnatury dostępu Współdzielonego, zobacz [sygnatura dostępu współdzielonego uwierzytelniania za pomocą usługi Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Powiązane próbki, która ilustruje korzystanie z reguł autoryzacji sygnatury dostępu Współdzielonego skonfigurowany dla przestrzeni nazw lub tematy, aby zabezpieczyć subskrypcje usługi Service Bus jest dostępnych tutaj: [uwierzytelniania przy użyciu dostępu sygnatury dostępu Współdzielonego z subskrypcjami magistrali usługi](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Reguły dostępu do udostępnionych autoryzacji dostępu na przestrzeni nazw

Operacje w głównej przestrzeni nazw usługi Service Bus wymagają uwierzytelniania certyfikatów. Należy przekazać certyfikatu zarządzania dla subskrypcji platformy Azure. Aby przekazać certyfikat zarządzania, wykonaj kroki [tutaj](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate)za pomocą [portalu Azure][Azure portal]. Aby uzyskać więcej informacji o certyfikatach zarządzania platformy Azure, zobacz [Omówienie certyfikatów Azure](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Punkt końcowy do uzyskiwania dostępu do reguł autoryzacji dostępu współdzielonego w przestrzeni nazw usługi Service Bus jest następujący:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

Aby utworzyć [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) obiektu w przestrzeni nazw usługi Service Bus, wykonaj operację POST na ten punkt końcowy z informacjami o reguły zserializowanym w formacie JSON i XML. Na przykład:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on the Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access the management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on the baseAddress above to create an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Podobnie użyj operacji pobrania w punkcie końcowym, aby odczytać reguł autoryzacji skonfigurowane w przestrzeni nazw.

Aby zaktualizować lub usunąć regułę autoryzacji określonych, użyj następującego punktu końcowego:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Reguły dostępu do udostępnionych autoryzacji dostępu na jednostkę

Dostęp można uzyskać [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) skonfigurowany dla kolejki usługi Service Bus lub temat za pośrednictwem obiektu [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) kolekcji w odpowiadającego [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) lub [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

Poniższy kod przedstawia sposób dodawania reguł autoryzacji dla kolejki.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Użyj autoryzacji sygnatura dostępu współdzielonego

Aplikacje z bibliotekami .NET magistrali usług przy użyciu zestawu .NET SDK usługi Azure mogą używać autoryzacji sygnatury dostępu Współdzielonego za pośrednictwem [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) klasy. Poniższy kod przedstawia użycie dostawcy tokenów do wysyłania wiadomości do kolejki usługi Service Bus.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Aplikacje umożliwia także SAS do uwierzytelniania przy użyciu ciągu połączenia SAS w metodach, które akceptują parametrów połączenia.

Należy pamiętać, że do autoryzacji sygnatury dostępu Współdzielonego za pomocą przekaźników usługi Service Bus, można użyć klawiszy SAS skonfigurowane w przestrzeni nazw usługi Service Bus. Jeśli utworzysz jawnie przekaźnik w przestrzeni nazw ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) z [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) obiektu, można ustawić tylko przekazywania tej zasady sygnatury dostępu Współdzielonego. Aby używać autoryzacji sygnatury dostępu Współdzielonego z subskrypcji usługi Service Bus, służy skonfigurowane na przestrzeni nazw usługi Service Bus lub na temat kluczy SAS.

## <a name="use-the-shared-access-signature-at-http-level"></a>Użyj sygnatura dostępu współdzielonego (na poziomie protokołu HTTP)

Teraz, gdy wiesz, jak utworzyć sygnatury dostępu współdzielonego dla dowolnego jednostek usługi Service Bus, możesz przystąpić do wykonania HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Należy pamiętać, że wszystko działa. Można utworzyć sygnatury dostępu Współdzielonego dla kolejki, tematu lub subskrypcji. 

Jeśli nadawcy lub klienta tokenu sygnatury dostępu Współdzielonego, nie ma klucza bezpośrednio, a ich nie można wycofać wyznaczania wartości skrótu, aby go uzyskać. W efekcie masz kontroli nad co mogą uzyskiwać dostęp do, jak długo. Ważne jest, aby pamiętać jest, że w przypadku zmiany klucza podstawowego w zasadach, żadnych sygnatur dostępu współdzielonego z niej utworzyć zostaną unieważnione.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Użyj sygnatura dostępu współdzielonego (na poziomie protokołu AMQP)

W poprzedniej sekcji pokazano, jak używać tokenu sygnatury dostępu Współdzielonego z żądaniem HTTP POST do wysyłania danych do usługi Service Bus. Wiesz, można uzyskać dostępu do usługi Service Bus przy użyciu zaawansowane komunikatów usługi kolejkowania protokołu (AMQP), który jest preferowanym protokołem do użycia ze względu na wydajność w wielu scenariuszach. Sposób użycia tokenu sygnatury dostępu Współdzielonego z protokołu AMQP jest opisany w dokumencie [AMQP Claim-Based zabezpieczeń wersja 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) będący w pracy projekt od 2013, ale dobrze obsługiwane przez platformę Azure dzisiaj.

Przed rozpoczęciem z wysyłania danych do usługi Service Bus, wydawcy musi wysłać tokenu sygnatury dostępu Współdzielonego wewnątrz komunikat protokołu AMQP dobrze zdefiniowany AMQP węzła o nazwie **$cbs** (można to sprawdzić jako kolejka "specjalne" używane przez usługę nabywanie i zweryfikować wszystkie skojarzenia zabezpieczeń tokeny). Należy określić wydawcę **ReplyTo** pola w komunikacie protokołu AMQP; jest to węzeł, w którym usługa odpowiedzi do wydawcy o rezultat weryfikacji tokenu (żądania/odpowiedzi prostego wzorca między wydawcy i usługi ). Ten węzeł odpowiedzi jest tworzona "na bieżąco," mówiąc o "dynamiczne tworzenie zdalnego węzła", zgodnie z opisem w specyfikacji protokołu AMQP 1.0. Po sprawdzeniu, czy token sygnatury dostępu Współdzielonego jest prawidłowa, wydawcy Przejdź dalej i uruchom, aby wysłać dane do usługi.

W poniższej procedurze pokazano sposób wysyłania tokenu sygnatury dostępu Współdzielonego przy użyciu protokołu AMQP [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) biblioteki. Jest to przydatne, jeśli nie możesz użyć oficjalnego zestawu SDK magistrali usług (na przykład na WinRT, .net Compact Framework, .net Micro Framework i Mono) opracowywanie w języku C\#. Oczywiście Ta biblioteka jest przydatne ułatwi zrozumienie sposobu opartego na oświadczeniach zabezpieczeń działa na poziomie protokołu AMQP, jak przedstawiono, jak to działa na poziomie protokołu HTTP (z żądania HTTP POST i tokenu sygnatury dostępu Współdzielonego wysyłany w nagłówku "Autoryzacji"). Jeśli nie ma potrzeby takich głębokie wiedzą na temat protokołu AMQP, możesz użyć oficjalnego zestawu SDK magistrali usług z platformą .net Framework aplikacji, co zrobić to za Ciebie.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

`PutCbsToken()` Metoda *połączenia* (wystąpienie klasy połączenia protokołu AMQP zgodnie z [AMQP .NET Lite biblioteki](https://github.com/Azure/amqpnetlite)) reprezentujący połączenie TCP do usługi i *sasToken* parametr, który jest sygnatury dostępu Współdzielonego token do wysłania. 

> [!NOTE]
> Należy pamiętać, że połączenie jest utworzone za pomocą **mechanizmu uwierzytelniania SASL ustawioną zewnętrzne** (i nie zwykły domyślna nazwa użytkownika i hasło używane, gdy nie jest konieczne do wysyłania tokenu sygnatury dostępu Współdzielonego).
> 
> 

Następnie wydawcy tworzy dwa łącza AMQP wysyłania tokenu sygnatury dostępu Współdzielonego i odbierania odpowiedzi (wynik weryfikacji tokenu) z usługi.

Komunikat protokołu AMQP zawiera zbiór właściwości oraz dodatkowe informacje od prostego komunikatu. SAS token jest treści wiadomości (przy użyciu jego konstruktora). **"ReplyTo"** właściwość jest ustawiona na nazwę węzła odbieranie wynik weryfikacji łącze odbiornika (można zmienić jego nazwę, a zostanie utworzony dynamicznie przez usługę). Ostatnie trzy właściwości aplikacji/niestandardowe są używane przez usługę do wskazuje, jaki rodzaj operacji musi wykonać. Zgodnie z opisem w specyfikacji wersji roboczej CBS, musi być **nazwy operacji** ("put-token"), **typu tokenu** (w tym przypadku "servicebus.windows.net:sastoken") i **"Nazwa" odbiorców** token dotyczy (jednostka całego).

Po wysłaniu tokenu sygnatury dostępu Współdzielonego łącze nadawcy, wydawcy musi odczytu odpowiedzi łącze odbiornika. Odpowiedź jest prosty komunikat protokołu AMQP z właściwością aplikacji o nazwie **"Kod stanu"** zawierających takie same wartości jak kod stanu HTTP.

## <a name="rights-required-for-service-bus-operations"></a>Uprawnienia wymagane dla operacji usługi Service Bus

W poniższej tabeli przedstawiono prawa dostępu wymagane przez różne operacje na zasobów usługi Service Bus.

| Operacja | Oświadczenia wymagane | Zakres oświadczeń |
| --- | --- | --- |
| **Namespace** | | |
| Konfiguruj reguły autoryzacji w przestrzeni nazw |Zarządzanie |Każdy adres przestrzeni nazw |
| **Rejestr usługi** | | |
| Wyliczanie zasad prywatnych |Zarządzanie |Każdy adres przestrzeni nazw |
| Rozpoczęcie nasłuchiwania na przestrzeni nazw |Nasłuchiwanie |Każdy adres przestrzeni nazw |
| Wysyłanie komunikatów do odbiornika w przestrzeni nazw |Send |Każdy adres przestrzeni nazw |
| **Kolejki** | | |
| Tworzenie kolejki |Zarządzanie |Każdy adres przestrzeni nazw |
| Usuwanie kolejki |Zarządzanie |Każdy adres prawidłową kolejką |
| Wyliczanie kolejek |Zarządzanie |Katalogu zasobów/kolejek |
| Pobierz opis kolejki |Zarządzanie |Każdy adres prawidłową kolejką |
| Konfigurowanie reguł autoryzacji dla kolejki |Zarządzanie |Każdy adres prawidłową kolejką |
| Wysyłanie do do kolejki |Send |Każdy adres prawidłową kolejką |
| Odbieranie komunikatów z kolejki |Nasłuchiwanie |Każdy adres prawidłową kolejką |
| Porzucenia lub pełne wiadomości po otrzymaniu komunikatu w trybie blokady podglądu |Nasłuchiwanie |Każdy adres prawidłową kolejką |
| Odroczenie komunikat dla nowszej pobierania |Nasłuchiwanie |Każdy adres prawidłową kolejką |
| Wiadomości utraconych wiadomości |Nasłuchiwanie |Każdy adres prawidłową kolejką |
| Pobierz stan skojarzony z sesją kolejki komunikatów |Nasłuchiwanie |Każdy adres prawidłową kolejką |
| Ustaw stan skojarzony z sesją kolejki komunikatów |Nasłuchiwanie |Każdy adres prawidłową kolejką |
| **Temat** | | |
| Tworzenie tematu |Zarządzanie |Każdy adres przestrzeni nazw |
| Usunięcie tematu |Zarządzanie |Każdy adres nieprawidłowy temat |
| Wyliczanie — tematy |Zarządzanie |Katalogu zasobów/tematy |
| Opis tematu pobierania |Zarządzanie |Każdy adres nieprawidłowy temat |
| Konfigurowanie reguł autoryzacji dla tematu |Zarządzanie |Każdy adres nieprawidłowy temat |
| Wyślij do tematu |Send |Każdy adres nieprawidłowy temat |
| **Subskrypcja** | | |
| Tworzenie subskrypcji |Zarządzanie |Każdy adres przestrzeni nazw |
| Usuń subskrypcję |Zarządzanie |.. /myTopic/Subscriptions/mySubscription |
| Wyliczanie subskrypcji |Zarządzanie |.. / myTopic/subskrypcji |
| Pobrać opisu subskrypcji |Zarządzanie |.. /myTopic/Subscriptions/mySubscription |
| Porzucenia lub pełne wiadomości po otrzymaniu komunikatu w trybie blokady podglądu |Nasłuchiwanie |.. /myTopic/Subscriptions/mySubscription |
| Odroczenie komunikat dla nowszej pobierania |Nasłuchiwanie |.. /myTopic/Subscriptions/mySubscription |
| Wiadomości utraconych wiadomości |Nasłuchiwanie |.. /myTopic/Subscriptions/mySubscription |
| Pobierz stan skojarzony z sesją tematu |Nasłuchiwanie |.. /myTopic/Subscriptions/mySubscription |
| Ustaw stan skojarzony z sesją tematu |Nasłuchiwanie |.. /myTopic/Subscriptions/mySubscription |
| **Reguły** | | |
| Utwórz regułę |Zarządzanie |.. /myTopic/Subscriptions/mySubscription |
| Usuwanie reguły |Zarządzanie |.. /myTopic/Subscriptions/mySubscription |
| Wyliczanie zasad |Zarządzanie lub nasłuchiwania |.. /myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat obsługi komunikatów usługi Service Bus, zobacz następujące tematy.

* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Jak używać kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com