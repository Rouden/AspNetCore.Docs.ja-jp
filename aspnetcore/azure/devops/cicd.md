---
title: ASP.NET Core および Azure を使用した DevOps |継続的インテグレーションとデプロイ
author: CamSoper
description: Azure でホストされる ASP.NET Core アプリの DevOps パイプラインの構築に関するエンドツーエンドのガイダンスを提供するガイド。
ms.author: scaddie
ms.date: 08/17/2018
uid: azure/devops/cicd
ms.openlocfilehash: e084a6115dc7e176c17b2b318233b7a003b39a83
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/17/2018
ms.locfileid: "42909928"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="80ef3-103">継続的インテグレーションとデプロイ</span><span class="sxs-lookup"><span data-stu-id="80ef3-103">Continuous integration and deployment</span></span>

<span data-ttu-id="80ef3-104">前の章では、フィード リーダーの簡単なアプリのローカル Git リポジトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="80ef3-105">この章では、そのコードを GitHub リポジトリに発行し、Visual Studio Team Services (VSTS) の DevOps パイプラインを構築します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-105">In this chapter, you'll publish that code to a GitHub repository and construct a Visual Studio Team Services (VSTS) DevOps pipeline.</span></span> <span data-ttu-id="80ef3-106">パイプラインは、継続的なビルドとアプリのデプロイに使用できます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="80ef3-107">任意のコミットを GitHub リポジトリには、ビルドと Azure Web アプリのステージング スロットにデプロイをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="80ef3-108">このセクションで、次のタスクを完了します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="80ef3-109">アプリのコードを GitHub に発行します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="80ef3-110">ローカル Git デプロイを切断します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="80ef3-111">VSTS アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-111">Create a VSTS account</span></span>
* <span data-ttu-id="80ef3-112">VSTS でチーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-112">Create a team project in VSTS</span></span>
* <span data-ttu-id="80ef3-113">ビルド定義の作成</span><span class="sxs-lookup"><span data-stu-id="80ef3-113">Create a build definition</span></span>
* <span data-ttu-id="80ef3-114">リリース パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-114">Create a release pipeline</span></span>
* <span data-ttu-id="80ef3-115">GitHub に変更をコミットし、自動的に Azure にデプロイ</span><span class="sxs-lookup"><span data-stu-id="80ef3-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="80ef3-116">VSTS の DevOps パイプラインを確認します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-116">Examine the VSTS DevOps pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="80ef3-117">アプリのコードを GitHub に発行します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="80ef3-118">ブラウザーのウィンドウを開きに移動します`https://github.com`します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="80ef3-119">をクリックして、 **+** ドロップダウンで、ヘッダーと選択**新しいリポジトリ**:</span><span class="sxs-lookup"><span data-stu-id="80ef3-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![新しいリポジトリの GitHub オプション](media/cicd/github-new-repo.png)

1. <span data-ttu-id="80ef3-121">自分のアカウントを選択、**所有者**ドロップダウンでは、入力と*単純なフィード-リーダー*で、**リポジトリ名**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="80ef3-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="80ef3-122">をクリックして、**リポジトリの作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="80ef3-123">ローカル コンピューターのコマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-123">Open your local machine's command shell.</span></span> <span data-ttu-id="80ef3-124">ディレクトリに移動し、*単純なフィード-リーダー* Git リポジトリが格納されています。</span><span class="sxs-lookup"><span data-stu-id="80ef3-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="80ef3-125">既存の名前を変更*原点*にリモート*上流*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="80ef3-126">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-126">Execute the following command:</span></span>
    ```console
    git remote rename origin upstream
    ```
1. <span data-ttu-id="80ef3-127">新しい追加*原点*github リポジトリのコピーをリモートでポイントします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="80ef3-128">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-128">Execute the following command:</span></span>
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. <span data-ttu-id="80ef3-129">新しく作成された GitHub リポジトリをローカル Git リポジトリを発行します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="80ef3-130">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-130">Execute the following command:</span></span>
    ```console
    git push -u origin master
    ```
1. <span data-ttu-id="80ef3-131">ブラウザーのウィンドウを開きに移動します`https://github.com/<GitHub_username>/simple-feed-reader/`します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="80ef3-132">GitHub リポジトリでコードが表示されることを検証します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="80ef3-133">ローカル Git デプロイを切断します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="80ef3-134">次の手順でローカル Git デプロイを削除します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="80ef3-135">VSTS は、置き換え両方、その機能を強化します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-135">VSTS both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="80ef3-136">開く、 [Azure portal](https://portal.azure.com/)に移動し、*ステージング (mywebapp\<unique_number\>ステージング/)* Web アプリ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="80ef3-137">入力して、Web アプリをすばやく配置できます*ステージング*ポータルの検索ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![ステージング Web アプリの検索語句](media/cicd/portal-search-box.png)

1. <span data-ttu-id="80ef3-139">クリックして**展開オプション**します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-139">Click **Deployment options**.</span></span> <span data-ttu-id="80ef3-140">新しいパネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-140">A new panel appears.</span></span> <span data-ttu-id="80ef3-141">クリックして**切断**前の章で追加されたローカル Git ソース管理構成を削除します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="80ef3-142">クリックして、削除操作を確定します、**はい**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="80ef3-143">移動し、 *mywebapp < unique_number >* App Service。</span><span class="sxs-lookup"><span data-stu-id="80ef3-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="80ef3-144">念のため、App Service をすばやく検索する、ポータルの検索ボックスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="80ef3-145">クリックして**展開オプション**します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-145">Click **Deployment options**.</span></span> <span data-ttu-id="80ef3-146">新しいパネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-146">A new panel appears.</span></span> <span data-ttu-id="80ef3-147">クリックして**切断**前の章で追加されたローカル Git ソース管理構成を削除します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="80ef3-148">クリックして、削除操作を確定します、**はい**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-a-vsts-account"></a><span data-ttu-id="80ef3-149">VSTS アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-149">Create a VSTS account</span></span>

1. <span data-ttu-id="80ef3-150">ブラウザーを開きに移動、 [VSTS アカウントの作成ページ](https://go.microsoft.com/fwlink/?LinkId=307137)します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-150">Open a browser, and navigate to the [VSTS account creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="80ef3-151">一意の名前を入力、**覚えやすい名前を選んで**テキスト ボックスに、VSTS アカウントにアクセスするための URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your VSTS account.</span></span>
1. <span data-ttu-id="80ef3-152">選択、 **Git**ラジオ ボタン、GitHub リポジトリでコードがホストされているためです。</span><span class="sxs-lookup"><span data-stu-id="80ef3-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="80ef3-153">**[Continue]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-153">Click the **Continue** button.</span></span> <span data-ttu-id="80ef3-154">少し待つ、アカウントとチーム プロジェクトの場合は、名前付き*MyFirstProject*が作成されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![VSTS アカウントの作成 ページ](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="80ef3-156">VSTS アカウントとプロジェクトが使用できることを示す確認の電子メールを開きます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-156">Open the confirmation email indicating that the VSTS account and project are ready for use.</span></span> <span data-ttu-id="80ef3-157">をクリックして、**プロジェクトを開始する**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-157">Click the **Start your project** button:</span></span>

    ![プロジェクト ボタンを開始します。](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="80ef3-159">ブラウザーに表示する *\<account_name\>. visualstudio.com*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="80ef3-160">をクリックして、 *MyFirstProject*プロジェクトの DevOps パイプラインの構成を開始するリンク。</span><span class="sxs-lookup"><span data-stu-id="80ef3-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-devops-pipeline"></a><span data-ttu-id="80ef3-161">DevOps パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-161">Configure the DevOps pipeline</span></span>

<span data-ttu-id="80ef3-162">完了する 3 つの異なる手順があります。</span><span class="sxs-lookup"><span data-stu-id="80ef3-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="80ef3-163">運用の DevOps パイプラインでは、次の 3 つのセクションでは結果の作業を完了します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-vsts-access-to-the-github-repository"></a><span data-ttu-id="80ef3-164">VSTS、GitHub リポジトリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-164">Grant VSTS access to the GitHub repository</span></span>

1. <span data-ttu-id="80ef3-165">展開、**外部リポジトリからコードをビルドまたは**アコーディオンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="80ef3-166">をクリックして、**セットアップ ビルド**ボタン。</span><span class="sxs-lookup"><span data-stu-id="80ef3-166">Click the **Setup Build** button:</span></span>

    ![ビルドのセットアップのボタン](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="80ef3-168">選択、 **GitHub**オプションを**ソースを選択して**セクション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![GitHub のソースを選択します。](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="80ef3-170">GitHub リポジトリを VSTS にアクセスできる前に、承認が必要です。</span><span class="sxs-lookup"><span data-stu-id="80ef3-170">Authorization is required before VSTS can access your GitHub repository.</span></span> <span data-ttu-id="80ef3-171">入力 *< GitHub_username > GitHub 接続*で、**接続名**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="80ef3-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="80ef3-172">例えば:</span><span class="sxs-lookup"><span data-stu-id="80ef3-172">For example:</span></span>

    ![GitHub 接続名](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="80ef3-174">GitHub アカウントで 2 要素認証が有効な場合は、個人用アクセス トークンが必要です。</span><span class="sxs-lookup"><span data-stu-id="80ef3-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="80ef3-175">この場合は、クリックして、 **GitHub 個人用アクセス トークンを使用して承認**リンク。</span><span class="sxs-lookup"><span data-stu-id="80ef3-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="80ef3-176">参照してください、 [GitHub 個人用アクセス トークンの作成手順については公式](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)ヘルプを参照します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="80ef3-177">のみ、*リポジトリ*アクセス許可のスコープが必要です。</span><span class="sxs-lookup"><span data-stu-id="80ef3-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="80ef3-178">それ以外の場合、をクリックして、 **OAuth を使用して承認**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="80ef3-179">メッセージが表示されたら、GitHub アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="80ef3-180">VSTS アカウントへのアクセスを許可する承認を選択します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-180">Then select Authorize to grant access to your VSTS account.</span></span> <span data-ttu-id="80ef3-181">成功した場合、新しいサービス エンドポイントが作成されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="80ef3-182">横にある省略記号ボタンをクリックして、**リポジトリ**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="80ef3-183">選択、 *< GitHub_username >/フィード リーダーの単純な*リストからのリポジトリ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="80ef3-184">をクリックして、**選択**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="80ef3-185">選択、*マスター*からブランチ、**手動スケジュール ビルドの既定のブランチ**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="80ef3-186">**[Continue]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-186">Click the **Continue** button.</span></span> <span data-ttu-id="80ef3-187">テンプレートの選択 ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="80ef3-188">ビルド定義を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-188">Create the build definition</span></span>

1. <span data-ttu-id="80ef3-189">テンプレートの選択 ページで、次のように入力します。 *ASP.NET Core*検索ボックスで。</span><span class="sxs-lookup"><span data-stu-id="80ef3-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![[テンプレート] ページの ASP.NET Core の検索](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="80ef3-191">テンプレートの検索結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-191">The template search results appear.</span></span> <span data-ttu-id="80ef3-192">ポインターを合わせる、 **ASP.NET Core**テンプレート、およびクリック、**適用**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="80ef3-193">**タスク**ビルド定義のタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="80ef3-194">**[トリガー]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="80ef3-195">チェック、**継続的インテグレーションを有効にする**ボックス。</span><span class="sxs-lookup"><span data-stu-id="80ef3-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="80ef3-196">下、**ブランチ フィルター**セクションで、いることを確認、**型**ドロップダウンに設定されている*Include*。</span><span class="sxs-lookup"><span data-stu-id="80ef3-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="80ef3-197">設定、**ブランチ仕様**ドロップダウンを*マスター*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![継続的インテグレーションの設定を有効にします。](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="80ef3-199">これらの設定により、ビルドに変更がプッシュされるときにトリガーする、*マスター* GitHub リポジトリの分岐。</span><span class="sxs-lookup"><span data-stu-id="80ef3-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="80ef3-200">継続的インテグレーションがテスト済みで、[を GitHub に変更をコミットし、自動的に Azure にデプロイ](#commit-changes-to-github-and-automatically-deploy-to-azure)セクション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="80ef3-201">をクリックして、**保存 & キュー**ボタンをクリックし、選択、**保存**オプション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![[保存] ボタン](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="80ef3-203">次のモーダル ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-203">The following modal dialog appears:</span></span>

    ![ビルド定義のモーダル ダイアログを保存します。](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="80ef3-205">既定のフォルダーを使用して、 *\\*、 をクリックし、**保存**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="80ef3-206">リリース パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-206">Create the release pipeline</span></span>

1. <span data-ttu-id="80ef3-207">をクリックして、**リリース**チーム プロジェクトのタブ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="80ef3-208">をクリックして、**新しいパイプライン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-208">Click the **New pipeline** button.</span></span>

    ![[リリース] タブの新しい定義のボタン](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="80ef3-210">テンプレートの選択 ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="80ef3-211">テンプレートの選択 ページで、次のように入力します。 *App Service*検索ボックスで。</span><span class="sxs-lookup"><span data-stu-id="80ef3-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![リリース パイプライン テンプレートの検索ボックス](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="80ef3-213">テンプレートの検索結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-213">The template search results appear.</span></span> <span data-ttu-id="80ef3-214">ポインターを合わせる、**スロットを使用した Azure App Service のデプロイ**テンプレート、およびクリック、**適用**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="80ef3-215">**パイプライン**リリース パイプラインのタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![リリース パイプラインのパイプライン タブ](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="80ef3-217">をクリックして、**追加**ボタン、**成果物**ボックス。</span><span class="sxs-lookup"><span data-stu-id="80ef3-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="80ef3-218">**成果物を追加**パネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-218">The **Add artifact** panel appears:</span></span>

    ![リリース パイプラインの成果物のパネルを追加](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="80ef3-220">選択、**ビルド**タイルから、**ソースの種類**セクション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="80ef3-221">この型は、ビルド定義、リリース パイプラインのリンクでできます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="80ef3-222">選択*MyFirstProject*から、**プロジェクト**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="80ef3-223">ビルド定義の名前を選択します。 *MyFirstProject ASP.NET Core-CI*、から、**ソース (ビルド定義)** ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="80ef3-224">選択*最新*から、**既定バージョン**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="80ef3-225">このオプションは、ビルド定義の最新の実行によって生成された成果物をビルドします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="80ef3-226">テキストを置き換える、**ソース エイリアス**を含む textbox*ドロップ*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="80ef3-227">**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-227">Click the **Add** button.</span></span> <span data-ttu-id="80ef3-228">**成果物**変更を表示する更新プログラムをセクションします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="80ef3-229">継続的なデプロイを有効にする稲妻のアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![リリース パイプラインの成果物の稲妻のアイコン](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="80ef3-231">このオプションを有効にすると、展開には、新しいビルドが利用可能な各時間に発生します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="80ef3-232">A**継続的配置トリガー**右側にパネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="80ef3-233">この機能を有効にするトグル ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="80ef3-234">有効にする必要はありません、**プル要求トリガー**します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="80ef3-235">をクリックして、**追加**ドロップダウンで、**ビルド ブランチ フィルター**セクション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="80ef3-236">選択、**ビルド定義の既定のブランチ**オプション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="80ef3-237">このフィルターによって、GitHub リポジトリからのビルドに対してのみトリガーするリリース*マスター*分岐します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="80ef3-238">**[保存]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-238">Click the **Save** button.</span></span> <span data-ttu-id="80ef3-239">をクリックして、 **OK** 、最終的なボタン**保存**モーダル ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="80ef3-240">をクリックして、**環境 1**ボックス。</span><span class="sxs-lookup"><span data-stu-id="80ef3-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="80ef3-241">**環境**右側にパネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="80ef3-242">変更、*環境 1*内のテキスト、**環境名**に textbox*運用*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![リリース パイプライン - 環境名 テキスト ボックス](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="80ef3-244">をクリックして、**フェーズ 1、2 つのタスク**のリンクを**運用**ボックス。</span><span class="sxs-lookup"><span data-stu-id="80ef3-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![リリース パイプラインの実稼働環境 link.png](media/cicd/vsts-production-link.png)

    <span data-ttu-id="80ef3-246">**タスク**環境のタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="80ef3-247">をクリックして、 **Azure App Service のデプロイ スロットを**タスク。</span><span class="sxs-lookup"><span data-stu-id="80ef3-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="80ef3-248">右側のパネルにその設定が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="80ef3-249">App Service に関連付けられている Azure サブスクリプションを選択して、 **Azure サブスクリプション**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="80ef3-250">選択すると、クリックして、 **Authorize**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="80ef3-251">選択*Web アプリ*から、**アプリの種類**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="80ef3-252">選択*mywebapp/< unique_number/>* から、 **App service の名前**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="80ef3-253">選択*azure チュートリアル*から、**リソース グループ**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="80ef3-254">選択*ステージング*から、**スロット**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="80ef3-255">**[保存]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="80ef3-256">既定のリリース パイプラインの名前を合わせます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="80ef3-257">編集することにある鉛筆アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="80ef3-258">使用*MyFirstProject ASP.NET Core-CD*名として。</span><span class="sxs-lookup"><span data-stu-id="80ef3-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![リリース パイプライン名](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="80ef3-260">**[保存]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="80ef3-261">GitHub に変更をコミットし、自動的に Azure にデプロイ</span><span class="sxs-lookup"><span data-stu-id="80ef3-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="80ef3-262">開いている*SimpleFeedReader.sln* Visual Studio でします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="80ef3-263">ソリューション エクスプ ローラーで開く*Pages\Index.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="80ef3-264">変更`<h2>Simple Feed Reader - V3</h2>`に`<h2>Simple Feed Reader - V4</h2>`します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="80ef3-265">キーを押して**Ctrl**+**Shift**+**B**アプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="80ef3-266">ファイルを GitHub リポジトリにコミットします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="80ef3-267">いずれかを使用して、**変更**Visual studio のページ*チーム エクスプ ローラー*タブ、または、次を実行、ローカル コンピューターのコマンド シェルの使用します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. <span data-ttu-id="80ef3-268">変更をプッシュ、*マスター*に分岐、*原点*GitHub リポジトリのリモート。</span><span class="sxs-lookup"><span data-stu-id="80ef3-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="80ef3-269">GitHub リポジトリのコミットが表示されます*マスター*ブランチ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![Master ブランチでの GitHub のコミット](media/cicd/github-commit.png)

    <span data-ttu-id="80ef3-271">ビルド定義の継続的な統合が有効になっているために、ビルドがトリガーされる**トリガー**  タブ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![継続的インテグレーションを有効にします。](media/cicd/enable-ci.png)

1. <span data-ttu-id="80ef3-273">移動し、**キューに登録済み**のタブ、**ビルドし、リリース** > **ビルド**vsts ページ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-273">Navigate to the **Queued** tab of the **Build and Release** > **Builds** page in VSTS.</span></span> <span data-ttu-id="80ef3-274">ブランチとコミットのビルドをトリガーしたキューに入っているビルドを示しています。</span><span class="sxs-lookup"><span data-stu-id="80ef3-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![キューに入っているビルド](media/cicd/build-queued.png)

1. <span data-ttu-id="80ef3-276">ビルドに成功すると、Azure にデプロイが発生します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="80ef3-277">ブラウザーでアプリに移動します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="80ef3-278">"V4"テキストが、見出しが表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80ef3-278">Notice that the "V4" text appears in the heading:</span></span>

    ![更新されたアプリ](media/cicd/updated-app-v4.png)

## <a name="examine-the-vsts-devops-pipeline"></a><span data-ttu-id="80ef3-280">VSTS の DevOps パイプラインを確認します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-280">Examine the VSTS DevOps pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="80ef3-281">ビルド定義</span><span class="sxs-lookup"><span data-stu-id="80ef3-281">Build definition</span></span>

<span data-ttu-id="80ef3-282">ビルド定義を名前で作成された*MyFirstProject ASP.NET Core-CI*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="80ef3-283">完了したら、ビルドの生成、 *.zip*発行する資産を含むファイル。</span><span class="sxs-lookup"><span data-stu-id="80ef3-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="80ef3-284">リリース パイプラインでは、これらの資産を Azure に配置します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="80ef3-285">ビルド定義の**タスク**タブには、使用されている個々 のステップが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="80ef3-286">5 つのビルド タスクがあります。</span><span class="sxs-lookup"><span data-stu-id="80ef3-286">There are five build tasks.</span></span>

![ビルド定義タスク](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="80ef3-288">**復元**&mdash;実行、`dotnet restore`アプリの NuGet パッケージを復元するコマンド。</span><span class="sxs-lookup"><span data-stu-id="80ef3-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="80ef3-289">既定のパッケージ フィードの使用は nuget.org します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="80ef3-290">**ビルド**&mdash;実行、`dotnet build --configuration release`アプリのコードをコンパイルするコマンド。</span><span class="sxs-lookup"><span data-stu-id="80ef3-290">**Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code.</span></span> <span data-ttu-id="80ef3-291">これは、`--configuration`オプションは、運用環境に展開に適していると、コードの最適化のバージョンを生成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="80ef3-292">変更、 *BuildConfiguration* 、ビルド定義の変数**変数** タブの場合、たとえば、デバッグ構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="80ef3-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="80ef3-293">**テスト**&mdash;実行、`dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>`アプリの単体テストを実行するコマンド。</span><span class="sxs-lookup"><span data-stu-id="80ef3-293">**Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests.</span></span> <span data-ttu-id="80ef3-294">単体テストは、すべて c# プロジェクトに一致する内部で実行される、 `**/*Tests/*.csproj` glob パターン。</span><span class="sxs-lookup"><span data-stu-id="80ef3-294">Unit tests are executed within any C# project matching the `**/*Tests/*.csproj` glob pattern.</span></span> <span data-ttu-id="80ef3-295">テスト結果が保存、 *.trx*によって指定された場所にファイル、`--results-directory`オプション。</span><span class="sxs-lookup"><span data-stu-id="80ef3-295">Test results are saved in a *.trx* file at the location specified by the `--results-directory` option.</span></span> <span data-ttu-id="80ef3-296">すべてのテストが失敗した場合、ビルドは失敗しは展開されません。</span><span class="sxs-lookup"><span data-stu-id="80ef3-296">If any tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="80ef3-297">テスト作業の単位を確認するには、変更*SimpleFeedReader.Tests\Services\NewsServiceTests.cs*テストの 1 つを意図的に分割します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-297">To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests.</span></span> <span data-ttu-id="80ef3-298">たとえば、変更`Assert.True(result.Count > 0);`に`Assert.False(result.Count > 0);`で、`Returns_News_Stories_Given_Valid_Uri`メソッド。</span><span class="sxs-lookup"><span data-stu-id="80ef3-298">For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method.</span></span> <span data-ttu-id="80ef3-299">コミットし、GitHub に変更をプッシュします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-299">Commit and push the change to GitHub.</span></span> <span data-ttu-id="80ef3-300">ビルドがトリガーされ、失敗します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-300">The build is triggered and fails.</span></span> <span data-ttu-id="80ef3-301">ビルド パイプラインの状態に変わります**失敗**します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-301">The build pipeline status changes to **failed**.</span></span> <span data-ttu-id="80ef3-302">変更、commit、およびプッシュを再び元に戻します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-302">Revert the change, commit, and push again.</span></span> <span data-ttu-id="80ef3-303">ビルドは成功します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-303">The build succeeds.</span></span>

1. <span data-ttu-id="80ef3-304">**発行**&mdash;実行、`dotnet publish --configuration release --output <local_path_on_build_agent>`を生成するコマンド、 *.zip*デプロイするアーティファクトを含むファイル。</span><span class="sxs-lookup"><span data-stu-id="80ef3-304">**Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="80ef3-305">`--output`の発行場所を指定するオプション、 *.zip*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80ef3-305">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="80ef3-306">渡すことによって場所が指定されている、[定義済み変数](https://docs.microsoft.com/vsts/pipelines/build/variables)という`$(build.artifactstagingdirectory)`します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-306">That location is specified by passing a [predefined variable](https://docs.microsoft.com/vsts/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="80ef3-307">など、ローカル パスにその変数を展開*c:\agent\_work\1\a*、ビルド エージェントにします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-307">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="80ef3-308">**成果物の公開** &mdash; Publishes、 *.zip*で生成されるファイル、**発行**タスク。</span><span class="sxs-lookup"><span data-stu-id="80ef3-308">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="80ef3-309">タスクを受け入れる、 *.zip*ファイルの場所、定義済みの変数をパラメーターとして`$(build.artifactstagingdirectory)`します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-309">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="80ef3-310">*.Zip*という名前のフォルダーとファイルをパブリッシュ*ドロップ*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-310">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="80ef3-311">ビルド定義のクリックして**概要**定義と共にビルドの履歴を表示するリンク。</span><span class="sxs-lookup"><span data-stu-id="80ef3-311">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![ビルド定義の履歴](media/cicd/build-definition-summary.png)

<span data-ttu-id="80ef3-313">[結果] ページで、一意のビルド番号に対応するリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-313">On the resulting page, click the link corresponding to the unique build number:</span></span>

![ビルド定義の概要 ページ](media/cicd/build-definition-completed.png)

<span data-ttu-id="80ef3-315">この特定のビルドの概要が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-315">A summary of this specific build is displayed.</span></span> <span data-ttu-id="80ef3-316">をクリックして、**成果物** タブに注意してください、*ドロップ*ビルドによって生成されたフォルダーが一覧表示。</span><span class="sxs-lookup"><span data-stu-id="80ef3-316">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![ビルド定義の成果物のドロップ フォルダー](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="80ef3-318">使用して、**ダウンロード**と**探索**へのリンクを発行された成果物を検査します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-318">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="80ef3-319">リリース パイプライン</span><span class="sxs-lookup"><span data-stu-id="80ef3-319">Release pipeline</span></span>

<span data-ttu-id="80ef3-320">リリース パイプラインは、名前で作成された*MyFirstProject ASP.NET Core-CD*:</span><span class="sxs-lookup"><span data-stu-id="80ef3-320">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![リリース パイプラインの概要](media/cicd/release-definition-overview.png)

<span data-ttu-id="80ef3-322">リリース パイプラインの 2 つの主要なコンポーネントは、**成果物**と**環境**します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-322">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="80ef3-323">ボックスをクリックすると、**成果物**セクションが次のパネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-323">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![リリース パイプラインの成果物](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="80ef3-325">**ソース (ビルド定義)** 値は、このリリースのパイプラインがリンクされているビルド定義を表します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-325">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="80ef3-326">*.Zip*ビルド定義が正常に実行によって生成されたファイルが提供されます、*運用*環境を Azure に配置します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-326">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="80ef3-327">をクリックして、*フェーズ 1、2 つのタスク*のリンクを*運用*環境のボックスに、リリース パイプラインのタスクを表示。</span><span class="sxs-lookup"><span data-stu-id="80ef3-327">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![リリース パイプラインのタスク](media/cicd/release-definition-tasks.png)

<span data-ttu-id="80ef3-329">リリース パイプラインは、2 つのタスクで構成されています: *Azure App Service のデプロイ スロットを*と*管理 Azure App Service のスロット スワップ*します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-329">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="80ef3-330">最初のタスクをクリックすると、次のタスクの構成が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-330">Clicking the first task reveals the following task configuration:</span></span>

![リリース パイプラインの展開タスク](media/cicd/release-definition-task1.png)

<span data-ttu-id="80ef3-332">Azure サブスクリプション、サービスの種類、web アプリ名、リソース グループ、およびデプロイ スロットは、デプロイ タスクで定義されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-332">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="80ef3-333">**パッケージまたはフォルダー**を保持するテキスト ボックス、 *.zip*を抽出して展開するファイルのパス、*ステージング*のスロット、 *mywebapp\<一意数 (_n)\>*  web アプリ。</span><span class="sxs-lookup"><span data-stu-id="80ef3-333">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="80ef3-334">スロット スワップのタスクをクリックすると、次のタスクの構成が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-334">Clicking the slot swap task reveals the following task configuration:</span></span>

![リリース パイプラインのスロット スワップ タスク](media/cicd/release-definition-task2.png)

<span data-ttu-id="80ef3-336">サブスクリプション、リソース グループ、サービスの種類、web アプリ名、およびデプロイ スロットの詳細が提供されます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-336">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="80ef3-337">**実稼働とスワップ**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-337">The **Swap with Production** checkbox is checked.</span></span> <span data-ttu-id="80ef3-338">その結果、展開、bits、*ステージング*スロットが運用環境にスワップされます。</span><span class="sxs-lookup"><span data-stu-id="80ef3-338">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="80ef3-339">その他の参考資料</span><span class="sxs-lookup"><span data-stu-id="80ef3-339">Additional reading</span></span>

* [<span data-ttu-id="80ef3-340">ASP.NET Core アプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="80ef3-340">Build your ASP.NET Core app</span></span>](https://docs.microsoft.com/vsts/build-release/apps/aspnet/build-aspnet-core)
* [<span data-ttu-id="80ef3-341">Azure Web アプリをビルドしてデプロイ</span><span class="sxs-lookup"><span data-stu-id="80ef3-341">Build and deploy to an Azure Web App</span></span>](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp)
* [<span data-ttu-id="80ef3-342">GitHub リポジトリの CI ビルド プロセスを定義します。</span><span class="sxs-lookup"><span data-stu-id="80ef3-342">Define a CI build process for your GitHub repository</span></span>](https://docs.microsoft.com/vsts/pipelines/build/ci-build-github)