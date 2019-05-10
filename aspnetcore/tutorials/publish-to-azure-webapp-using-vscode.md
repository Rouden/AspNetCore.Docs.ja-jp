---
title: Visual Studio Code で ASP.NET Core アプリを Azure に公開する
author: ricardoserradas
description: Visual Studio Code を使用して Azure App Service に ASP.NET Core アプリを発行する方法を説明します。
ms.author: ricardoserradas
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 64d82835f6a47a458802692c99658b964c07f807
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59711299"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a><span data-ttu-id="6ea3d-103">Visual Studio Code で ASP.NET Core アプリを Azure に公開する</span><span class="sxs-lookup"><span data-stu-id="6ea3d-103">Publish an ASP.NET Core app to Azure with Visual Studio Code</span></span>

<span data-ttu-id="6ea3d-104">投稿者: [Ricardo Serradas](https://twitter.com/ricardoserradas)</span><span class="sxs-lookup"><span data-stu-id="6ea3d-104">By [Ricardo Serradas](https://twitter.com/ricardoserradas)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="6ea3d-105">App Service の配置に関する問題を解決するには、「<xref:host-and-deploy/azure-apps/troubleshoot>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-105">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="intro"></a><span data-ttu-id="6ea3d-106">イントロ</span><span class="sxs-lookup"><span data-stu-id="6ea3d-106">Intro</span></span>

<span data-ttu-id="6ea3d-107">このチュートリアルでは、ASP.Net Core MVC アプリケーションを作成し、Visual Studio Code 内にそれをデプロイする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-107">With this tutorial, you'll learn how to create an ASP.Net Core MVC Application and deploy it within Visual Studio Code.</span></span>

## <a name="set-up"></a><span data-ttu-id="6ea3d-108">設定</span><span class="sxs-lookup"><span data-stu-id="6ea3d-108">Set up</span></span>

- <span data-ttu-id="6ea3d-109">Azure アカウントをお持ちでない場合は、[Azure 無料アカウント](https://azure.microsoft.com/free/dotnet/)を作成します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-109">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span>
- <span data-ttu-id="6ea3d-110">[.NET Core SDK](https://dotnet.microsoft.com/download) をインストールします</span><span class="sxs-lookup"><span data-stu-id="6ea3d-110">Install [.NET Core SDK](https://dotnet.microsoft.com/download)</span></span>
- <span data-ttu-id="6ea3d-111">[Visual Studio Code](https://code.visualstudio.com/Download) をインストールします</span><span class="sxs-lookup"><span data-stu-id="6ea3d-111">Install [Visual Studio Code](https://code.visualstudio.com/Download)</span></span>
  - <span data-ttu-id="6ea3d-112">Visual Studio Code の [C# 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)をインストールします</span><span class="sxs-lookup"><span data-stu-id="6ea3d-112">Install the [C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to Visual Studio Code</span></span>
  - <span data-ttu-id="6ea3d-113">Visual Studio Code の [Azure App Service 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)をインストールし、構成してから先に進みます</span><span class="sxs-lookup"><span data-stu-id="6ea3d-113">Instal the [Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) to Visual Studio Code and configure it before proceeding</span></span>

## <a name="create-an-aspnet-core-mvc-project"></a><span data-ttu-id="6ea3d-114">ASP.NET Core MVC プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="6ea3d-114">Create an ASP.Net Core MVC project</span></span>

<span data-ttu-id="6ea3d-115">ターミナルを使用し、プロジェクトを作成するフォルダーに移動し、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-115">Using a terminal, navigate to the folder you want the project to be created on and use the following command:</span></span>

```cmd
> dotnet new mvc
```

<span data-ttu-id="6ea3d-116">フォルダーの構造は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-116">You'll have a folder structure similar to the following:</span></span>

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a><span data-ttu-id="6ea3d-117">Visual Studio Code でプロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="6ea3d-117">Open it with Visual Studio Code</span></span>

<span data-ttu-id="6ea3d-118">プロジェクトが作成されたら、次のいずれかの方法を利用し、Visual Studio Code でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-118">After your project is created, you can open it with Visual Studio Code by using one of the options below:</span></span>

### <a name="through-the-command-line"></a><span data-ttu-id="6ea3d-119">コマンド ラインから</span><span class="sxs-lookup"><span data-stu-id="6ea3d-119">Through the command line</span></span>

<span data-ttu-id="6ea3d-120">プロジェクトを作成したフォルダー内で次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-120">Use the following command within the folder you created the project:</span></span>

```cmd
> code .
```

<span data-ttu-id="6ea3d-121">下のコマンドが機能しない場合、[このリンク](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform)を参照し、インストールが正しく構成されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-121">If the command below does not work, check if your installation is configured properly by referencing [this link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span></span>

### <a name="through-visual-studio-code-interface"></a><span data-ttu-id="6ea3d-122">Visual Studio Code インターフェイスから</span><span class="sxs-lookup"><span data-stu-id="6ea3d-122">Through Visual Studio Code interface</span></span>

- <span data-ttu-id="6ea3d-123">Visual Studio Code を開きます</span><span class="sxs-lookup"><span data-stu-id="6ea3d-123">Open Visual Studio Code</span></span>
- <span data-ttu-id="6ea3d-124">メニューで `File > Open Folder` を選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-124">On the menu, select `File > Open Folder`</span></span>
- <span data-ttu-id="6ea3d-125">MVC プロジェクトを作成したフォルダーのルートを選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-125">Select the root of the folder you created the MVC Project</span></span>

<span data-ttu-id="6ea3d-126">プロジェクト フォルダーを開くと、ビルドやデバッグに必要なアセットが足りないというメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-126">When you open the project folder, you'll receive a message saying that required assets to build and debug are missing.</span></span> <span data-ttu-id="6ea3d-127">[はい] を選択し、アセットを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-127">Accept the help to add them.</span></span>

![プロジェクトが読み込まれた Visual Studio Code インターフェイス](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

<span data-ttu-id="6ea3d-129">`.vscode` フォルダーがプロジェクト構造の下に作成されます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-129">A `.vscode` folder will be created under the project structure.</span></span> <span data-ttu-id="6ea3d-130">これには次のファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-130">It will contain the following files:</span></span>

```cmd
launch.json
tasks.json
```

<span data-ttu-id="6ea3d-131">.NET Core Web アプリのビルドとデバッグに役立つユーティリティ ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-131">These are utility files to help you build and debug your .NET Core Web App.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6ea3d-132">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="6ea3d-132">Run the app</span></span>

<span data-ttu-id="6ea3d-133">アプリを Azure にデプロイする前に、ローカル コンピューターでアプリが正しく実行されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-133">Before we deploy the app to Azure, make sure it is running properly on your local machine.</span></span>

- <span data-ttu-id="6ea3d-134">F5 を押し、プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-134">Press F5 to run the project</span></span>

<span data-ttu-id="6ea3d-135">既定のブラウザーの新しいタブで Web アプリが実行を開始します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-135">Your web app will start running on a new tab of your default browser.</span></span> <span data-ttu-id="6ea3d-136">起動の直後にプライバシーに関する警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-136">You may notice a privacy warning as soon as it starts.</span></span> <span data-ttu-id="6ea3d-137">これはアプリが HTTP と HTTPS のいずれかで起動するためです。既定では HTTPS エンドポイントに移動します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-137">This is because your app will start either using HTTP and HTTPS, and it navigates to the HTTPS endpoint by default.</span></span>

![アプリをローカルでデバッグするときに表示されるプライバシーに関する警告](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

<span data-ttu-id="6ea3d-139">デバッグ セッションを維持するには、`Advanced` をクリックし、次に `Continue to localhost (unsafe)` をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-139">To keep the debugging session, click `Advanced` and then `Continue to localhost (unsafe)`.</span></span>

## <a name="generate-the-deployment-package-locally"></a><span data-ttu-id="6ea3d-140">デプロイ パッケージをローカルで生成する</span><span class="sxs-lookup"><span data-stu-id="6ea3d-140">Generate the deployment package locally</span></span>

- <span data-ttu-id="6ea3d-141">Visual Studio Code ターミナルを開きます</span><span class="sxs-lookup"><span data-stu-id="6ea3d-141">Open Visual Studio Code terminal</span></span>
- <span data-ttu-id="6ea3d-142">次のコマンドを使用し、`publish` という名前のサブフォルダーに `Release` パッケージを生成します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-142">Use the following command to generate a `Release` package to a sub folder called `publish`:</span></span>
  - `dotnet publish -c Release -o ./publish`
- <span data-ttu-id="6ea3d-143">新しい `publish` フォルダーがプロジェクト構造の下に作成されます</span><span class="sxs-lookup"><span data-stu-id="6ea3d-143">A new `publish` folder will be created under the project structure</span></span>

![発行フォルダーの構造](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a><span data-ttu-id="6ea3d-145">Azure App Service に発行する</span><span class="sxs-lookup"><span data-stu-id="6ea3d-145">Publish to Azure App Service</span></span>

<span data-ttu-id="6ea3d-146">Visual Studio Code 用の Azure App Service 拡張機能を活用し、下の手順で Azure App Service に直接、Web サイトを発行します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-146">Leveraging the Azure App Service extension for Visual Studio Code, follow the steps below to publish the website directly to the Azure App Service.</span></span>

### <a name="if-youre-creating-a-new-web-app"></a><span data-ttu-id="6ea3d-147">新しい Web アプリを作成する場合</span><span class="sxs-lookup"><span data-stu-id="6ea3d-147">If you're creating a new Web App</span></span>

- <span data-ttu-id="6ea3d-148">`publish` フォルダーを右クリックし、`Deploy to Web App...` を選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-148">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="6ea3d-149">Web アプリを作成するサブスクリプションを選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-149">Select the subscription you want to create the Web App</span></span>
- <span data-ttu-id="6ea3d-150">`Create New Web App` を選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-150">Select `Create New Web App`</span></span>
- <span data-ttu-id="6ea3d-151">Web アプリの名前を入力します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-151">Enter a name for the Web App</span></span>

<span data-ttu-id="6ea3d-152">拡張機能により新しい Web アプリが作成され、それにパッケージが自動的にデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-152">The extension will create the new Web App and will automatically start deploying the package to it.</span></span> <span data-ttu-id="6ea3d-153">デプロイが完了したら、`Browse Website` をクリックし、デプロイの有効性を検証します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-153">Once the deployment is finished, click `Browse Website` to validate the deployment.</span></span>

![デプロイが成功したときのメッセージ](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

<span data-ttu-id="6ea3d-155">`Browse Website` をクリックすると、既定のブラウザーでそこに移動します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-155">Once you click `Browse Website`, you'll navigate to it using your default browser:</span></span>

![新しい Web アプリが正常にデプロイされました](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a><span data-ttu-id="6ea3d-157">既存の Web アプリにデプロイする場合</span><span class="sxs-lookup"><span data-stu-id="6ea3d-157">If you're deploying to an existing Web App</span></span>

- <span data-ttu-id="6ea3d-158">`publish` フォルダーを右クリックし、`Deploy to Web App...` を選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-158">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="6ea3d-159">既存の Web アプリが存在するサブスクリプションを選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-159">Select the subscription the existing Web App resides</span></span>
- <span data-ttu-id="6ea3d-160">一覧から Web アプリを選択します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-160">Select the Web App from the list</span></span>
- <span data-ttu-id="6ea3d-161">Visual Studio Code から、既存のコンテンツを上書きするかどうかたずねられます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-161">Visual Studio Code will ask you if you want to overwrite the existing content.</span></span> <span data-ttu-id="6ea3d-162">`Deploy` をクリックして確定します</span><span class="sxs-lookup"><span data-stu-id="6ea3d-162">Click `Deploy` to confirm</span></span>

<span data-ttu-id="6ea3d-163">拡張機能により、更新後のコンテンツが Web アプリにデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-163">The extension will deploy the updated content to the Web App.</span></span> <span data-ttu-id="6ea3d-164">完了したら、`Browse Website` をクリックし、デプロイの有効性を検証します。</span><span class="sxs-lookup"><span data-stu-id="6ea3d-164">Once it's done, click `Browse Website` to validate the deployment.</span></span>

![既存の Web アプリが正常にデプロイされました](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a><span data-ttu-id="6ea3d-166">次の手順</span><span class="sxs-lookup"><span data-stu-id="6ea3d-166">Next steps</span></span>

- [<span data-ttu-id="6ea3d-167">最初の Azure DevOps パイプラインを作成する</span><span class="sxs-lookup"><span data-stu-id="6ea3d-167">Create your first Azure DevOps pipeline</span></span>](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a><span data-ttu-id="6ea3d-168">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6ea3d-168">Additional resources</span></span>

- [<span data-ttu-id="6ea3d-169">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6ea3d-169">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
- [<span data-ttu-id="6ea3d-170">Azure リソース グループ</span><span class="sxs-lookup"><span data-stu-id="6ea3d-170">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)