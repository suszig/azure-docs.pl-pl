<properties services="virtual-machines" title="How to Log on to a Virtual Machine Running Windows Server" authors="cynthn" solutions="" manager="timlt" editor="tysonn" />

4. クリックすると **接続** を作成し、リモート デスクトップ プロトコル (.rdp) ファイルをダウンロードします。 クリックして **開く** このファイルを使用します。

5. リモート デスクトップ ウィンドウで [ **接続** を続行します。

    ![接続の続行](./media/virtual-machines-log-on-win-server/connectpublisher.png)

6. Windows セキュリティ] ウィンドウで、仮想マシンで、アカウントの資格情報を入力し、クリックして **OK**します。

    ほとんどの場合は、仮想マシンの作成時に指定したローカル アカウントのユーザー名とパスワードです。 この場合、ドメインとは、仮想マシンの名前ととして入力されて *vmname*& #92;*ユーザー名*します。  
    
    仮想マシンに属する場合、組織でのドメイン、ユーザー名には形式でそのドメインの名前が含まれています確認 *ドメイン*& #92;*ユーザー名*します。 さらに、アカウントは、管理者グループに属しているか、または VM へのリモート アクセス特権が付与されている必要があります。
    
    仮想マシンがドメイン コントローラーである場合は、そのドメインのドメイン管理者アカウントのユーザー名とパスワードを入力します。

7.  をクリックして **はい** を仮想マシンの id を確認し、ログオンを完了します。

    ![目的の仮想マシンであることを確認](./media/virtual-machines-log-on-win-server/connectverify.png)

