---
title: 'チュートリアル: Azure Active Directory と Box の統合 | Microsoft Docs'
description: Azure Active Directory と Box の間にシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2017
ms.author: jeedes
ms.openlocfilehash: 638ae63057df00375b05a58e3ceab510e2a608de
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2018
---
# <a name="integrate-azure-active-directory-with-box"></a>Azure Active Directory と Box の統合

このチュートリアルでは、Box と Azure Active Directory (Azure AD) を統合する方法について説明します。

Azure AD と Box の統合には、次の利点があります。

- Box にアクセスするユーザーを Azure AD で制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Box にサインオン (シングル サインオン (SSO)) できるように設定できます。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Box と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Box SSO が有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を*使用しない*ことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従います。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[1 か月の評価版を入手できます](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 

このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの Box の追加
2. Azure AD シングル サインオンの構成とテスト

## <a name="add-box-from-the-gallery"></a>ギャラリーからの Box の追加
Azure AD と Box の統合を構成するには、次の手順でギャラリーから管理対象 SaaS アプリの一覧に Box を追加する必要があります。

1. [Azure Portal](https://portal.azure.com) の左側のウィンドウで、**[Azure Active Directory]** を選択します。 

    ![Azure Active Directory のボタン][1]

2. **[エンタープライズ アプリケーション]** > **[すべてのアプリケーション]** の順に選択します。

    ![[エンタープライズ アプリケーション] ウィンドウ][2]
    
3. 新しいアプリケーションを追加するには、ウィンドウの上部にある **[新しいアプリケーション]** ボタンを選びます。

    ![[新しいアプリケーション] ボタン][3]

4. 検索ボックスに「**Box**」と入力し、結果リストで **[Box]** を選択してから、**[追加]** を選択します。

    ![結果一覧の Box](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト

このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、Box で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Box ユーザーが Azure AD で特定されている必要があります。 言い換えると、Azure AD ユーザーと Box の同じユーザーの間で、リンク関係が確立されている必要があります。

リンク関係を確立するには、Azure AD の "*ユーザー名*" の値を Box の *Username* として割り当てます。

Box で Azure AD のシングル サインオンを構成してテストするには、次からの 5 つのセクションで構成要素を完了します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

次の手順に従って、Azure Portal で Azure AD のシングル サインオンを有効にし、Box アプリケーションでシングル サインオンを構成します。

1. Azure Portal の **Box** アプリケーション統合ウィンドウで、**[シングル サインオン]** を選択します。

    ![[シングル サインオン] リンク][4]

2. **[シングル サインオン]** ウィンドウの **[シングル サインオン モード]** ボックスで、**[SAML ベースのサインオン]** を選択します。
 
    ![[シングル サインオン] ウィンドウ](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. **[Box のドメインと URL]** で、次の手順を実行します。

    ![[Box のドメインと URL] のシングル サインオン情報](./media/active-directory-saas-box-tutorial/url3.png)

    a.[サインオン URL] ボックスに、次のパターンを使用して、ユーザーが RightScale アプリケーションへのサインオンに使用する URL を入力します。 **[サインオン URL]** ボックスに、*https://\<subdomain>.box.com* の形式で URL を入力します。

    b. **[識別子]** ボックスに「**box.net**」を入力します。
     
    > [!NOTE] 
    > 上記の値は、実際の値ではありません。 実際のサインオン URL と識別子で値を更新してください。 値を取得するには、[Box クライアント サポート チーム](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire)に問い合わせてください。 

4. **[SAML 署名証明書]** で、**[メタデータ XML]** を選択し、コンピューターにメタデータ ファイルを保存します。

    ![証明書のダウンロードのリンク](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. **[保存]** を選択します。

    ![[シングル サインオンの構成] の [保存] ボタン](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)
    
6. アプリケーションの SSO を構成するには、「[Set up SSO on your own](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown)」(自分で SSO を設定する) の手順に従います。

> [!NOTE] 
> Box アカウント用の SSO 設定を有効にできない場合は、必要に応じて、[Box クライアント サポート チーム](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire)にお問い合わせのうえ、ダウンロードした XML ファイルを提出してください。

> [!TIP]
> アプリのセットアップ中、上記手順の簡易版を [Azure Portal](https://portal.azure.com) でご覧いただけます。 **[Active Directory]** > **[エンタープライズ アプリケーション]** セクションにこのアプリを追加した後、**[シングル サインオン]** タブを選択し、下部にある **[構成]** セクションの組み込みドキュメントにアクセスします。 組み込みドキュメント機能の詳細については、[Azure AD の組み込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関する記事をご覧ください。
>

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成

このセクションでは、Azure Portal で次の手順に従って Britta Simon というテスト ユーザーを作成します。

![Azure AD のテスト ユーザーの作成][100]

1. Azure Portal の左側のウィンドウで、**[Azure Active Directory]** を選択します。

    ![[Azure Active Directory] リンク](./media/active-directory-saas-box-tutorial/create_aaduser_01.png)

2. 現在のユーザーの一覧を表示するには、**[ユーザーとグループ]** > **[すべてのユーザー]** の順に選択します。

    ![[ユーザーとグループ] と [すべてのユーザー] リンク](./media/active-directory-saas-box-tutorial/create_aaduser_02.png)

3. **[すべてのユーザー]** の上部にある **[追加]** を選択します。

    ![[追加] ボタン](./media/active-directory-saas-box-tutorial/create_aaduser_03.png)

    **[ユーザー]** ウィンドウが開きます。

4. **[ユーザー]** ウィンドウで、次の手順を実行します。

    ![[ユーザー] ウィンドウ](./media/active-directory-saas-box-tutorial/create_aaduser_04.png)

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに、ユーザーである Britta Simon の電子メール アドレスを入力します。

    c. **[パスワードを表示]** チェック ボックスをオンにし、**[パスワード]** ボックスに表示された値を書き留めます。

    d. **[作成]**を選択します。
 
### <a name="create-a-box-test-user"></a>Box テスト ユーザーの作成

このセクションでは、Box で Britta Simon というテスト ユーザーを作成します。 Box では、Just-In-Time プロビジョニングがサポートされています。この設定は既定で有効になっています。 ユーザーがまだ存在していない場合は、Box にアクセスしようとしたときに新しいユーザーが作成されます。 ユーザーを作成する操作は不要です。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、ユーザー Britta Simon に Box へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。 そのためには、次の手順を実行します。

![ユーザー ロールを割り当てる][200]

1. Azure Portal で **[アプリケーション]** ビューを開いて**ディレクトリ** ビューに移動してから、**[エンタープライズ アプリケーション]** > **[すべてのアプリケーション]** の順に選択します。

    ![[エンタープライズ アプリケーション] と [すべてのアプリケーション] のリンク][201] 

2. **[アプリケーション]**リストで **[Box]** を選択します。

    ![[Box] リンク](./media/active-directory-saas-box-tutorial/tutorial_box_app.png)  

3. 左側のウィンドウで **[ユーザーとグループ]** を選択します。

    ![[ユーザーとグループ] リンク][202]

4. **[追加]** を選択し、**[割り当ての追加]** ウィンドウで **[ユーザーとグループ]** を選択します。

    ![[割り当ての追加] ウィンドウ][203]

5. **[ユーザーとグループ]** ウィンドウの **[ユーザー]** 一覧で、**Britta Simon** を選択します。

6. **[Select]\(選択\)** ボタンをクリックします。

7. **[割り当ての追加]** ウィンドウで **[割り当て]** を選択します。
    
### <a name="test-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで **[Box]** タイルを選択すると、Box アプリケーションにサインインするためのサインイン ページが開きます。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory の統合に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)
* [ユーザー プロビジョニングの構成](active-directory-saas-box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

