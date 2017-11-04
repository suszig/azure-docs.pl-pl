#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Konfigurowanie projektu systemu iOS w programie Xamarin Studio
1. Otwórz w Xamarin.Studio, **Info.plist**i zaktualizuj **identyfikator pakietu** o identyfikatorze pakietu utworzonego wcześniej z nowego identyfikatora aplikacji.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Przewiń w dół do **tryby tła**. Wybierz **Włącz tryby tła** pole i **zdalnego powiadomienia** pole.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Kliknij dwukrotnie plik projektu w panelu rozwiązania, aby otworzyć **opcje projektu**.
4. W obszarze **kompilacji**, wybierz **iOS podpisywania pakietu**i wybierz odpowiedniego tożsamości i inicjowania obsługi profilu można właśnie skonfigurowane zapasowej dla tego projektu.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Dzięki temu, że projekt korzysta z nowego profilu do podpisywania kodu. Dla inicjowania obsługi dokumentacji oficjalnego urządzeń Xamarin [Inicjowanie obsługi administracyjnej urządzeń Xamarin].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Konfigurowanie projektu systemu iOS w programie Visual Studio
1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **właściwości**.
2. Na stronach właściwości, kliknij przycisk **aplikacji systemu iOS** , a następnie zaktualizować **identyfikator** o identyfikatorze utworzony wcześniej.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. W **iOS podpisywania pakietu** , wybierz odpowiedni tożsamości i skonfigurowany tylko dla tego projektu profil inicjowania obsługi administracyjnej.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dzięki temu, że projekt korzysta z nowego profilu do podpisywania kodu. Dla inicjowania obsługi dokumentacji oficjalnego urządzeń Xamarin [Inicjowanie obsługi administracyjnej urządzeń Xamarin].
4. Kliknij dwukrotnie plik Info.plist, aby go otworzyć, a następnie Włącz **RemoteNotifications** w obszarze **tryby tła**.

[Inicjowanie obsługi administracyjnej urządzeń Xamarin]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
