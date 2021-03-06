<properties
	pageTitle="チュートリアル: Azure Active Directory と Hightail の統合 | Microsoft Azure"
	description="Azure Active Directory と Hightail の間でシングル サインオンを構成する方法について説明します。"
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/11/2016"
	ms.author="jeedes"/>


# チュートリアル: Azure Active Directory と Hightail の統合

このチュートリアルの目的は、Hightail と Azure Active Directory (Azure AD) を統合する方法を説明することです。

Hightail と Azure AD の統合には、次の利点があります。

- Hightail にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Hightail にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Active Directory ポータル) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」を参照してください。

## 前提条件

Hightail と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Hightail でのシングル サインオンが有効なサブスクリプション


> [AZURE.NOTE] このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます。


## シナリオの説明
このチュートリアルの目的は、テスト環境で Azure AD のシングル サインオンをテストできるようにすることです。

このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーから Hightail を追加する
2. Azure AD シングル サインオンの構成とテスト


## ギャラリーから Hightail を追加する
Azure AD への Hightail の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Hightail を追加する必要があります。

**ギャラリーから Hightail を追加するには、次の手順を実行します。**

1. **Azure クラシック ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。 
 
	![Active Directory][1]

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

	![アプリケーション][2]

4. ページの下部にある **[追加]** をクリックします。

	![アプリケーション][3]

5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

	![アプリケーション][4]

6. 検索ボックスに、「**Hightail**」と入力します。

	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_01.png)

7. 結果ウィンドウで **[Hightail]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

	![ギャラリーでアプリを選択する](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_02.png)

##  Azure AD シングル サインオンの構成とテスト
このセクションの目的は、"Britta Simon" というテスト ユーザーに基づいて、Hightail で Azure AD のシングル サインオンを構成し、テストする方法について説明することです。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Hightail ユーザーが Azure AD で認識されている必要があります。言い換えると、Azure AD ユーザーと Hightail の関連ユーザーの間で、リンク関係が確立されている必要があります。

このリンク関係は、Azure AD の **[ユーザー名]** の値を、Hightail の **[Username]** の値として割り当てることで確立されます。

Hightail で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
4. **[Hightail のテスト ユーザーの作成](#creating-a-hightail-test-user)** - Hightail で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
5. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### Azure AD シングル サインオンの構成

このセクションの目的は、Azure クラシック ポータルで Azure AD のシングル サインオンを有効にすることと、Hightail アプリケーションでシングル サインオンを構成することです。

Hightail アプリケーションは、特定の形式で構成された SAML アサーションを受け入れます。このアプリケーションには、次の要求を構成してください。この属性の値は、アプリケーションの **[属性]** タブから管理できます。次のスクリーンショットはその例です。

![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_51.png)

**Hightail で Azure AD シングル サインオンを構成するには、次の手順に従います。**


1. Azure クラシック ポータルの **[Hightail]** アプリケーション統合ページで、上部のメニューから **[属性]** をクリックします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_81.png)


1. **[Saml トークン属性]** ダイアログで、以下の表の各行について、次の手順を実行します。

	| 属性名 | 属性値 |
	| --- | --- |    
	| FirstName | User.givenname |
    | LastName | User.surname |
	| 電子メール | User.mail |
	| UserIdentity | User.mail |

	a.**[ユーザー属性の追加]** をクリックして **[ユーザー属性の追加]** ダイアログを開きます。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_82.png)


	b.**[属性名]** ボックスに、その行に対して表示される属性名を入力します。

    c.**[属性値]** リストから、その行に対して表示される属性値を選択します。

    d.**[完了]** をクリックします。
	



1. 上部のメニューで **[クイック スタート]** をクリックします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)



2. **[ユーザーの Hightail へのアクセスを設定してください]** ページで、**[Microsoft Azure AD シングル サインオン]** を選択し、**[次へ]** をクリックします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_03.png)


3. **[アプリケーション設定の構成]** ダイアログ ページで、**IDP 開始モード**でアプリケーションを構成する場合は、次の手順を実行して **[次へ]** をクリックします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_04.png)



    a.**[応答 URL]** テキストボックスに **"https://www.hightail.com/samlLogin?phi\_action=app/samlLogin&subAction=handleSamlResponse"** というパターンで URL を入力します。

	b.**[次へ]** をクリックします。

4. **[アプリケーション設定の構成]** ダイアログ ページで、**SP 開始モード**でアプリケーションを構成する場合は、**[詳細設定を表示します (オプション)]** をクリックし、**[サインオン URL]** を入力して **[次へ]** をクリックします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_06.png)

	a.[サインオン URL] テキストボックスに、**https://www.hightail.com/loginSSO** というパターンで、Hightail アプリケーションにサインオンするユーザーが使用する URL を入力します。これは、SSO を使用するすべてのユーザーに共通のログイン ページです。

	b.**[次へ]** をクリックします。

5. **[Hightail シングル サインオン パラメーターの構成]** ページで、次の手順を実行し、**[次へ]** をクリックします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_05.png)


    a.**[証明書のダウンロード]** をクリックし、コンピューターに Base-64 でエンコードされた証明書ファイルを保存します。

    b.**[次へ]** をクリックします。

	> [AZURE.NOTE] Hightail アプリでシングル サインオンを構成する前に、電子メール ドメインを Hightail チームのホワイト リストに登録し、そのドメインを使用するすべてのユーザーがシングル サインオン機能を利用できるようにします。

6. アプリケーションに合わせて SSO を構成するには、管理者として Hightail テナントにサインオンする必要があります。

	a.上部にあるメニューの **[Account]** タブをクリックし、**[Configure SAML]** を選択します。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png)


	b.**[Enable SAML Authentication]** チェックボックスをオンにします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png)

	c.Base-64 でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして、**[SAML Token Signing Certificate]** ボックスに貼り付けます。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png)


	d.Azure AD のリモート ログイン URL を Hightail の **[SAML Authority (Identity Provider)]** にコピーします。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_005.png)
	
	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

	e.**IDP 開始モード**でアプリケーションを構成する場合は、**[Identity Provider (IdP) initiated log in]** を選択します。**SP 開始モード**の場合は、**[Service Provider (SP) initiated log in]** を選択します。
	
	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

	f.インスタンスの SAML コンシューマー URL をコピーし、手順 4 のように **[応答 URL]** テキストボックスに貼り付けます。

	g.**[保存]** をクリックします。



6. Azure クラシック ポータルで、シングル サインオンの構成確認を選択し、**[次へ]** をクリックします。

	![Azure AD Single Sign-On][10]

7. **[シングル サインオンの確認]** ページで **[完了]** をクリックします。
 
	![Azure AD Single Sign-On][11]




### Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure クラシック ポータルで Britta Simon というテスト ユーザーを作成することです。

ユーザーの一覧で **[Britta Simon]** を選択します。

![Azure AD ユーザーの作成][20]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure クラシック ポータル**の左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_09.png)

2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3. 上部のメニューで **[ユーザー]** をクリックして、ユーザーの一覧を表示します。
 
	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png)


4. 下部にあるツール バーで **[ユーザーの追加]** をクリックして、**[ユーザーの追加]** ダイアログ ボックスを開きます。

	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png)

5. **[このユーザーに関する情報の入力]** ダイアログ ページで、次の手順に従います。

	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_05.png)

    a.**[ユーザーの種類]** として **[組織内の新しいユーザー]** を選択します。

    b.**[ユーザー名]** ボックスに「**BrittaSimon**」と入力します。

    c.**[次へ]** をクリックします。

6.  **[ユーザー プロファイル]** ダイアログ ページで、次の手順を実行します。

	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_06.png)

    a.**[名]** ボックスに「**Britta**」と入力します。

    b.**[姓]** ボックスに「**Simon**」と入力します。

    c.**[表示名]** ボックスに「**Britta Simon**」と入力します。

    d.**[ロール]** 一覧で **[ユーザー]** を選択します。

    e.**[次へ]** をクリックします。

7. **[一時パスワードの取得]** ダイアログ ページで、**[作成]** をクリックします。

	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_07.png)


8. **[一時パスワードの取得]** ダイアログ ページで、次の手順に従います。
 
	![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-hightail-tutorial/create_aaduser_08.png)

    a.**[新しいパスワード]** の値を書き留めます。

    b.**[完了]** をクリックします。



### Hightail テスト ユーザーの作成

このセクションの目的は、Hightail で Britta Simon というユーザーを作成することです。

このセクションでは、ユーザー側で必要な操作はありません。Hightail は、カスタムの要求に基づく時間どおりのユーザー プロビジョニングをサポートしています。前述のセクション「**[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-sign-on)**」に従ってカスタムの要求を構成した場合、まだ存在していないアプリケーションにユーザーが自動的に作成されます。

> [AZURE.NOTE] ユーザーを手動で作成する必要がある場合は、Hightail サポート チーム ([support@hightail.com](mailto:support@hightail.com)) に問い合わせてください。


### Azure AD テスト ユーザーの割り当て

このセクションの目的は、Britta Simon に Hightail へのアクセスを許可することで、このユーザーが Azure のシングル サインオンを使用できるようにすることです。

![ユーザーの割り当て][200]

**Hightail に Britta Simon を割り当てるには、次の手順を実行します。**

1. Azure クラシック ポータルでアプリケーション ビューを開くために、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
 
	![ユーザーの割り当て][201]

2. アプリケーションの一覧で **[Hightail]** を選択します。

	![Configure Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_50.png)


1. 上部のメニューで **[ユーザー]** をクリックします。

	![ユーザーの割り当て][203]

1. ユーザーの一覧で **[Britta Simon]** を選択します。

2. 下部にあるツール バーで **[割り当て]** をクリックします。

![ユーザーの割り当て][205]



### シングル サインオンのテスト

このセクションの目的は、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストすることです。

アクセス パネルで Hightail のタイルをクリックすると、自動的に Hightail アプリケーションにサインオンします。


## その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0413_2016-->