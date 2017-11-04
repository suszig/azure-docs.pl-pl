## <a name="typical-output"></a>Najczęściej pojawiające się dane wyjściowe

Poniższy przykład przedstawia dane wyjściowe zapisywane w pliku dziennika przez próbkę Hello World. Dane wyjściowe są formatowane w celu uzyskania czytelności:

```json
[{
    "time": "Mon Apr 11 13:42:50 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:42:50 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:42:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:43:00 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:45:00 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Fragmenty kodu

W tej sekcji omówiono niektóre kluczowe części kodu znajdujące się w próbce hello\_world.

### <a name="iot-edge-gateway-creation"></a>Tworzenie bramy krawędzi IoT

Aby utworzyć bramę, zaimplementuj *procesu bramy*. Ten program tworzy wewnętrznej infrastruktury (broker), ładuje moduły krawędzi IoT i konfiguruje procesu bramy. Usługa IoT Edge zawiera funkcję **Gateway\_Create\_From\_JSON**, aby umożliwić uruchomienie bramy z pliku JSON. Aby użyć **bramy\_Utwórz\_z\_JSON** działać i przekaż go ścieżka do pliku JSON, który określa moduły krawędzi IoT, aby załadować.

Kod można znaleźć w procesie bramy w *Hello World* przykładowa w [main.c] [ lnk-main-c] pliku. Poniższy fragment kodu zawiera, w celu uzyskania czytelności, skróconą wersję kodu procesu bramy. Zanim ten przykładowy program zniszczy bramę, tworzy bramę, a następnie czeka na zatwierdzenie użytkownika przez naciśnięcie klawisza **ENTER**.

```C
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Plik ustawień JSON zawiera listę modułów krawędzi IoT załadować i linki między modułów. Każdy moduł krawędzi IoT musi określać a:

* **name**: unikatowa nazwa modułu.
* **loader**: moduł ładujący będący w stanie załadować odpowiedni moduł. Moduły ładujące to punkt rozszerzenia służący do ładowania różnych typów modułów. Krawędź IoT udostępnia ładowarki do użycia z modułami napisane w macierzystym C, Node.js, Java i .NET. Przykładowe Hello World tylko wykorzystuje natywnego modułu ładującego C, ponieważ wszystkie moduły, w tym przykładzie są dynamiczne bibliotek napisana C. Aby uzyskać więcej informacji na temat używania modułów krawędzi IoT napisane w różnych językach, zobacz [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), lub [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) próbek.
    * **Nazwa**: Nazwa modułu ładującego można załadować modułu.
    * **entrypoint**: ścieżka do biblioteki zawierającej moduł. W systemie Linux Ta biblioteka jest plikiem .so, w systemie Windows Ta biblioteka jest plikiem dll. Punkt wejścia jest specyficzny dla typu używanego modułu ładującego. Punkt wejścia modułu ładującego Node.js jest plikiem js. Punkt wejścia modułu ładującego Java to ścieżka i nazwa klasy. Punkt wejścia modułu ładującego .NET jest nazwa zestawu i nazwa klasy.

* **args**: wszystkie informacje o konfiguracji wymaganej przez moduł.

Poniższy kod przedstawia JSON użyć do zadeklarowania krawędzi IoT modułów przykładowej Hello World w systemie Linux. To, czy moduł wymaga jakichś argumentów, zależy od projektu modułu. W tym przykładzie moduł rejestratora przyjmuje argument, który jest ścieżką do pliku wyjściowego, natomiast moduł hello\_world nie ma żadnych argumentów.

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

Plik JSON zawiera również linki między modułami, które są przekazywane do brokera. Link ma dwie właściwości:

* **źródło**: Nazwa modułu, z `modules` sekcji lub `\*`.
* **sink**: nazwa modułu z sekcji `modules`.

Każdy link definiuje trasę i kierunek komunikatów. Komunikaty z **źródła** modułu są dostarczane do **zbiornika** modułu. Można ustawić **źródła** moduł `\*`, co oznacza, że **zbiornika** modułu odbiera komunikaty z dowolnego modułu.

Następujący kod pokazuje konfigurowanie połączeń między modułami użytymi w przykładzie hello\_world w systemie Linux za pomocą pliku JSON. Każdy komunikat generowany przez moduł `hello_world` jest używany przez moduł `logger`.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Publikowanie komunikatów w module hello\_world

Kod używany przez moduł hello\_world do publikacji komunikatów można znaleźć w pliku [„hello_world.c”][lnk-helloworld-c]. Poniższy fragment kodu pokazuje jego zmienioną wersję wraz z dodanymi komentarzami. Niektóre elementy kodu obsługujące błędy zostały usunięte, aby uzyskać jego lepszą czytelność:

```C
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

Witaj\_moduł world nigdy nie przetwarza wiadomości, które inne moduły krawędzi IoT publikowane brokera. Z tego powodu implementacja zwrotnego komunikatu w module hello\_world jest funkcją bez operacji.

```C
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-processing"></a>Przetwarzanie komunikatów modułu rejestratora

Moduł rejestratora odbiera komunikaty z brokera i zapisuje je w pliku. Nigdy nie publikuje żadnych komunikatów. W związku z tym kod modułu rejestratora nigdy nie wywołuje funkcji **Broker_Publish**.

**Logger_Receive** działać w [logger.c] [ lnk-logger-c] pliku jest wywołaniem zwrotnym brokera wywołuje na dostarczenie wiadomości do modułu rejestratora. Poniższy fragment kodu pokazuje zmienioną wersję wraz z dodanymi komentarzami. Niektóre elementy kodu obsługujące błędy zostały usunięte, aby uzyskać jego lepszą czytelność:

```C
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Następne kroki

W tym artykule został uruchomiony proste brama brzegowa IoT, który zapisuje komunikaty do pliku dziennika. Aby uruchomić przykład, który wysyła wiadomości do Centrum IoT, zobacz:

- [Krawędź IoT — wysyłanie wiadomości urządzenia do chmury z symulowane urządzenie przy użyciu systemu Linux][lnk-gateway-simulated-linux] 
- [Krawędź IoT — wysyłanie wiadomości urządzenia do chmury z symulowane urządzenie przy użyciu systemu Windows][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md