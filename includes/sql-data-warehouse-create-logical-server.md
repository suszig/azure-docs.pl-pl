### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Tworzenie nowego serwera logicznego SQL w witrynie Azure Portal

1. Kliknij pozycję **Nowy**, wyszukaj **serwer logiczny**, a następnie naciśnij klawisz **ENTER**.

    ![wyszukiwanie serwera logicznego](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Wybierz pozycję **SQL Server (serwer logiczny)**. 

    ![wybieranie serwera logicznego](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Kliknij pozycję **Utwórz**, aby otworzyć nowy blok SQL Server (serwer logiczny).

   <kbd> ![otwieranie logicznego bloku serwera](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd>
    <kbd>![logiczny blok serwera](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd>
  
3. W polu tekstowym nazwy serwera w bloku SQL Server (serwer logiczny) podaj prawidłową nazwę nowego serwera logicznego. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![nazwa nowego serwera](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > W pełni kwalifikowana nazwa nowego serwera to <Twoja_nazwa_serwera>.database.windows.net.
    >
    
4. W polu tekstowym Identyfikator logowania administratora serwera podaj nazwę użytkownika na potrzeby logowania w celu uwierzytelniania SQL dla tego serwera. Ta nazwa logowania jest nazywana główną nazwą logowania serwera. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![identyfikator logowania administratora SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. W polach tekstowych **Hasło** i **Potwierdź hasło** podaj hasło do głównego konta logowania serwera. Zielony znacznik wyboru wskazuje, czy podane hasło jest poprawne.
    
    ![Hasło administratora SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Wybierz subskrypcję, w której masz uprawnienia do tworzenia obiektów.

    ![subskrypcja](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. W polu tekstowym Grupa zasobów wybierz opcję **Utwórz nową**, a następnie w polu tekstowym Grupa zasobów podaj prawidłową nazwę nowej grupy (możesz także użyć istniejącej grupy zasobów, jeśli masz już taką utworzoną na własne potrzeby). Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.

    ![nowa grupa zasobów](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. W polu tekstowym **Lokalizacja** wybierz centrum danych odpowiednie dla danej lokalizacji — np. „Australia Wschodnia”.
    
    ![lokalizacja serwera](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > W tym bloku nie można zmienić pola wyboru dla opcji **Zezwalaj usługom platformy Azure na dostęp do serwera**. To ustawienie można zmienić w bloku zapory serwera. Aby uzyskać więcej informacji, zobacz artykuł [Wprowadzenie do zabezpieczeń](../articles/sql-database/sql-database-get-started-security.md).
    >
    
9. Kliknij przycisk **Utwórz**.

    ![przycisk Utwórz](./media/sql-data-warehouse-create-logical-server/create.png)



<!--HONumber=Jan17_HO3-->


