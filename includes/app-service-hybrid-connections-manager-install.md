
1.  **ハイブリッド接続** ブレードで、をクリックしたハイブリッド接続が作成されると、クリックして **リスナーのセットアップ**します。
    
    ![[リスナーのセットアップ] をクリック](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4.  **ハイブリッド接続のプロパティ** ブレードが開きます。  **内部設置型 Hybrid Connection Manager**, 、選択 **をダウンロードして手動で構成**, 、ダウンロードした HybridConnectionManager.msi パッケージを保存し、ゲートウェイの接続文字列をコピーします。
    
    ![ここをクリックしてインストール](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. 管理者のコマンド プロンプトから、次のコマンドを入力してインストーラーを起動します。

        start HybridConnectionManager.msi
 
7. インストーラーの実行後にをクリックして **今**, し、参照、%ProgramFiles%\Microsoft\HybridConnectionManager フォルダーに、HCMConfigWizard.exe を実行してをクリックして **はい** で、 **ユーザー アカウント制御** ダイアログ。
        
7. 先ほどコピーしたハイブリッド接続文字列を貼り付け、 **OK**します。 
    
    ![インストール中](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. インストールが完了したら、クリックして **閉じる**します。
    
    ![[閉じる] をクリックしてください](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
     **ハイブリッド接続** ブレードで、 **ステータス** 列が表示されます **接続**します。 
    
    ![接続されている状態](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)
