---
title: "Omówienie szablonu licencji Widevine | Dokumentacja firmy Microsoft"
description: "Ten temat zawiera omówienie szablonu licencji Widevine, która umożliwia skonfigurowanie licencji Widevine."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 667ff16dc7608dab2a5b8b1fd7df715da4620ca1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="widevine-license-template-overview"></a>Omówienie szablonu licencji Widevine
## <a name="overview"></a>Omówienie
Usługa Azure Media Services umożliwia teraz Konfigurowanie i żądania licencji Widevine. Gdy użytkownik końcowy próbą odtwarzanie zawartości Widevine chronione, żądanie jest wysyłane do usługi dostarczania licencji uzyskanie licencji. Jeśli usługa licencji zatwierdza żądanie, wystawia licencji, które są wysyłane do klienta i może służyć do odszyfrowania i odtwarzać zawartość określonego.

Żądania licencji Widevine jest w formacie JSON wiadomości.  

>[!NOTE]
> Możesz utworzyć pusty bez wartości po prostu "{}" i zostanie utworzony szablon licencji z wszystkimi wartościami domyślnymi. Ustawienie domyślne działa w większości przypadków. Na przykład w MS na podstawie licencji dostarczania scenariusze, które powinny być zawsze domyślnie. Należy ustawić wartości "dostawca" i "content_id", musi być zgodna firmy Google Widevine poświadczeń przez dostawcę.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>Komunikat JSON
| Nazwa | Wartość | Opis |
| --- | --- | --- |
| ładunek |Ciąg kodowany jako Base64 |Żądanie licencji wysłane przez klienta. |
| content_id |Ciąg kodowany jako Base64 |Identyfikator używany do uzyskania dla każdego content_key_specs.track_type KeyId(s) i zawartości kluczy. |
| Dostawcy |Ciąg |Umożliwia wyszukiwanie zasad i zawartości kluczy. Jeśli MS klucza dostawy jest używane w celu dostarczania licencji Widevine, ten parametr zostanie zignorowany. |
| nazwa_zasady |Ciąg |Nazwa zasady wcześniej zarejestrowane. Optional (Opcjonalność) |
| allowed_track_types |wyliczenia |SD_ONLY lub SD_HD. Formanty, które zawartości kluczy powinien być uwzględniany w licencji |
| content_key_specs |Tablica JSON struktur, zobacz **specyfikacji klucz zawartości** poniżej |Skuteczniejszą kontrolę na zawartość klucze do zwrócenia. Aby uzyskać więcej informacji, zobacz zawartości specyfikacja klucza poniżej.  Można określić tylko jeden allowed_track_types i content_key_specs. |
| use_policy_overrides_exclusively |Wartość logiczna. wartość PRAWDA lub FAŁSZ |Użyj zasad atrybuty określone w policy_overrides i pominąć wszystkie zapisane wcześniej zasady. |
| policy_overrides |Struktura JSON, zobacz **zasady zastępują** poniżej |Ustawienia zasad dla tej licencji.  W przypadku tego zasobu ma wstępnie zdefiniowane zasady, te określone wartości będą używane. |
| session_init |Struktura JSON, zobacz **inicjowania sesji** poniżej |Opcjonalne dane przekazywane do licencji. |
| parse_only |Wartość logiczna. wartość PRAWDA lub FAŁSZ |Żądanie licencji jest analizowana, ale żadna licencja jest wystawiony. Jednak wartości formularza żądania licencji są zwracane w odpowiedzi. |

## <a name="content-key-specs"></a>Dane techniczne klucz zawartości
Jeśli istnieje już istniejących zasad, jest niepotrzebna, aby określić jedną z wartości w zawartości specyfikacja klucza.  Istniejące zasady skojarzone z tą zawartością będzie służyć do określenia ochrony danych wyjściowych, takich jak HDCP i CGMS.  Jeśli wcześniej istniejących zasad nie jest zarejestrowany na serwerze licencji Widevine, dostawcy zawartości można wprowadzić wartości do żądania licencji.   

Każdy content_key_specs należy określić dla wszystkich ścieżek, niezależnie od use_policy_overrides_exclusively opcji. 

| Nazwa | Wartość | Opis |
| --- | --- | --- |
| content_key_specs. track_type |Ciąg |Nazwa typu ścieżki. Jeśli content_key_specs jest określony w żądaniu licencji, upewnij się śledzić wszystkie typy jawnie określić. Błąd w tym celu spowoduje niepowodzenie do odtwarzania ostatnich 10 sekund. |
| content_key_specs  <br/> security_level |UInt32 |Definiuje wymagania dotyczące niezawodności klienta dla odtwarzania. <br/> 1 - opartych na oprogramowaniu whitebox kryptograficznego jest wymagana. <br/> 2 - kryptograficznego oprogramowania i zaciemnionego dekodera jest wymagana. <br/> 3 operacji kryptograficznych i materiału klucza muszą być wykonywane w środowisku kopii zaufanego wykonywania sprzętu. <br/> 4 - szyfrowania i dekodowania zawartości muszą być wykonywane w środowisku kopii zaufanego wykonywania sprzętu.  <br/> 5 - kryptograficznego, dekodowania i wszystkie obsługi nośnika (skompresowanym i nieskompresowanym) musi obsługiwane w środowisku kopii zaufanego wykonywania sprzętu. |
| content_key_specs <br/> required_output_protection.hdc |String — jeden z: HDCP_V2 HDCP_NONE, HDCP_V1, |Wskazuje, czy jest wymagane HDCP |
| content_key_specs <br/>key |Base64 <br/>ciąg kodowany jako |Klucz zawartości do użycia dla tej ścieżki. Jeśli zostanie określona, track_type lub key_id jest wymagany.  Ta opcja umożliwia dostawcy zawartości wprowadzić klucz zawartości dla tej ścieżki, zamiast czekać na serwer licencji Widevine Generowanie lub wyszukać klucz. |
| content_key_specs.key_id |Kodowany w formacie Base64 ciąg binarny, 16 bajtów |Unikatowy identyfikator dla klucza. |

## <a name="policy-overrides"></a>Zastąpienia zasad
| Nazwa | Wartość | Opis |
| --- | --- | --- |
| policy_overrides. can_play |Wartość logiczna. wartość PRAWDA lub FAŁSZ |Wskazuje, że odtwarzanie zawartości jest dozwolone. Domyślna to false. |
| policy_overrides. can_persist |Wartość logiczna. wartość PRAWDA lub FAŁSZ |Wskazuje, że licencja może utrwalone pamięci trwałej w trybie offline. Domyślna to false. |
| policy_overrides. can_renew |wartość logiczną PRAWDA lub FAŁSZ |Wskazuje, że odnowienia licencji jest dozwolone. Jeśli PRAWDA, czas trwania licencji można przedłużyć pulsu. Domyślna to false. |
| policy_overrides. license_duration_seconds |Int64 |Określa przedział czasu dla określonych licencji. Wartość 0 wskazuje, że nie ma żadnego limitu czasu trwania. Domyślna to 0. |
| policy_overrides. rental_duration_seconds |Int64 |Określa przedział czasu, podczas odtwarzania jest dozwolone. Wartość 0 wskazuje, że nie ma żadnego limitu czasu trwania. Domyślna to 0. |
| policy_overrides. playback_duration_seconds |Int64 |Wyświetlanie okna czasu po rozpoczęciu w czasie trwania licencji. Wartość 0 wskazuje, że nie ma żadnego limitu czasu trwania. Domyślna to 0. |
| policy_overrides. renewal_server_url |Ciąg |Wszystkie żądania pulsu (odnowienia) ta licencja jest skierowana do określonego adresu URL. To pole jest używane tylko, jeśli can_renew ma wartość true. |
| policy_overrides. renewal_delay_seconds |Int64 |Liczbę sekund po license_start_time, przed próbą najpierw odnawiania. To pole jest używane tylko, jeśli can_renew ma wartość true. Wartość domyślna to 0 |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Określa opóźnienie w sekundach między żądań odnowienia kolejnych licencji, w razie awarii. To pole jest używane tylko, jeśli can_renew ma wartość true. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |Okno czasu, w którym można kontynuować podczas odnawiania odtwarzania jest próba, jeszcze nie powiodło się z powodu problemów z wewnętrznej bazy danych z serwerem licencji. Wartość 0 wskazuje, że nie ma żadnego limitu czasu trwania. To pole jest używane tylko, jeśli can_renew ma wartość true. |
| policy_overrides. renew_with_usage |wartość logiczną PRAWDA lub FAŁSZ |Wskazuje, że licencja są przesyłane do odnowienia po uruchomieniu użycia. To pole jest używane tylko, jeśli can_renew ma wartość true. |

## <a name="session-initialization"></a>Inicjowanie sesji
| Nazwa | Wartość | Opis |
| --- | --- | --- |
| provider_session_token |Ciąg kodowany jako Base64 |Token sesji jest przekazany z powrotem do licencji i będą istnieć w kolejnych odnowienia.  Poza sesji nie zachowa tokenu sesji. |
| provider_client_token |Ciąg kodowany jako Base64 |Token klienta do wysłania w odpowiedzi licencji.  Jeśli żądanie licencji zawiera token klienta, ta wartość jest ignorowana. Token klienta będzie trwało poza sesji licencji. |
| override_provider_client_token |Wartość logiczna. wartość PRAWDA lub FAŁSZ |Jeśli wartość FAŁSZ i żądania licencji zawiera token klienta, użyj tokenu z żądania, nawet wtedy, gdy określono token klienta w tej struktury.  Jeśli PRAWDA, należy zawsze używać tokenu określone w tej struktury. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurowanie licencji Widevine przy użyciu typów .NET
Usługa Media Services udostępnia interfejsów API architektury .NET, które umożliwiają skonfigurowanie licencji Widevine. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klasy zgodnie z definicją w SDK .NET usługi Media Services
Poniżej przedstawiono definicje tych typów.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Przykład
Poniższy przykład pokazuje, jak skonfigurować proste licencji Widevine przy użyciu interfejsów API architektury .NET.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zobacz też
[Za pomocą PlayReady i Widevine dynamicznie Common Encryption](media-services-protect-with-drm.md)

