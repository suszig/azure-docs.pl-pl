<properties
   pageTitle="Praca w tym samym środowisku usługi Office 365 na dowolnym urządzeniu z usługą Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak udostępniać użytkownikom dowolną aplikację Office 365 przy użyciu usługi Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>



# Praca w tym samym środowisku usługi Office 365 na dowolnym urządzeniu z usługą Azure RemoteApp

> [AZURE.IMPORTANT]
> Usługa Azure RemoteApp nie jest już obsługiwana. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).

W tym artykule opisano sposób wdrażania usługi Office 365 na dowolnym urządzeniu w firmie. Użytkownicy mogą korzystać z tych samych możliwości i środowiska interfejsu użytkownika na urządzeniach Apple oraz z systemami Android i Windows.

Osiągniemy ten cel dzięki usłudze Azure RemoteApp, obsługując usługę Office 365 na skalowalnych maszynach wirtualnych platformy Azure, z którymi użytkownicy mogą nawiązywać połączenie. Ten zestaw maszyn wirtualnych nazywamy „kolekcją w chmurze”.

## Tworzenie kolekcji w chmurze

Najpierw, po utworzeniu konta platformy Azure, przejdź do usługi **RemoteApp**, klikając link po lewej stronie.
![Wyświetlanie usługi Azure RemoteApp w witrynie Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Następnie kontynuuj, klikając pozycję **nowe** w dolnej części ekranu i „szybko tworząc” kolekcję. Podaj nazwę, region, subskrypcję, plan i obraz „Office Proffesional 2013” udostępniony przez firmę Microsoft.
![Okno dialogowe tworzenia](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Gdy wypełnisz formularz, powinien rozpocząć się proces tworzenia kolekcji. Może on potrwać mniej więcej godzinę.

![Oczekiwanie](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Po zakończeniu procesu ekran będzie wyglądać podobnie do poniższego. Po kliknięciu pozycji **Publikowanie** zobaczysz, że większość aplikacji pakietu Office została już opublikowana.
![Utworzona kolekcja](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Opublikowane aplikacje](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

W tym miejscu możesz także dodać więcej użytkowników z dostępem do tej kolekcji, klikając pozycję **Dostęp użytkowników**.
![Konfigurowanie dostępu użytkowników](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Teraz spróbujmy nawiązać połączenie z usługą Office 365.

## Łączenie z usługą Office 365

Przejdź do strony [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), przewiń w dół i kliknij pozycję **Pobierz klientów**, aby zainstalować klienta usługi Azure RemoteApp na aktualnie używanym urządzeniu. Poniższe zrzuty ekranu dotyczą systemu Windows.

Po uruchomieniu aplikacji zostanie wyświetlony monit o zalogowanie przy użyciu konta Microsoft (zwanego wcześniej „Live ID”). Zaloguj się za pomocą konta używanego jako konto platformy Azure. Po zalogowaniu zobaczysz powiadomienia o nowych zaproszeniach. Kliknij w tym miejscu, aby wyświetlić listę podobną do poniższej. Zaakceptuj zaproszenie odpowiadające adresowi e-mail właściciela konta platformy Azure.

![Nowe zaproszenie](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Ekran z nowymi zaproszeniami wygląda następująco:

![Akceptowanie zaproszenia](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Po zaakceptowaniu zaproszenia w kliencie usługi Azure RemoteApp powinny być widoczne wszystkie aplikacje pakietu Office.

![Lista aplikacji](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Kliknięcie dowolnej aplikacji powinno spowodować jej uruchomienie na maszynie wirtualnej platformy Azure. Wszystko gotowe. Owocnej pracy.

![uruchamianie](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![powerpoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)



<!--HONumber=Sep16_HO3-->


