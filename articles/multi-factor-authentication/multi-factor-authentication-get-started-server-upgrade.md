<properties 
    pageTitle="Uaktualnianie agenta PhoneFactor do serwera Azure Multi-Factor Authentication" 
    description="W tym dokumencie opisano sposób rozpoczęcia pracy z serwerem Azure MFA oraz uaktualnienia ze starszej wersji agenta PhoneFactor." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# Uaktualnianie agenta PhoneFactor do serwera Azure Multi-Factor Authentication

Uaktualnianie z agenta PhoneFactor (v5.x lub starszej wersji) do serwera Azure Multi-Factor Authentication wymaga odinstalowania agenta PhoneFactor i powiązanych z nim składników przed rozpoczęciem instalowania serwera Multi-Factor Authentication i powiązanych z nim składników. 

## Aby uaktualnić agenta PhoneFactor do serwera Azure Multi-Factor Authentication
<ol>
<li>Najpierw należy utworzyć kopię zapasową pliku danych PhoneFactor. Domyślna lokalizacja instalacji to C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Jeśli zainstalowano portal użytkowników:</li>
<ol>
<li>Przejdź do folderu instalacji i utwórz kopię zapasową pliku web.config. Domyślna lokalizacja instalacji to C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Jeśli do portalu dodano niestandardowe kompozycje, należy wykonać kopię zapasową niestandardowego folderu poniżej katalogu C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Odinstaluj portal użytkowników za pośrednictwem agenta PhoneFactor (opcja dostępna tylko wtedy, gdy jest zainstalowany na tym samym serwerze co agent PhoneFactor) lub za pomocą modułu Programy i funkcje systemu Windows.</li></ol>




<li>Jeśli zainstalowana jest usługa sieci Web aplikacji mobilnej:
<ol>
<li>Przejdź do folderu instalacyjnego i utwórz kopię zapasową pliku web.config. Domyślna lokalizacja instalacji to C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Odinstaluj usługę sieci Web aplikacji mobilnej za pośrednictwem modułu Programy i funkcje systemu Windows.</li></ol>

<li>Jeśli zainstalowano zestaw SDK usługi sieci Web, należy go odinstalować za pomocą agenta PhoneFactor lub za pośrednictwem modułu Programy i funkcje systemu Windows.

<li>Odinstaluj agenta PhoneFactor za pośrednictwem modułu Programy i funkcje systemu Windows.

<li>Zainstaluj serwer Multi-Factor Authentication. Należy zauważyć, że ścieżka instalacji jest pobierana z rejestru z poprzedniej instalacji agenta PhoneFactor, dlatego instalacji należy dokonać w tej samej lokalizacji (np. C:\Program Files\PhoneFactor). Nowe instalacje będą miały różne domyślne ścieżki instalacyjne (np. C:\Program Files\Multi-Factor Authentication Server). Plik danych pozostawiony przez poprzedniego agenta PhoneFactor należy uaktualnić podczas instalacji, aby po zainstalowaniu nowego serwera Multi-Factor Authentication określeni wcześniej użytkownicy i ustawienia nadal były dostępne.

<li>Jeśli zostanie wyświetlony monit, uaktywnij serwer Multi-Factor Authentication i upewnij się, że jest przypisany do odpowiedniej grupy replikacji.

<li>Jeśli wcześniej zainstalowano zestaw SDK usługi sieci Web, należy zainstalować nowy zestaw SDK usługi sieci Web za pomocą interfejsu użytkownika serwera Multi-Factor Authentication. Należy zauważyć, że domyślną nazwą katalogu wirtualnego jest teraz „MultiFactorAuthWebServiceSdk”, a nie „PhoneFactorWebServiceSdk”. Jeśli chcesz użyć poprzedniej nazwy, musisz podczas instalacji zmienić nazwę katalogu wirtualnego. W przeciwnym razie, jeśli zezwolisz procesowi instalacji na użycie nowej nazwy domyślnej, trzeba będzie zmienić adres URL w każdej aplikacji, która odwołuje się do zestawu SDK usługi sieci Web, takiej jak portal użytkowników i usługa sieci Web aplikacji mobilnej, aby wskazywała na poprawną lokalizację.

<li>Jeśli wcześniej zainstalowano portal użytkowników na tym serwerze agenta PhoneFactor, zainstaluj nowy portal użytkowników usługi Multi-Factor Authentication za pomocą interfejsu użytkownika serwera Multi-Factor Authentication. Należy zauważyć, że domyślną nazwą katalogu wirtualnego jest teraz „MultiFactorAuth”, a nie „PhoneFactor”. Jeśli chcesz użyć poprzedniej nazwy, musisz podczas instalacji zmienić nazwę katalogu wirtualnego. W przeciwnym razie, jeśli zezwolisz procesowi instalacji na użycie nowej nazwy domyślnej, musisz kliknąć ikonę Portal użytkowników na serwerze Multi-Factor Authentication i zaktualizować adres URL portalu użytkowników na karcie Ustawienia. 

<li>Jeśli portal użytkowników i/lub usługa sieci Web aplikacji mobilnej zostały wcześniej zainstalowane na innym serwerze z agenta PhoneFactor:
<ol>
<li>Przejdź do lokalizacji instalacji (np. C:\Program Files\PhoneFactor) i skopiuj odpowiednie pliki instalacyjne na inny serwer. Zarówno dla portalu użytkowników, jak i dla usługi sieci Web aplikacji mobilnej istnieją instalatory w wersji 32-bitowej i 64-bitowej. Mają nazwy odpowiednio MultiFactorAuthenticationUserPortalSetupXX.msi i MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.</li>
<li>Aby zainstalować portal użytkowników na serwerze sieci Web, otwórz wiersz polecenia jako administrator i uruchom instalatora MultiFactorAuthenticationUserPortalSetupXX.msi. Należy zauważyć, że domyślną nazwą katalogu wirtualnego jest teraz „MultiFactorAuth”, a nie „PhoneFactor”. Jeśli chcesz użyć poprzedniej nazwy, musisz podczas instalacji zmienić nazwę katalogu wirtualnego. W przeciwnym razie, jeśli zezwolisz procesowi instalacji na użycie nowej nazwy domyślnej, musisz kliknąć ikonę Portal użytkowników na serwerze Multi-Factor Authentication i zaktualizować adres URL portalu użytkowników na karcie Ustawienia. Trzeba będzie poinformować istniejących użytkowników o nowym adresie URL.</li>
<li>Przejdź do lokalizacji instalacji portalu użytkowników (np. C:\inetpub\wwwroot\MultiFactorAuth) i otwórz do edycji plik web.config. Z oryginalnego pliku web.config, który został umieszczony w kopii zapasowej przed uaktualnieniem, skopiuj wartości z sekcji appSettings i applicationSettings do nowego pliku web.config. Jeśli nowa domyślna nazwa katalogu wirtualnego została zachowana podczas instalacji zestawu SDK usługi sieci Web, zmień adres URL w sekcji applicationSettings, aby wskazywał poprawną lokalizację. Jeśli w poprzednim pliku web.config zostały zmienione inne wartości domyślne, zastosuj te same zmiany w nowym pliku web.config.</li>
<li>Aby zainstalować usługę sieci Web aplikacji mobilnej na serwerze sieci Web, otwórz wiersz polecenia jako administrator i uruchom instalatora MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Należy zauważyć, że domyślną nazwą katalogu wirtualnego jest teraz „MultiFactorAuthMobileAppWebService”, a nie „PhoneFactorPhoneAppWebService”. Jeśli chcesz użyć poprzedniej nazwy, musisz podczas instalacji zmienić nazwę katalogu wirtualnego. Można wybrać krótszą nazwę, aby ułatwić użytkownikom końcowym wpisywanie jej w urządzeniach przenośnych. W przeciwnym razie, jeśli zezwolisz procesowi instalacji na użycie nowej nazwy domyślnej, kliknij ikonę aplikację mobilnej na serwerze Multi-Factor Authentication i zaktualizuj adres URL usługi sieci Web aplikacji mobilnej.</li>
<li>Przejdź do lokalizacji instalacji usługi sieci Web aplikacji mobilnej (np. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) i otwórz do edycji plik web.config. Z oryginalnego pliku web.config, który został umieszczony w kopii zapasowej przed uaktualnieniem, skopiuj wartości z sekcji appSettings i applicationSettings do nowego pliku web.config. Jeśli nowa domyślna nazwa katalogu wirtualnego została zachowana podczas instalacji zestawu SDK usługi sieci Web, zmień adres URL w sekcji applicationSettings, aby wskazywał poprawną lokalizację. Jeśli w poprzednim pliku web.config zostały zmienione inne wartości domyślne, zastosuj te same zmiany w nowym pliku web.config.</li></ol>


 


 


<!--HONumber=jun16_HO2-->


