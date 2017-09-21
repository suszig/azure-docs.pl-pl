W lokalnym oknie terminala dodaj zdalną platformę Azure do lokalnego repozytorium Git.

```bash
git remote add azure <URI from previous step>
```

Wypchnij na zdalną platformę Azure w celu wdrożenia aplikacji za pomocą następującego polecenia. Gdy zostanie wyświetlony monit o podanie hasła, upewnij się, że wprowadzasz hasło utworzone podczas [konfiguracji użytkownika wdrożenia](#configure-a-deployment-user), a nie hasło, którego używasz do logowania się w witrynie Azure Portal.

```bash
git push azure master
```

Poprzednie polecenie wyświetli informacje podobne do następującego przykładu:
