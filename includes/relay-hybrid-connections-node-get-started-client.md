### <a name="create-a-nodejs-application"></a>Tworzenie aplikacji w języku Node.js
* Utwórz plik JavaScript o nazwie `sender.js`.

### <a name="add-the-relay-npm-package"></a>Dodawanie pakietu NPM usługi Relay
* Uruchom polecenie `npm install hyco-ws` w wierszu polecenia języka Node w folderze projektu.

### <a name="write-some-code-to-send-messages"></a>Pisanie kodu w celu wysyłania komunikatów
1. Dodaj następujący element `constants` na początku pliku `sender.js`.
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. Dodaj następujący element usługi Relay `constants` do pliku `sender.js` na potrzeby szczegółów połączenia hybrydowego. Zastąp symbole zastępcze w nawiasach odpowiednimi wartościami uzyskanymi podczas tworzenia połączenia hybrydowego.
   
   1. `const ns` — przestrzeń nazw usługi Relay
   2. `const path` — nazwa połączenia hybrydowego
   3. `const keyrule` — nazwa klucza sygnatury dostępu współdzielonego
   4. `const key` — wartość klucza sygnatury dostępu współdzielonego
3. Dodaj następujący kod do pliku `sender.js`:
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    Tak powinien wyglądać plik listener.js:
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```



<!--HONumber=Nov16_HO2-->


