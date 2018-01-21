## <a name="create-a-ruby-application"></a>Tworzenie aplikacji Ruby
Aby uzyskać instrukcje, zobacz [tworzenie aplikacji Ruby na platformie Azure](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Skonfigurować aplikację do użycia usługi Service Bus
Aby korzystać z usługi Service Bus, pobranie i użycie pakiet Azure Ruby, który zawiera zestaw wygody bibliotek, które komunikują się z magazynu usługi REST.

### <a name="use-rubygems-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu RubyGems
1. Użyj interfejsu wiersza polecenia, takich jak **PowerShell** (system Windows), **terminali** (Mac), lub **Bash** (Unix).
2. Wpisz "azure gem instalacji" w oknie wiersza polecenia, aby zainstalować gem i zależności.

### <a name="import-the-package"></a>Importowanie pakietu
Za pomocą edytora tekstu, Dodaj następujący element do góry dopisków fonetycznych pliku, w którym mają być używane magazynu:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Skonfiguruj połączenie usługi Service Bus
Aby ustawić wartości przestrzeni nazw, nazwę klucza, klucz podpisujący i hosta, należy użyć poniższego kodu:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Ustaw wartość przestrzeni nazw na wartość, którą utworzono zamiast całej adresu URL. Na przykład użyć **"yourexamplenamespace"**, nie "yourexamplenamespace.servicebus.windows.net".
