---
title: 使用 Visual Studio 创建和发布 .NET Framework NuGet 包的介绍性指南
description: 使用 Visual Studio 2017 创建和发布 .NET Framework NuGet 包的演练教程。
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 03/13/2018
ms.topic: quickstart
ms.openlocfilehash: 01760034a121b1ff6f227e006415779898c4cf6d
ms.sourcegitcommit: a6ca160b1e7e5c58b135af4eba0e9463127a59e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-and-publish-a-package-using-visual-studio-net-framework"></a><span data-ttu-id="263d2-103">快速入门：使用 Visual Studio 创建和发布包 (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="263d2-103">Quickstart: Create and publish a package using Visual Studio (.NET Framework)</span></span>

<span data-ttu-id="263d2-104">若要从 .NET Framework 类库创建 NuGet 包，需要在 Visual Studio 中创建 DLL，然后使用 nuget.exe 命令行工具创建并发布包。</span><span class="sxs-lookup"><span data-stu-id="263d2-104">Creating a NuGet package from a .NET Framework Class Library involves creating the DLL in Visual Studio, then using the nuget.exe command line tool to create and publish the package.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="263d2-105">系统必备</span><span class="sxs-lookup"><span data-stu-id="263d2-105">Prerequisites</span></span>

1. <span data-ttu-id="263d2-106">通过任何与 .NET 相关的工作负载从 [visualstudio.com](https://www.visualstudio.com/) 安装任意版本的 Visual Studio 2017。</span><span class="sxs-lookup"><span data-stu-id="263d2-106">Install any edition of Visual Studio 2017 from [visualstudio.com](https://www.visualstudio.com/) with any .NET-related workload.</span></span> <span data-ttu-id="263d2-107">安装 .NET 工作负载时，Visual Studio 2017 会自动包含 NuGet 功能。</span><span class="sxs-lookup"><span data-stu-id="263d2-107">Visual Studio 2017 automatically includes NuGet capabilities when a .NET workload is installed.</span></span>

1. <span data-ttu-id="263d2-108">要安装 `nuget.exe` CLI，从 [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe) 下载它，将 `.exe` 文件保存到合适的文件夹，然后将该文件夹添加到 PATH 环境变量中。</span><span class="sxs-lookup"><span data-stu-id="263d2-108">Install the `nuget.exe` CLI by downloading it from [nuget.org](https://dist.nuget.org/win-x86-commandline/latest/nuget.exe), saving that `.exe` file to a suitable folder, and adding that folder to your PATH environment variable.</span></span>

1. <span data-ttu-id="263d2-109">如果你还没有帐户，请[在 nuget.org 上注册一个免费帐户](https://www.nuget.org/users/account/LogOn?returnUrl=%2F)。</span><span class="sxs-lookup"><span data-stu-id="263d2-109">[Register for a free account on nuget.org](https://www.nuget.org/users/account/LogOn?returnUrl=%2F) if you don't have one already.</span></span> <span data-ttu-id="263d2-110">创建新帐户会发送确认电子邮件。</span><span class="sxs-lookup"><span data-stu-id="263d2-110">Creating a new account sends a confirmation email.</span></span> <span data-ttu-id="263d2-111">必须先确认该帐户，才能上传包。</span><span class="sxs-lookup"><span data-stu-id="263d2-111">You must confirm the account before you can upload a package.</span></span>

## <a name="create-a-class-library-project"></a><span data-ttu-id="263d2-112">创建类库项目</span><span class="sxs-lookup"><span data-stu-id="263d2-112">Create a class library project</span></span>

<span data-ttu-id="263d2-113">可以使用现有的 .NET Framework 类库项目用于要打包的代码，或者创建一个简单的项目，如下所示：</span><span class="sxs-lookup"><span data-stu-id="263d2-113">You can use an existing .NET Framework Class Library project for the code you want to package, or create a simple one as follows:</span></span>

1. <span data-ttu-id="263d2-114">在 Visual Studio 中，选择“文件”>“新建”>“项目”，再依次选择“Visual C#”节点、“类库(.NET Framework)”模板，将项目命名为“AppLogger”，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="263d2-114">In Visual Studio, choose **File > New > Project**, select the **Visual C#** node, select the "Class Library (.NET Framework)" template, name the project AppLogger, and click **OK**.</span></span>

1. <span data-ttu-id="263d2-115">右键单击生成的项目文件并选择“生成”，确保已正确创建项目。</span><span class="sxs-lookup"><span data-stu-id="263d2-115">Right-click on the resulting project file and select **Build** to make sure the project was created properly.</span></span> <span data-ttu-id="263d2-116">DLL 位于调试文件夹中（或发布中，如果生成的是该配置）。</span><span class="sxs-lookup"><span data-stu-id="263d2-116">The DLL is found within the Debug folder (or Release if you build that configuration instead).</span></span>

<span data-ttu-id="263d2-117">当然，在实际的 NuGet 包中，可实现许多有用的功能，让其他人可通过这些功能生成应用程序。</span><span class="sxs-lookup"><span data-stu-id="263d2-117">Within a real NuGet package, of course, you implement many useful features with which others can build applications.</span></span> <span data-ttu-id="263d2-118">也可以按个人喜欢的方式设置目标框架。</span><span class="sxs-lookup"><span data-stu-id="263d2-118">You can also set the target frameworks however you like.</span></span> <span data-ttu-id="263d2-119">有关示例，请参阅 [UWP](../guides/create-uwp-packages.md) 和 [Xamarin](../guides/create-packages-for-xamarin.md) 指南。</span><span class="sxs-lookup"><span data-stu-id="263d2-119">For example, see the guides for [UWP](../guides/create-uwp-packages.md) and [Xamarin](../guides/create-packages-for-xamarin.md).</span></span>

<span data-ttu-id="263d2-120">但是对于本演练，无需编写其他任何代码，因为模板的类库足以创建包。</span><span class="sxs-lookup"><span data-stu-id="263d2-120">For this walkthrough, however, you won't write any additional code because a class library from the template is sufficient to create a package.</span></span> <span data-ttu-id="263d2-121">但是，如果你需要此程序包的某个功能代码，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="263d2-121">Still, if you'd like some functional code for the package, use the following:</span></span>

```cs
using System;

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
> <span data-ttu-id="263d2-122">除非你有其他选择理由，否则 .NET Standard 是 NuGet 包的首选目标，因为它提供了与最广泛的使用项目的兼容性。</span><span class="sxs-lookup"><span data-stu-id="263d2-122">Unless you have a reason to choose otherwise, .NET Standard is the preferred target for NuGet packages, as it provides compatibility with the widest range of consuming projects.</span></span> <span data-ttu-id="263d2-123">请参阅[使用 Visual Studio 创建和发布包 (.NET Standard)](create-and-publish-a-package-using-visual-studio.md)。</span><span class="sxs-lookup"><span data-stu-id="263d2-123">See [Create and publish a package using Visual Studio (.NET Standard)](create-and-publish-a-package-using-visual-studio.md).</span></span>

## <a name="configure-project-properties-for-the-package"></a><span data-ttu-id="263d2-124">配置包的项目属性</span><span class="sxs-lookup"><span data-stu-id="263d2-124">Configure project properties for the package</span></span>

<span data-ttu-id="263d2-125">NuGet 包中包含清单（`.nuspec` 文件），其中包含相关的元数据，例如包标识符、版本号和描述等。</span><span class="sxs-lookup"><span data-stu-id="263d2-125">A NuGet package contains a manifest (a `.nuspec` file), that contains relevant metadata such as the package identifier, version number, description, and more.</span></span> <span data-ttu-id="263d2-126">其中一些元数据可直接从项目属性中获取，避免在项目和清单中对其进行单独更新。</span><span class="sxs-lookup"><span data-stu-id="263d2-126">Some of these can be drawn from the project properties directly, which avoids having to separately update them in both the project and the manifest.</span></span> <span data-ttu-id="263d2-127">本节介绍在何处设置适用的属性。</span><span class="sxs-lookup"><span data-stu-id="263d2-127">This section describes where to set the applicable properties.</span></span>

1. <span data-ttu-id="263d2-128">选择“项目”>“属性”菜单命令，然后选择“应用程序”选项卡。</span><span class="sxs-lookup"><span data-stu-id="263d2-128">Select the **Project > Properties** menu command, then select the **Application** tab.</span></span>

1. <span data-ttu-id="263d2-129">在“程序集名称”字段中，为包提供唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="263d2-129">In the **Assembly name** field, give your package a unique identifier.</span></span>

    > [!Important]
    > <span data-ttu-id="263d2-130">你必须为包提供一个在 nuget.org 中唯一或你使用的任何主机的标识符。</span><span class="sxs-lookup"><span data-stu-id="263d2-130">You must give the package an identifier that's unique across nuget.org or whatever host you're using.</span></span> <span data-ttu-id="263d2-131">对于本次演练，我们建议在名称中包含“Sample”或“Test”，因为稍后的发布步骤确实会使该包公开显示（尽管实际上不太可能有人会使用它）。</span><span class="sxs-lookup"><span data-stu-id="263d2-131">For this walkthrough we recommend including "Sample" or "Test" in the name as the later publishing step does make the package publicly visible (though it's unlikely anyone will actually use it).</span></span>
    >
    > <span data-ttu-id="263d2-132">如果你尝试发布名称已存在的包，则会看到一个错误。</span><span class="sxs-lookup"><span data-stu-id="263d2-132">If you attempt to publish a package with a name that already exists, you see an error.</span></span>

1. <span data-ttu-id="263d2-133">选择“程序集信息...”按钮，此时会出现一个对话框，可在其中输入清单中的其他属性（请参阅 [.nuspec 文件引用 - 替换令牌](../reference/nuspec.md#replacement-tokens)）。</span><span class="sxs-lookup"><span data-stu-id="263d2-133">Select the **Assembly Information...** button, which brings up a dialog box in which you can enter other properties that carry into the manifest (see [.nuspec file reference - replacement tokens](../reference/nuspec.md#replacement-tokens)).</span></span> <span data-ttu-id="263d2-134">最常用的字段是“标题”、“描述”、“公司”、“版权所有”和“程序集版本”。</span><span class="sxs-lookup"><span data-stu-id="263d2-134">The most commonly used fields are **Title**, **Description**, **Company**, **Copyright**, and **Assembly version**.</span></span> <span data-ttu-id="263d2-135">这些属性最终会随包出现在主机上（例如 nuget.org ），因此请确保对其进行完整描述。</span><span class="sxs-lookup"><span data-stu-id="263d2-135">These properties ultimately appear with your package on a host like nuget.org, so make sure they're fully descriptive.</span></span>

    ![Visual Studio 内 .NET Framework 项目中的程序集信息](media/qs_create-vs-01b-project-properties.png)

1. <span data-ttu-id="263d2-137">可选：若要直接查看和编辑属性，请打开项目中的 `Properties/AssemblyInfo.cs` 文件。</span><span class="sxs-lookup"><span data-stu-id="263d2-137">Optional: to see and edit the properties directly, open the `Properties/AssemblyInfo.cs` file in the project.</span></span>

1. <span data-ttu-id="263d2-138">设置属性时，请将项目配置设置为“发布”并重新生成项目以生成更新的 DLL。</span><span class="sxs-lookup"><span data-stu-id="263d2-138">When the properties are set, set the project configuration to **Release** and rebuild the project to generate the updated DLL.</span></span>

## <a name="generate-the-initial-manifest"></a><span data-ttu-id="263d2-139">生成初始清单</span><span class="sxs-lookup"><span data-stu-id="263d2-139">Generate the initial manifest</span></span>

<span data-ttu-id="263d2-140">既然已准备好 DLL 并已设置项目属性，现在即可使用 `nuget spec` 命令从项目生成初始 `.nuspec` 文件。</span><span class="sxs-lookup"><span data-stu-id="263d2-140">With a DLL in hand and project properties set, you now use the `nuget spec` command to generate an initial `.nuspec` file from the project.</span></span> <span data-ttu-id="263d2-141">此步骤包括相关替换令牌，便于从项目文件中提取信息。</span><span class="sxs-lookup"><span data-stu-id="263d2-141">This step includes the relevant replacement tokens to draw information from the project file.</span></span>

<span data-ttu-id="263d2-142">仅运行一次 `nuget spec` 即可生成初始清单。</span><span class="sxs-lookup"><span data-stu-id="263d2-142">You run `nuget spec` only once to generate the initial manifest.</span></span> <span data-ttu-id="263d2-143">更新包时，可以更改项目中的值或直接编辑清单。</span><span class="sxs-lookup"><span data-stu-id="263d2-143">When updating the package, you either change values in your project or edit the manifest directly.</span></span>

1. <span data-ttu-id="263d2-144">打开命令提示符并导航到包含 `AppLogger.csproj` 文件的项目文件夹。</span><span class="sxs-lookup"><span data-stu-id="263d2-144">Open a command prompt and navigate to the project folder containing `AppLogger.csproj` file.</span></span>

1. <span data-ttu-id="263d2-145">运行以下命令：`nuget spec AppLogger.csproj`。</span><span class="sxs-lookup"><span data-stu-id="263d2-145">Run the following command: `nuget spec AppLogger.csproj`.</span></span> <span data-ttu-id="263d2-146">通过指定项目，NuGet 会创建匹配项目名称的清单，在此示例中为 `AppLogger.nuspec`。</span><span class="sxs-lookup"><span data-stu-id="263d2-146">By specifying a project, NuGet creates a manifest that matches the name of the project, in this case `AppLogger.nuspec`.</span></span> <span data-ttu-id="263d2-147">它还会包括清单中的替换令牌。</span><span class="sxs-lookup"><span data-stu-id="263d2-147">It also include replacement tokens in the manifest.</span></span>

1. <span data-ttu-id="263d2-148">在文本编辑器中打开 `AppLogger.nuspec` 以检查其内容，其内容应如下所示：</span><span class="sxs-lookup"><span data-stu-id="263d2-148">Open `AppLogger.nuspec` in a text editor to examine its contents, which should appear as follows:</span></span>

    ```xml
    <?xml version="1.0"?>
    <package >
      <metadata>
        <id>$id$</id>
        <version>$version$</version>
        <title>$title$</title>
        <authors>$author$</authors>
        <owners>$author$</owners>
        <licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
        <projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
        <iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <description>$description$</description>
        <releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
        <copyright>Copyright 2018</copyright>
        <tags>Tag1 Tag2</tags>
      </metadata>
    </package>
    ```

## <a name="edit-the-manifest"></a><span data-ttu-id="263d2-149">编辑清单</span><span class="sxs-lookup"><span data-stu-id="263d2-149">Edit the manifest</span></span>

1. <span data-ttu-id="263d2-150">如果尝试在 `.nuspec` 文件中创建包含默认值的包，NuGet 会产生错误，因此在继续操作之前必须编辑以下字段。</span><span class="sxs-lookup"><span data-stu-id="263d2-150">NuGet produces an error if you try to create a package with default values in your `.nuspec` file, so you must edit the following fields before proceeding.</span></span> <span data-ttu-id="263d2-151">有关如何使用这些字段的说明，请参阅 [.nuspec 文件引用 - 单个元素](../reference/nuspec.md#single-elements)。</span><span class="sxs-lookup"><span data-stu-id="263d2-151">See [.nuspec file reference - single elements](../reference/nuspec.md#single-elements) for a description of how these are used.</span></span>

    - <span data-ttu-id="263d2-152">licenseUrl</span><span class="sxs-lookup"><span data-stu-id="263d2-152">licenseUrl</span></span>
    - <span data-ttu-id="263d2-153">projectUrl</span><span class="sxs-lookup"><span data-stu-id="263d2-153">projectUrl</span></span>
    - <span data-ttu-id="263d2-154">iconUrl</span><span class="sxs-lookup"><span data-stu-id="263d2-154">iconUrl</span></span>
    - <span data-ttu-id="263d2-155">releaseNotes</span><span class="sxs-lookup"><span data-stu-id="263d2-155">releaseNotes</span></span>
    - <span data-ttu-id="263d2-156">标记</span><span class="sxs-lookup"><span data-stu-id="263d2-156">tags</span></span>

1. <span data-ttu-id="263d2-157">对于面向公共使用而生成的包，请特别注意 Tags 属性，因为这些标记可帮助其他人在源中（例如 nuget.org）查找包并了解其用途。</span><span class="sxs-lookup"><span data-stu-id="263d2-157">For packages built for public consumption, pay special attention to the **Tags** property, as tags help others find your package on sources like nuget.org and understand what it does.</span></span>

1. <span data-ttu-id="263d2-158">也可以在此时向清单中添加任何其他元素，如 [.nuspec 文件引用](../reference/nuspec.md)中所述。</span><span class="sxs-lookup"><span data-stu-id="263d2-158">You can also add any other elements to the manifest at this time, as described on [.nuspec file reference](../reference/nuspec.md).</span></span>

1. <span data-ttu-id="263d2-159">在继续之前保存文件。</span><span class="sxs-lookup"><span data-stu-id="263d2-159">Save the file before proceeding.</span></span>

## <a name="run-the-pack-command"></a><span data-ttu-id="263d2-160">运行 pack 命令</span><span class="sxs-lookup"><span data-stu-id="263d2-160">Run the pack command</span></span>

1. <span data-ttu-id="263d2-161">从包含 `.nuspec` 文件的文件夹中的命令提示符，运行命令 `nuget pack`。</span><span class="sxs-lookup"><span data-stu-id="263d2-161">From a command prompt in the folder containing your `.nuspec` file, run the command `nuget pack`.</span></span>

1. <span data-ttu-id="263d2-162">NuGet 以 identifier-version.nupkg 形式生成 `.nupkg` 文件，你可在当前文件夹中找到该文件。</span><span class="sxs-lookup"><span data-stu-id="263d2-162">NuGet generates a `.nupkg` file in the form of *identifier-version.nupkg*, which you'll find in the current folder.</span></span>

## <a name="publish-the-package"></a><span data-ttu-id="263d2-163">发布包</span><span class="sxs-lookup"><span data-stu-id="263d2-163">Publish the package</span></span>

<span data-ttu-id="263d2-164">有了 `.nupkg` 文件后，可以使用 `nuget.exe` 以及从 nuget.org 获取的 API 密钥将其发布到 nuget.org。对于 nuget.org，必须使用 `nuget.exe` 4.1.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="263d2-164">Once you have a `.nupkg` file, you publish it to nuget.org using `nuget.exe` with an API key acquired from nuget.org. For nuget.org you must use `nuget.exe` 4.1.0 or higher.</span></span>

[!INCLUDE [publish-notes](includes/publish-notes.md)]

### <a name="acquire-your-api-key"></a><span data-ttu-id="263d2-165">获取 API 密钥</span><span class="sxs-lookup"><span data-stu-id="263d2-165">Acquire your API key</span></span>

[!INCLUDE [publish-api-key](includes/publish-api-key.md)]

### <a name="publish-with-nuget-push"></a><span data-ttu-id="263d2-166">用 nuget push 发布</span><span class="sxs-lookup"><span data-stu-id="263d2-166">Publish with nuget push</span></span>

1. <span data-ttu-id="263d2-167">更改到包含 `.nupkg` 文件的文件夹。</span><span class="sxs-lookup"><span data-stu-id="263d2-167">Change to the folder containing the `.nupkg` file.</span></span>

1. <span data-ttu-id="263d2-168">运行以下命令，指定包名称并使用你的 API 密钥替换密钥值：</span><span class="sxs-lookup"><span data-stu-id="263d2-168">Run the following command, specifying your package name and replacing the key value with your API key:</span></span>

    ```cli
    nuget push AppLogger.1.0.0.nupkg qz2jga8pl3dvn2akksyquwcs9ygggg4exypy3bhxy6w6x6 -Source https://api.nuget.org/v3/index.json
    ```

1. <span data-ttu-id="263d2-169">nuget.exe 会显示发布过程的结果：</span><span class="sxs-lookup"><span data-stu-id="263d2-169">nuget.exe displays the results of the publishing process:</span></span>

    ```output
    Pushing AppLogger.1.0.0.nupkg to 'https://www.nuget.org/api/v2/package'...
        PUT https://www.nuget.org/api/v2/package/
        Created https://www.nuget.org/api/v2/package/ 6829ms
    Your package was pushed.
    ```

<span data-ttu-id="263d2-170">请参阅 [nuget push](../tools/cli-ref-push.md)。</span><span class="sxs-lookup"><span data-stu-id="263d2-170">See [nuget push](../tools/cli-ref-push.md).</span></span>

### <a name="publish-errors"></a><span data-ttu-id="263d2-171">发布错误</span><span class="sxs-lookup"><span data-stu-id="263d2-171">Publish errors</span></span>

[!INCLUDE [publish-errors](includes/publish-errors.md)]

### <a name="manage-the-published-package"></a><span data-ttu-id="263d2-172">管理已发布的包</span><span class="sxs-lookup"><span data-stu-id="263d2-172">Manage the published package</span></span>

[!INCLUDE [publish-manage](includes/publish-manage.md)]

## <a name="related-topics"></a><span data-ttu-id="263d2-173">相关主题</span><span class="sxs-lookup"><span data-stu-id="263d2-173">Related topics</span></span>

- [<span data-ttu-id="263d2-174">创建包</span><span class="sxs-lookup"><span data-stu-id="263d2-174">Create a Package</span></span>](../create-packages/creating-a-package.md)
- [<span data-ttu-id="263d2-175">发布包</span><span class="sxs-lookup"><span data-stu-id="263d2-175">Publish a Package</span></span>](../create-packages/publish-a-package.md)
- [<span data-ttu-id="263d2-176">预发行包</span><span class="sxs-lookup"><span data-stu-id="263d2-176">Pre-release Packages</span></span>](../create-packages/Prerelease-Packages.md)
- [<span data-ttu-id="263d2-177">支持多个目标框架</span><span class="sxs-lookup"><span data-stu-id="263d2-177">Support multiple target frameworks</span></span>](../create-packages/supporting-multiple-target-frameworks.md)
- [<span data-ttu-id="263d2-178">包版本控制</span><span class="sxs-lookup"><span data-stu-id="263d2-178">Package versioning</span></span>](../reference/package-versioning.md)
- [<span data-ttu-id="263d2-179">创建本地化包</span><span class="sxs-lookup"><span data-stu-id="263d2-179">Creating localized packages</span></span>](../create-packages/creating-localized-packages.md)