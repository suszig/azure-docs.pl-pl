 ---
title: importowanie środowiska Postman dla usługi Azure Media Services REST wywołuje opis: w tym temacie przedstawiono definicję środowiska Postman dla wywołania usługi Azure Media Services REST.
usług: documentationcenter usługi media services: "Autor: Menedżer Juliako: Edytor cfowler:"

MS.Service: ms.workload usługi media services: ms.tgt_pltfrm nośnika: na ms.devlang: na ms.topic: artykuł ms.date: ms.author 01/04/2017: juliako

---

# <a name="import-the-postman-environment"></a>Importuj środowiska Postman 

Ten artykuł zawiera definicję **Postman** zmiennych środowiskowych, które są używane [kolekcji Postman](postman-collection.md) zawierający grupowanych żądania HTTP, które wywoływania interfejsów API REST usługi Media. Pliki środowiska i kolekcji są używane przez [wymaga skonfigurowania Postman interfejsu API REST usługi Media](media-rest-apis-with-postman.md) samouczka.

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
