## <a name="typical-output"></a>Najczęściej pojawiające się dane wyjściowe
Poniżej znajduje się przykład danych wyjściowych zapisywanych w pliku dziennika w ramach próbki kodu Hello World. Znaki nowego wiersza i tabulatory zostały dodane, aby poprawić czytelność kodu:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Fragmenty kodu
W tej sekcji omówiono niektóre kluczowe fragmenty kodu znajdujące się w próbce Hello World.

### <a name="gateway-creation"></a>Tworzenie bramy
Deweloper musi utworzyć *proces bramy*. Ten program tworzy wewnętrzną infrastrukturę (brokera), ładuje moduły i konfiguruje wszystko tak, aby działo poprawnie. Zestaw SDK zawiera funkcję **Gateway_Create_From_JSON**, aby umożliwić uruchomienie bramy z pliku JSON. Aby użyć funkcji **Gateway_Create_From_JSON**, należy udostępnić ścieżkę pliku JSON, który określa moduły do załadowania. 

Kod procesu bramy w próbce Hello World można znaleźć w pliku [main.c][lnk-main-c]. Poniższy fragment kodu zawiera, dla czytelności, skróconą wersję kodu procesu bramy. Zanim program zniszczy bramę, tworzy bramę, a następnie czeka na zatwierdzenie użytkownika poprzez naciśnięcie klawisza **ENTER**. 

```
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

Plik ustawień JSON zawiera listę modułów do załadowania. W każdym module muszą być określone następujące elementy:

* **module_name**: unikatowa nazwa modułu.
* **module_path**: ścieżka do biblioteki zawierającej moduł. W systemie Linux jest to plik so, a w systemie Windows jest to plik dll.
* **args**: wszystkie informacje o konfiguracji wymaganej przez moduł.

Plik JSON zawiera również linki między modułami, które zostaną przekazane do brokera. Link ma dwie właściwości:

* **source**: nazwa modułu z sekcji `modules` lub „\*”.
* **sink**: nazwa modułu z sekcji `modules`.

Każdy link definiuje trasę i kierunek komunikatów. Komunikaty z modułu `source` będą dostarczane do modułu `sink`. Dla właściwości `source` można ustawić wartość „\*”, co oznacza, że komunikaty z wszystkich modułów będę odbierane przez moduł `sink`.

Poniższy przykład pokazuje plik ustawień JSON używany do konfigurowania próbki kodu Hello World w systemie Linux. Każdy komunikat generowany przez moduł `hello_world` będzie używany przez moduł `logger`. To, czy moduł wymaga argumentu, zależy od projektu modułu. W tym przykładzie moduł rejestratora przyjmuje argument, który jest ścieżką do pliku wyjściowego, natomiast moduł Hello World nie ma żadnych argumentów:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "loading args": {
              "module path" : "./modules/logger/liblogger_hl.so"
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "loading args": {
              "module path" : "./modules/hello_world/libhello_world_hl.so"
            },
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Publikowanie komunikatów w module Hello World
Kod używany przez moduł „hello world” do publikacji komunikatów można znaleźć w pliku [„hello_world.c”][lnk-helloworld-c]. Poniższy fragment kodu pokazuje jego zatwierdzoną wersję wraz z dodatkowymi komentarzami. Niektóre elementy kodu obsługujące błędy zostały usunięte, aby uzyskać jego lepszą czytelność:

```
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

### <a name="hello-world-module-message-processing"></a>Przetwarzanie komunikatów w module Hello World
Moduł Hello World nigdy nie musi przetwarzać komunikatów, które inne moduły publikują do brokera. To powoduje, że implementacja zwrotnego komunikatu w module Hello World jest funkcją bez operacji.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Publikowanie i przetwarzanie komunikatu przez moduł rejestratora
Moduł rejestratora odbiera komunikaty z brokera i zapisuje je w pliku. Nigdy nie publikuje żadnych komunikatów. W związku z tym kod modułu rejestratora nigdy nie wywołuje funkcji **Broker_Publish**.

Funkcja **Logger_Recieve** w pliku [logger.c][lnk-logger-c] jest wywołaniem zwrotnym, które broker wywołuje w celu dostarczenia komunikatu do modułu rejestratora. Poniższy fragment kodu pokazuje jego zatwierdzoną wersję wraz z dodatkowymi komentarzami. Niektóre elementy kodu obsługujące błędy zostały usunięte, aby uzyskać jego lepszą czytelność:

```
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
Aby dowiedzieć się więcej na temat używania zestawu SDK usługi IoT Gateway, zobacz następujące tematy:

* [IoT Gateway SDK – send device-to-cloud messages with a simulated device using Linux] (Zestaw SDK Bramy IoT — wysyłanie komunikatów z urządzenia do chmury przy użyciu symulowanego urządzenia z systemem Linux) [lnk-gateway-simulated].
* [Azure IoT Gateway SDK] (Zestaw SDK Bramy IoT platformy Azure) [lnk-gateway-sdk] w witrynie GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!--HONumber=Nov16_HO2-->


