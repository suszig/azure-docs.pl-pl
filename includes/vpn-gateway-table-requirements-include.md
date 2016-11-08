W poniższej tabeli wymieniono wymagania dla bram sieci VPN opartych na zasadach i na trasie. Ta tabela ma zastosowanie w obu modelach wdrażania — przy użyciu usługi Resource Manager i klasycznym. W przypadku modelu klasycznego bramy sieci VPN opartych na zasadach są takie same, jak bramy statyczne, a bramy oparte na trasach są takie same, jak bramy dynamiczne.

|  | **Podstawowa brama sieci VPN opartej na zasadach** | **Podstawowa brama sieci VPN opartej na trasach** | **Standardowa brama sieci VPN opartej na trasach** | **Brama sieci VPN na opartej na trasach o wysokiej wydajności** |
| --- | --- | --- | --- | --- |
| **Łączność typu „lokacja do lokacji” (Site-to-Site, S2S)** |Konfiguracja sieci VPN opartej na zasadach |Konfiguracja sieci VPN opartej na trasach |Konfiguracja sieci VPN opartej na trasach |Konfiguracja sieci VPN opartej na trasach |
| **Łączność typu „punkt do lokacji” (Point-to-Site, P2S**) |Nieobsługiwane |Obsługiwane (może współistnieć z S2S) |Obsługiwane (może współistnieć z S2S) |Obsługiwane (może współistnieć z S2S) |
| **Metoda uwierzytelniania** |Klucz wstępny |Klucz wstępny dla połączeń S2S, certyfikaty dla łączności P2S |Klucz wstępny dla połączeń S2S, certyfikaty dla łączności P2S |Klucz wstępny dla połączeń S2S, certyfikaty dla łączności P2S |
| **Maksymalna liczba połączeń S2S** |1 |10 |10 |30 |
| **Maksymalna liczba połączeń P2S** |Nieobsługiwane |128 |128 |128 |
| **Aktywna obsługa routingu (BGP)** |Nieobsługiwane |Nieobsługiwane |Nieobsługiwane |Nieobsługiwane |

<!--HONumber=Jun16_HO2-->


