Emulator magazynu obsługuje jednego stałego konta i klucz uwierzytelniania dobrze znanego uwierzytelniania klucza wspólnego. Tego konta i klucz są dozwolone w emulatorze magazynu tylko poświadczenia klucza wspólnego. Są to:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Klucz uwierzytelniania obsługiwanych przez emulator magazynu jest przeznaczony tylko do celów testowych funkcje kodu uwierzytelniania klienta. Nie ma żadnych celów zabezpieczeń. Nie można użyć konta magazynu w środowisku produkcyjnym i klucz w emulatorze magazynu. Nie należy używać konta programowanie z danymi produkcyjnymi.
> 
> Emulator magazynu obsługuje tylko połączenia za pośrednictwem protokołu HTTP. Jednak HTTPS to protokół zalecany do uzyskiwania dostępu do zasobów w sieci produkcyjnej kontem magazynu platformy Azure.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Połącz się kontem emulatora przy użyciu skrótu
Najprostszym sposobem nawiązać emulator magazynu z poziomu aplikacji jest skonfigurowanie parametrów połączenia w pliku konfiguracji aplikacji, która odwołuje się skrót `UseDevelopmentStorage=true`. Oto przykład parametrów połączenia do emulatora magazynu w *app.config* pliku: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Połącz się kontem emulatora, korzystając z dobrze znaną nazwę konta i klucza
Aby utworzyć parametry połączenia, które odwołuje się do emulatora nazwę konta i klucz, należy określić punkty końcowe dla każdej usługi, które mają być używane z emulatora w parametrach połączenia. Jest to konieczne, tak aby parametry połączenia będzie odwoływać się do emulatora punktów końcowych, które są inne niż konto magazynu w środowisku produkcyjnym. Na przykład wartość parametrów połączenia będzie wyglądać następująco:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Ta wartość jest taka sama jak pokazano powyżej, skrót `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Określ serwer proxy HTTP
Można również określić serwer proxy HTTP do użycia podczas testowania usługi przed emulatora magazynu. Może to być przydatne do obserwacji żądań i odpowiedzi HTTP podczas debugowania kodu operacji względem usługi magazynu. Aby określić serwer proxy, Dodaj `DevelopmentStorageProxyUri` opcji w parametrach połączenia, a następnie ustaw dla niego wartość identyfikatora URI serwera proxy. Na przykład poniżej przedstawiono parametry połączenia, które wskazuje emulator magazynu i konfiguruje serwer proxy HTTP:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

