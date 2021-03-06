---
title: "チュートリアル: Azure Active Directory と MaxxPoint の統合 | Microsoft Docs"
description: "Azure Active Directory と MaxxPoint の間でシングル サインオンを構成する方法について説明します。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d92e3edb-31aa-4d21-9bf8-6e5917d7bf5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 6844a46978618d5dc3292168bbcc228480311d47
ms.openlocfilehash: e84a3aeb9e7a8cddccb293ab2bb9d7fdc5fa82b4
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a>チュートリアル: Azure Active Directory と MaxxPoint の統合

このチュートリアルでは、MaxxPoint と Azure Active Directory (Azure AD) を統合する方法について説明します。

MaxxPoint と Azure AD の統合には、次の利点があります。

- MaxxPoint にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に MaxxPoint にサインオン (シングル サインオン) できるように設定できます。
- 1 つの中央サイト (Microsoft Azure 管理ポータル) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「 [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

MaxxPoint と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- MaxxPoint でのシングル サインオンが有効なサブスクリプション


> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。


このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の試用環境がない場合は、[こちら](https://azure.microsoft.com/pricing/free-trial/)から&1; か月の試用版を入手できます。


## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の&2; つの要素で構成されています。

1. ギャラリーからの MaxxPoint の追加
2. Azure AD シングル サインオンの構成とテスト


## <a name="adding-maxxpoint-from-the-gallery"></a>ギャラリーからの MaxxPoint の追加
Azure AD への MaxxPoint の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に MaxxPoint を追加する必要があります。

**ギャラリーから MaxxPoint を追加するには、次の手順を実行します。**

1. **[Microsoft Azure 管理ポータル](https://portal.azure.com)**の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]

2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![アプリケーション][2]
    
3. ダイアログの上部にある **[追加]** をクリックします。

    ![アプリケーション][3]

4. 検索ボックスに「**MaxxPoint**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_001.png)

5. 結果ウィンドウで **[MaxxPoint]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、MaxxPoint で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する MaxxPoint ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと MaxxPoint の関連ユーザーの間で、リンク関係が確立されている必要があります。

このリンク関係は、Azure AD の **[ユーザー名]** の値を、MaxxPoint の **[Username (ユーザー名)]** の値として割り当てることで確立されます。

MaxxPoint で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[MaxxPoint のテスト ユーザーの作成](#creating-a-maxxpoint-test-user)** - MaxxPoint で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[Testing Single Sign-On](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Microsoft Azure 管理ポータルで Azure AD のシングル サインオンを有効にし、MaxxPoint アプリケーションでシングル サインオンを構成します。

**MaxxPoint で Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Microsoft Azure 管理ポータルの **MaxxPoint** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![[シングル サインオンの構成]][4]

2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![Configure Single Sign-On](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_300.png)

3. **[MaxxPoint のドメインと URL]** セクションで、**IDP 開始モード**でアプリケーションを構成する場合は、手順を実行する必要はありません。

    ![Configure Single Sign-On](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
4. **[MaxxPoint のドメインと URL]** セクションで、**SP 開始モード**でアプリケーションを構成する場合は、次の手順を実行します。
    
    ![[シングル サインオンの構成]](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    a. **[詳細な URL 設定の表示]** をクリックします。

    b. **[サインオン URL]** ボックスに、`https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure` のパターンを使用して URL を入力します。

    > [!NOTE] 
    > これは実際の値ではないので注意してください。 この値は実際のサインオン URL で更新する必要があります。 これらの値を取得するには、[MaxxPoint サポート チーム](mailto:iumsupport@intercall.com)に問い合わせてください。

5. **[SAML 署名証明書]** セクションで、**[新しい証明書の作成]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_400.png)     

6. **[新しい証明書の作成]** ダイアログで、カレンダー アイコンをクリックし、**期限日**を選択します。 **[保存]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_500.png)

7. **[SAML 署名証明書]** セクションで、**[Make new certificate active (新しい証明書を有効にする)]** をクリックし、**[保存]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_05.png)

8. ポップアップ表示される **[Rollover certificate (ロール オーバー証明書)]** ウィンドウで、**[OK]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_600.png)

9. **[SAML 署名証明書]** セクションで、**[Metadata XML (メタデータ XML)]** をクリックし、コンピューターにメタデータ ファイルを保存します。

    ![[シングル サインオンの構成]](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

10. アプリケーション用に構成された SSO を入手するには、[MaxxPoint サポート チーム](mailto:iumsupport@intercall.com)に連絡し、ダウンロードした**メタデータ**を提供してください。 


### <a name="creating-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Microsoft Azure 管理ポータルで Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_01.png) 

2. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_02.png) 

3. ダイアログの上部にある **[追加]** をクリックして **[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_03.png) 

4. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_04.png) 

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。

    c. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。

    d. **[作成]**をクリックします。 



### <a name="creating-a-maxxpoint-test-user"></a>MaxxPoint のテスト ユーザーの作成

このセクションでは、MaxxPoint で Britta Simon というユーザーを作成します。 [MaxxPoint サポート チーム](mailto:iumsupport@intercall.com)と連携し、MaxxPoint プラットフォームにユーザーを追加してください。


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に MaxxPoint へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザーの割り当て][200] 

**MaxxPoint に Britta Simon を割り当てるには、次の手順を実行します。**

1. Microsoft Azure 管理ポータルでアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

2. アプリケーションの一覧で **[MaxxPoint]** を選択します。

    ![Configure Single Sign-On](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    


### <a name="testing-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで [MaxxPoint] タイルをクリックすると、自動的に MaxxPoint アプリケーションにサインオンします。


## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_203.png
