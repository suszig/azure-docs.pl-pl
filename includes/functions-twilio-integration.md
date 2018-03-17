W tym przykładzie polega na użyciu [usługi Twilio](https://www.twilio.com/) usługi do wysyłania wiadomości SMS na telefon komórkowy. Środowisko Azure Functions ma już obsługę usługi Twilio za pośrednictwem [powiązania usługi Twilio](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), a próbki korzysta z tej funkcji.

Najpierw potrzebne jest konto usługi Twilio. Możesz utworzyć bezpłatne w https://www.twilio.com/try-twilio. Po utworzeniu konta Dodaj następujące trzy **ustawień aplikacji** do funkcji aplikacji.

| Nazwa ustawienia aplikacji | Opis wartości |
| - | - |
| **TwilioAccountSid**  | Identyfikator SID dla konta usługi Twilio |
| **TwilioAuthToken**   | Token uwierzytelniania dla konta usługi Twilio |
| **TwilioPhoneNumber** | Numer telefonu skojarzony z Twoim kontem usługi Twilio. To jest używany do wysyłania wiadomości SMS. |