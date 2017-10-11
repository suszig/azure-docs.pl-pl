<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Przygotowania do aktualizacji
Konieczne będzie przed skanowania i zastosować aktualizację, wykonaj następujące czynności:

1. Utwórz migawkę chmury danych urządzenia.
2. Upewnij się, że kontroler stałe adresy IP są rutowalne i można połączyć się z Internetem. Te stałe adresy IP będzie używany do obsługi aktualizacji na urządzeniu. Można to sprawdzić, uruchamiając następujące polecenie cmdlet na każdym kontrolerze z interfejsu programu Windows PowerShell urządzenia:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Przykładowe dane wyjściowe dla połączenia testowego, gdy stałe adresy IP można połączyć się z Internetem**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Po ręcznej kontrole wstępne została ukończona pomyślnie, można przejść do skanowania i zainstalować aktualizacje.

