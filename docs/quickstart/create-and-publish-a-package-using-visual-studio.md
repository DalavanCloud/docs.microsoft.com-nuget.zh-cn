---
title: 使用 Visual Studio 创建和发布 .NET Standard NuGet 包的介绍性指南
description: 使用 Visual Studio 2017 创建和发布 .NET Standard NuGet 包的演练教程。
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 03/18/2018
ms.topic: quickstart
ms.openlocfilehash: c5d58aa6312eae801607ca44a81bc092a7a7c15f
ms.sourcegitcommit: a6ca160b1e7e5c58b135af4eba0e9463127a59e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-and-publish-a-nuget-package-using-visual-studio-net-standard"></a><span data-ttu-id="6c342-103">快速入门：使用 Visual Studio 创建和发布 NuGet 包 (.NET Standard)</span><span class="sxs-lookup"><span data-stu-id="6c342-103">Quickstart: Create and publish a NuGet package using Visual Studio (.NET Standard)</span></span>

<span data-ttu-id="6c342-104">从 Visual Studio 的 .NET Standard 类库创建 NuGet 包，然后使用 CLI 工具将其发布到 nuget.org，这是一个很简单的过程。</span><span class="sxs-lookup"><span data-stu-id="6c342-104">It's a simple process to create a NuGet package from a .NET Standard Class Library in Visual Studio, and then publish it to nuget.org using a CLI tool.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c342-105">系统必备</span><span class="sxs-lookup"><span data-stu-id="6c342-105">Prerequisites</span></span>

1. <span data-ttu-id="6c342-106">通过任何与 .NET 相关的工作负载从 [visualstudio.com](https://www.visualstudio.com/) 安装任意版本的 Visual Studio 2017。</span><span class="sxs-lookup"><span data-stu-id="6c342-106">Install any edition of Visual Studio 2017 from [visualstudio.com](https://www.visualstudio.com/) with any .NET-related workload.</span></span> <span data-ttu-id="6c342-107">安装 .NET 工作负载时，Visual Studio 2017 会自动包含 NuGet 功能。</span><span class="sxs-lookup"><span data-stu-id="6c342-107">Visual Studio 2017 automatically includes NuGet capabilities when a .NET workload is installed.</span></span>

1. <span data-ttu-id="6c342-108">要安装 `nuget.exe` CLI，从 [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) 下载它，将 `.exe` 文件保存到合适的文件夹，然后将该文件夹添加到 PATH 环境变量中。</span><span class="sxs-lookup"><span data-stu-id="6c342-108">Install the `nuget.exe` CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe), saving that `.exe` file to a suitable folder, and adding that folder to your PATH environment variable.</span></span>

    <span data-ttu-id="6c342-109">或者，如果安装了 [.NET Core SDK](https://www.microsoft.com/net/download/)，则可以使用 `dotnet` CLI。</span><span class="sxs-lookup"><span data-stu-id="6c342-109">Alternately, if you have the [.NET Core SDK](https://www.microsoft.com/net/download/) installed, you can use the `dotnet` CLI.</span></span>

1. <span data-ttu-id="6c342-110">如果你还没有帐户，请[在 nuget.org 上注册一个免费帐户](https://www.nuget.org/users/account/LogOn?returnUrl=%2F)。</span><span class="sxs-lookup"><span data-stu-id="6c342-110">[Register for a free account on nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) if you don't have one already.</span></span> <span data-ttu-id="6c342-111">创建新帐户会发送确认电子邮件。</span><span class="sxs-lookup"><span data-stu-id="6c342-111">Creating a new account sends a confirmation email.</span></span> <span data-ttu-id="6c342-112">必须先确认该帐户，才能上传包。</span><span class="sxs-lookup"><span data-stu-id="6c342-112">You must confirm the account before you can upload a package.</span></span>

## <a name="create-a-class-library-project"></a><span data-ttu-id="6c342-113">创建类库项目</span><span class="sxs-lookup"><span data-stu-id="6c342-113">Create a class library project</span></span>

<span data-ttu-id="6c342-114">可以使用现有的 .NET Standard 类库项目用于要打包的代码，或者创建一个简单的项目，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6c342-114">You can use an existing .NET Standard Class Library project for the code you want to package, or create a simple one as follows:</span></span>

1. <span data-ttu-id="6c342-115">在 Visual Studio 中，选择“文件”>“新建”>“项目”，展开“Visual C# > .NET Standard”节点，选择“类库 (.NET Standard)”模板，将项目命名为“AppLogger”，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="6c342-115">In Visual Studio, choose **File > New > Project**, expand the **Visual C# > .NET Standard** node, select the "Class Library (.NET Standard)" template, name the project AppLogger, and click **OK**.</span></span>

1. <span data-ttu-id="6c342-116">右键单击生成的项目文件并选择“生成”，确保已正确创建项目。</span><span class="sxs-lookup"><span data-stu-id="6c342-116">Right-click on the resulting project file and select **Build** to make sure the project was created properly.</span></span> <span data-ttu-id="6c342-117">DLL 位于调试文件夹中（或发布中，如果生成的是该配置）。</span><span class="sxs-lookup"><span data-stu-id="6c342-117">The DLL is found within the Debug folder (or Release if you build that configuration instead).</span></span>

<span data-ttu-id="6c342-118">当然，在实际的 NuGet 包中，可实现许多有用的功能，让其他人可通过这些功能生成应用程序。</span><span class="sxs-lookup"><span data-stu-id="6c342-118">Within a real NuGet package, of course, you implement many useful features with which others can build applications.</span></span> <span data-ttu-id="6c342-119">但是对于本演练，无需编写其他任何代码，因为模板的类库足以创建包。</span><span class="sxs-lookup"><span data-stu-id="6c342-119">For this walkthrough, however, you won't write any additional code because a class library from the template is sufficient to create a package.</span></span> <span data-ttu-id="6c342-120">但是，如果你需要此程序包的某个功能代码，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="6c342-120">Still, if you'd like some functional code for the package, use the following:</span></span>

```cs
namespace AppLogger
{
    public class Logger
    {
        public void Log(string text)
        {
            Console.WriteLine(text);
        }
    }
}
```

> [!Tip]
> <span data-ttu-id="6c342-121">除非你有其他选择理由，否则 .NET Standard 是 NuGet 包的首选目标，因为它提供了与最广泛的使用项目的兼容性。</span><span class="sxs-lookup"><span data-stu-id="6c342-121">Unless you have a reason to choose otherwise, .NET Standard is the preferred target for NuGet packages, as it provides compatibility with the widest range of consuming projects.</span></span>

## <a name="configure-package-properties"></a><span data-ttu-id="6c342-122">配置包属性</span><span class="sxs-lookup"><span data-stu-id="6c342-122">Configure package properties</span></span>

1. <span data-ttu-id="6c342-123">选择“项目”>“属性”菜单命令，然后选择“包”选项卡。（“包”选项卡仅出现在 .NET Standard 类库项目中；如果要面向 .NET Framework，请参阅[创建和发布 .NET Framework 包](create-and-publish-a-package-using-visual-studio-net-framework.md)。</span><span class="sxs-lookup"><span data-stu-id="6c342-123">Select the **Project > Properties** menu command, then select the **Package** tab. (The **Package** tab appears only for .NET Standard class library projects; if you are targeting .NET Framework, see [Create and publish a .NET Framework package](create-and-publish-a-package-using-visual-studio-net-framework.md) instead.</span></span> <span data-ttu-id="6c342-124">如果未出现在 .NET Standard 项目中，可能需要将 Visual Studio 2017 更新到最新版本。）</span><span class="sxs-lookup"><span data-stu-id="6c342-124">If it doesn't appear for a .NET Standard project, you may need to update Visual Studio 2017 to the latest release.)</span></span>

    ![Visual Studio 项目中的 NuGet 包属性](media/qs_create-vs-01-package-properties.png)

    > [!Note]
    > <span data-ttu-id="6c342-126">对于面向公共使用而生成的包，请特别注意 **Tags** 属性，因为这些标记可帮助其他人查找包并了解其用途。</span><span class="sxs-lookup"><span data-stu-id="6c342-126">For packages built for public consumption, pay special attention to the **Tags** property, as tags help others find your package and understand what it does.</span></span>

1. <span data-ttu-id="6c342-127">为包提供一个唯一标识符，并填写任何其他所需的属性。</span><span class="sxs-lookup"><span data-stu-id="6c342-127">Give your package a unique identifier and fill out any other desired properties.</span></span> <span data-ttu-id="6c342-128">有关不同属性的说明，请参阅 [.nuspec 文件引用](../reference/nuspec.md)。</span><span class="sxs-lookup"><span data-stu-id="6c342-128">For a description of the different properties, see [.nuspec file reference](../reference/nuspec.md).</span></span> <span data-ttu-id="6c342-129">这里的所有属性都列入 Visual Studio 为项目创建的 `.nuspec` 清单。</span><span class="sxs-lookup"><span data-stu-id="6c342-129">All of the properties here go into the `.nuspec` manifest that Visual Studio creates for the project.</span></span>

    > [!Important]
    > <span data-ttu-id="6c342-130">你必须为包提供一个在 nuget.org 中唯一或你使用的任何主机的标识符。</span><span class="sxs-lookup"><span data-stu-id="6c342-130">You must give the package an identifier that's unique across nuget.org or whatever host you're using.</span></span> <span data-ttu-id="6c342-131">对于本次演练，我们建议在名称中包含“Sample”或“Test”，因为稍后的发布步骤确实会使该包公开显示（尽管实际上不太可能有人会使用它）。</span><span class="sxs-lookup"><span data-stu-id="6c342-131">For this walkthrough we recommend including "Sample" or "Test" in the name as the later publishing step does make the package publicly visible (though it's unlikely anyone will actually use it).</span></span>
    >
    > <span data-ttu-id="6c342-132">如果你尝试发布名称已存在的包，则会看到一个错误。</span><span class="sxs-lookup"><span data-stu-id="6c342-132">If you attempt to publish a package with a name that already exists, you see an error.</span></span>

1. <span data-ttu-id="6c342-133">可选：若要直接查看项目文件中的属性，请右击“解决方案资源管理器”中的“项目”，然后选择“编辑 AppLogger.csproj”。</span><span class="sxs-lookup"><span data-stu-id="6c342-133">Optional: to see the properties directly in the project file, right-click the project in Solution Explorer and select **Edit AppLogger.csproj**.</span></span>

## <a name="run-the-pack-command"></a><span data-ttu-id="6c342-134">运行 pack 命令</span><span class="sxs-lookup"><span data-stu-id="6c342-134">Run the pack command</span></span>

1. <span data-ttu-id="6c342-135">将此配置设置为“发布”。</span><span class="sxs-lookup"><span data-stu-id="6c342-135">Set the configuration to **Release**.</span></span>

1. <span data-ttu-id="6c342-136">请在“解决方案资源管理器”中右键单击该项目，然后选择“Pack”命令：</span><span class="sxs-lookup"><span data-stu-id="6c342-136">Right click the project in **Solution Explorer** and select the **Pack** command:</span></span>

    ![Visual Studio 项目上下文菜单上的 NuGet pack 命令](media/qs_create-vs-02-pack-command.png)

1. <span data-ttu-id="6c342-138">Visual Studio 构建项目并创建 `.nupkg` 文件。</span><span class="sxs-lookup"><span data-stu-id="6c342-138">Visual Studio builds the project and creates the `.nupkg` file.</span></span> <span data-ttu-id="6c342-139">检查“输出”窗口以查看详细信息（类似于以下内容），其中包含包文件的路径。</span><span class="sxs-lookup"><span data-stu-id="6c342-139">Examine the **Output** window for details (similar to the following), which contains the path to the package file.</span></span> <span data-ttu-id="6c342-140">另请注意，生成的程序集位于适合 .NET Standard 2.0 目标的 `bin\Release\netstandard2.0` 中。</span><span class="sxs-lookup"><span data-stu-id="6c342-140">Note also that the built assembly is in `bin\Release\netstandard2.0` as befits the .NET Standard 2.0 target.</span></span>

    ```output
    1>------ Build started: Project: AppLogger, Configuration: Release Any CPU ------
    1>AppLogger -> d:\proj\AppLogger\AppLogger\bin\Release\netstandard2.0\AppLogger.dll
    1>Successfully created package 'd:\proj\AppLogger\AppLogger\bin\Release\AppLogger.1.0.0.nupkg'.
    ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
    ```

### <a name="alternate-option-pack-with-msbuild"></a><span data-ttu-id="6c342-141">备用选项：使用 MSBuild 打包</span><span class="sxs-lookup"><span data-stu-id="6c342-141">Alternate option: pack with MSBuild</span></span>

<span data-ttu-id="6c342-142">作为使用“打包”菜单命令的备选项，当项目包含必要的包数据时，NuGet 4.x+ 和 MSBuild 15.1+ 支持 `pack` 目标。</span><span class="sxs-lookup"><span data-stu-id="6c342-142">As an alternate to using the **Pack** menu command, NuGet 4.x+ and MSBuild 15.1+ supports a `pack` target when the project contains the necessary package data.</span></span> <span data-ttu-id="6c342-143">打开命令提示符，导航到项目文件夹并运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="6c342-143">Open a command prompt, navigate to your project folder and run the following command.</span></span> <span data-ttu-id="6c342-144">（用户通常习惯从“开始”菜单中启动“适用于 Visual Studio 的开发人员命令提示符”，因为它将使用 MSBuild 的所有必需路径进行配置。）</span><span class="sxs-lookup"><span data-stu-id="6c342-144">(You typically want to start the "Developer Command Prompt for Visual Studio" from the Start menu, as it will be configured with all the necessary paths for MSBuild.)</span></span>

```cli
msbuild /t:pack /p:Configuration=Release
```

<span data-ttu-id="6c342-145">然后可在 `bin\Release` 文件夹中找到此包。</span><span class="sxs-lookup"><span data-stu-id="6c342-145">The package can then be found in the `bin\Release` folder.</span></span>

<span data-ttu-id="6c342-146">有关 `msbuild /t:pack` 的其他选项，请参阅 [NuGet 打包和还原为 MSBuild 目标](../reference/msbuild-targets.md#pack-target)。</span><span class="sxs-lookup"><span data-stu-id="6c342-146">For additional options with `msbuild /t:pack`, see [NuGet pack and restore as MSBuild targets](../reference/msbuild-targets.md#pack-target).</span></span>

## <a name="publish-the-package"></a><span data-ttu-id="6c342-147">发布包</span><span class="sxs-lookup"><span data-stu-id="6c342-147">Publish the package</span></span>

<span data-ttu-id="6c342-148">有了 `.nupkg` 文件后，可以使用 `nuget.exe` CLI 或 `dotnet.exe` CLI 以及从 nuget.org 获取的 API 密钥将其发布到 nuget.org。</span><span class="sxs-lookup"><span data-stu-id="6c342-148">Once you have a `.nupkg` file, you publish it to nuget.org using either the `nuget.exe` CLI or the `dotnet.exe` CLI along with an API key acquired from nuget.org.</span></span>

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### <a name="acquire-your-api-key"></a><span data-ttu-id="6c342-149">获取 API 密钥</span><span class="sxs-lookup"><span data-stu-id="6c342-149">Acquire your API key</span></span>

[!INCLUDE [publish-api-key](includes/publish-api-key.md)]

### <a name="publish-with-nuget-push"></a><span data-ttu-id="6c342-150">用 nuget push 发布</span><span class="sxs-lookup"><span data-stu-id="6c342-150">Publish with nuget push</span></span>

<span data-ttu-id="6c342-151">该步骤是使用 `dotnet.exe` 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="6c342-151">This step is an alternative to using `dotnet.exe`.</span></span>

1. <span data-ttu-id="6c342-152">更改到包含 `.nupkg` 文件的文件夹。</span><span class="sxs-lookup"><span data-stu-id="6c342-152">Change to the folder containing the `.nupkg` file.</span></span>

1. <span data-ttu-id="6c342-153">运行以下命令，指定包名称并使用你的 API 密钥替换密钥值：</span><span class="sxs-lookup"><span data-stu-id="6c342-153">Run the following command, specifying your package name and replacing the key value with your API key:</span></span>

    ```cli
    nuget push AppLogger.1.0.0.nupkg qz2jga8pl3dvn2akksyquwcs9ygggg4exypy3bhxy6w6x6 -Source https://api.nuget.org/v3/index.json
    ```

1. <span data-ttu-id="6c342-154">nuget.exe 会显示发布过程的结果：</span><span class="sxs-lookup"><span data-stu-id="6c342-154">nuget.exe displays the results of the publishing process:</span></span>

    ```output
    Pushing AppLogger.1.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
        PUT https://www.nuget.org/api/v2/package/
        Created https://www.nuget.org/api/v2/package/ 6829ms
    Your package was pushed.
    ```

<span data-ttu-id="6c342-155">请参阅 [nuget push](../tools/cli-ref-push.md)。</span><span class="sxs-lookup"><span data-stu-id="6c342-155">See [nuget push](../tools/cli-ref-push.md).</span></span>

### <a name="publish-with-dotnet-nuget-push"></a><span data-ttu-id="6c342-156">用 dotnet nuget push 发布</span><span class="sxs-lookup"><span data-stu-id="6c342-156">Publish with dotnet nuget push</span></span>

<span data-ttu-id="6c342-157">该步骤是使用 `nuget.exe` 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="6c342-157">This step is an alternative to using `nuget.exe`.</span></span>

[!INCLUDE [publish-dotnet](includes/publish-dotnet.md)]

### <a name="publish-errors"></a><span data-ttu-id="6c342-158">发布错误</span><span class="sxs-lookup"><span data-stu-id="6c342-158">Publish errors</span></span>

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### <a name="manage-the-published-package"></a><span data-ttu-id="6c342-159">管理已发布的包</span><span class="sxs-lookup"><span data-stu-id="6c342-159">Manage the published package</span></span>

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## <a name="related-topics"></a><span data-ttu-id="6c342-160">相关主题</span><span class="sxs-lookup"><span data-stu-id="6c342-160">Related topics</span></span>

- [<span data-ttu-id="6c342-161">创建包</span><span class="sxs-lookup"><span data-stu-id="6c342-161">Create a Package</span></span>](../create-packages/creating-a-package.md)
- [<span data-ttu-id="6c342-162">发布包</span><span class="sxs-lookup"><span data-stu-id="6c342-162">Publish a Package</span></span>](../create-packages/publish-a-package.md)
- [<span data-ttu-id="6c342-163">预发行包</span><span class="sxs-lookup"><span data-stu-id="6c342-163">Pre-release Packages</span></span>](../create-packages/Prerelease-Packages.md)
- [<span data-ttu-id="6c342-164">支持多个目标框架</span><span class="sxs-lookup"><span data-stu-id="6c342-164">Support multiple target frameworks</span></span>](../create-packages/supporting-multiple-target-frameworks.md)
- [<span data-ttu-id="6c342-165">包版本控制</span><span class="sxs-lookup"><span data-stu-id="6c342-165">Package versioning</span></span>](../reference/package-versioning.md)
- [<span data-ttu-id="6c342-166">创建本地化包</span><span class="sxs-lookup"><span data-stu-id="6c342-166">Creating localized packages</span></span>](../create-packages/creating-localized-packages.md)
- [<span data-ttu-id="6c342-167">.NET Standard 库文档</span><span class="sxs-lookup"><span data-stu-id="6c342-167">.NET Standard Library documentation</span></span>](/dotnet/articles/standard/library)
- [<span data-ttu-id="6c342-168">从 .NET Framework 移植到 .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c342-168">Porting to .NET Core from .NET Framework</span></span>](/dotnet/articles/core/porting/index)