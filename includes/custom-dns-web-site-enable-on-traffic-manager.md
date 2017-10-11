Po wykonaniu propagacji rekordów dla nazwy domeny należy sprawdzić, czy niestandardową nazwę domeny można uzyskać dostępu do aplikacji sieci web w usłudze Azure App Service za pomocą przeglądarki.

> [!NOTE]
> Może upłynąć trochę czasu, zanim Twoje CNAME propagację za pośrednictwem systemu DNS. Można użyć usługi, takie jak <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> Aby weryfikować dostępność CNAME.
> 
> 

Jeśli aplikacja sieci web nie ma jeszcze dodany jako punkt końcowy Menedżera ruchu, możesz to zrobić przed rozpoznawanie nazw działa jako trasy nazwy domeny niestandardowej do usługi Traffic Manager. Menedżer ruchu jest następnie przekierowuje do aplikacji sieci web. Skorzystaj z informacji w [Dodawanie lub usuwanie punktów końcowych](../articles/traffic-manager/traffic-manager-endpoints.md) można dodać aplikację sieci web jako punktu końcowego w profilu usługi Traffic Manager.

> [!NOTE]
> Jeśli aplikacja sieci web nie jest wyświetlana podczas dodawania punktu końcowego, sprawdź, czy jest ono skonfigurowane dla **standardowe** tryb planu usługi aplikacji. Należy użyć **standardowe** tryb dla aplikacji sieci web w celu zapewnienia ich współdziałania z usługą Traffic Manager.
> 
> 

1. W przeglądarce otwórz [Azure Portal](https://portal.azure.com).
2. W **aplikacje sieci Web** kliknij nazwę aplikacji sieci web, wybierz pozycję **ustawienia**, a następnie wybierz **domen niestandardowych**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. W **domen niestandardowych** bloku, kliknij przycisk **dodać nazwę hosta**.
4. Użyj **Hostname** pola tekstowe do wprowadzania nazwy domeny usługi Traffic Manager do skojarzenia z tą aplikacją sieci web.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Kliknij przycisk **weryfikacji** Aby zapisać konfigurację nazwy domeny.
6. Po kliknięciu **weryfikacji** Azure będzie rozpocząć wyłączyć weryfikację domeny przepływu pracy. Spowoduje to zaewidencjonowanie własność domeny, a także Hostname Powodzenie dostępności i raportów lub szczegółowe informacje o błędzie z wskazowki porady na temat naprawić błąd.    
7. Po pomyślnym zweryfikowaniem **dodać nazwę hosta** przycisk staną się aktywne i będzie można przypisać nazwy hosta. Teraz przejdź do niestandardowej nazwy domeny w przeglądarce. Powinna zostać wyświetlona Twojej pracy aplikacji przy użyciu niestandardowej nazwy domeny. 
   
   Po zakończeniu konfiguracji niestandardowej nazwy domeny będzie wyświetlane w **nazwy domen** części aplikacji sieci web.

W tym momencie można wprowadzić nazwę nazwy domeny usługi Traffic Manager w przeglądarce i zobaczyć, czy go pomyślnie przejście do aplikacji sieci web.

