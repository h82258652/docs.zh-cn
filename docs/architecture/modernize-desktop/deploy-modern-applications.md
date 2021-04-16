---
title: 部署新式桌面应用程序
description: 部署新式桌面应用程序时需要了解的所有内容。
ms.date: 05/12/2020
ms.openlocfilehash: ba47f09b27adf270734bbfff285fe44dd4175d29
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98615849"
---
# <a name="deploying-modern-desktop-applications"></a><span data-ttu-id="f2fac-103">部署新式桌面应用程序</span><span class="sxs-lookup"><span data-stu-id="f2fac-103">Deploying Modern Desktop Applications</span></span>

<span data-ttu-id="f2fac-104">当你开发桌面应用程序时，需要考虑的一个问题是如何打包应用程序并将其部署到用户的计算机上。</span><span class="sxs-lookup"><span data-stu-id="f2fac-104">When you develop desktop applications, one thing to consider is how your application is going to be packaged and deployed to the users' machines.</span></span> <span data-ttu-id="f2fac-105">打包、部署和安装的问题在于，它通常由 IT 专业人员负责，而 IT 专业人员关注的点与开发人员不同。</span><span class="sxs-lookup"><span data-stu-id="f2fac-105">The problem with packaging, deployment, and installation is that it usually falls under the umbrella of the IT professionals, who care about different things than developers.</span></span>

<span data-ttu-id="f2fac-106">如今，我们都很熟悉 DevOps 的概念，在 DevOps 中，开发人员和 IT 专业人员密切合作，将应用程序迁移到生产环境中。</span><span class="sxs-lookup"><span data-stu-id="f2fac-106">These days, we're all familiar with the DevOps concept, where developers and IT Pros work closely to move applications to their production environments.</span></span> <span data-ttu-id="f2fac-107">但是，如果你身陷这场桌面战役已经超过 10 年，那么，你可能看到过下面这样的场景。</span><span class="sxs-lookup"><span data-stu-id="f2fac-107">But if you've been in the desktop battle for more than 10 years, you might have seen the following story.</span></span> <span data-ttu-id="f2fac-108">一支由开发人员组成的团队共同努力，以期按时完成项目。</span><span class="sxs-lookup"><span data-stu-id="f2fac-108">A team of developers works together hard to meet the project deadlines.</span></span> <span data-ttu-id="f2fac-109">业务人员很紧张，因为他们需要系统在多台用户计算机上正常运行，以维持公司运营。</span><span class="sxs-lookup"><span data-stu-id="f2fac-109">Business people are nervous since they need the system working on many user's machines to run the company.</span></span> <span data-ttu-id="f2fac-110">在“D 日”，项目经理向每位开发人员确认了他们的代码运行良好，一切运转正常，他们可以交付应用了。</span><span class="sxs-lookup"><span data-stu-id="f2fac-110">On "D-Day", the project manager checks with every developer that their code is working well and that everything is fine, so they can ship.</span></span> <span data-ttu-id="f2fac-111">然后，包团队开始生成应用的安装程序，将其分发到每台用户计算机上，一组测试用户开始运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-111">Then, the package team comes in generating the setup for the app, distribute it to every user machine and a set of test users run the application.</span></span> <span data-ttu-id="f2fac-112">好吧，他们只是试了一下，因为还没显示任何 UI，应用程序就引发一个异常，指出“Method ~ of object ~ failed”。</span><span class="sxs-lookup"><span data-stu-id="f2fac-112">Well, they try, because before showing any UI, the application throws an exception that says "Method ~ of object ~ failed".</span></span> <span data-ttu-id="f2fac-113">恐慌情绪开始蔓延，经过简单调查后，矛头指向一个年轻而疲惫的开发人员，他引入了一个第三方控件，这无疑是“在开发计算机上工作”。</span><span class="sxs-lookup"><span data-stu-id="f2fac-113">Panic starts flowing through the air and a brief investigation points to a young and tired developer that has introduced a third-party control, that certainly "worked on the dev machine".</span></span>

<span data-ttu-id="f2fac-114">安装桌面应用程序历来就是一场噩梦，主要原因有两个：</span><span class="sxs-lookup"><span data-stu-id="f2fac-114">Installing desktop applications have traditionally been a nightmare for two main reasons:</span></span>

- <span data-ttu-id="f2fac-115">开发团队和 IT 团队之间缺乏紧密的合作文化。</span><span class="sxs-lookup"><span data-stu-id="f2fac-115">Lack of close collaboration culture between dev and IT teams.</span></span>
- <span data-ttu-id="f2fac-116">缺乏可以依赖的可靠打包和部署技术。</span><span class="sxs-lookup"><span data-stu-id="f2fac-116">Lack of a solid packaging and deploying technology we can build upon.</span></span>

<span data-ttu-id="f2fac-117">实际上，我们一直生活在这样的事实中，有时候你后悔安装了一个应用，因为：</span><span class="sxs-lookup"><span data-stu-id="f2fac-117">In fact, we've been living with the fact that sometimes you regret that you installed an app because:</span></span>

- <span data-ttu-id="f2fac-118">它最终在你的计算机上产生了一些意想不到的副作用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-118">It ends up having some undesired side effects on your machine.</span></span>
- <span data-ttu-id="f2fac-119">以前安装的某些应用程序停止工作。</span><span class="sxs-lookup"><span data-stu-id="f2fac-119">Some applications that were previously installed stop working.</span></span>

<span data-ttu-id="f2fac-120">此外，你无法通过卸载应用将系统还原到原始状态。</span><span class="sxs-lookup"><span data-stu-id="f2fac-120">Additionally, you can't just restore the system to its original state by uninstalling the app.</span></span> <span data-ttu-id="f2fac-121">我们对这种情况习以为常，以至于创造出“DLL Hell”或“Winrot”这样的术语。</span><span class="sxs-lookup"><span data-stu-id="f2fac-121">We're so used to live this situation that we've coined terms like "DLL Hell" or "Winrot".</span></span>

<span data-ttu-id="f2fac-122">在本章中，我们将探讨 MSIX。</span><span class="sxs-lookup"><span data-stu-id="f2fac-122">In this chapter, we'll talk about MSIX.</span></span> <span data-ttu-id="f2fac-123">MSIX 是 Microsoft 推出的全新技术，它试图汲取以往技术的精华，为未来的打包技术打下坚实的基础。</span><span class="sxs-lookup"><span data-stu-id="f2fac-123">MSIX is the brand-new technology from Microsoft that tries to capture the best of previous technologies to provide a solid foundation for the packaging technology of the future.</span></span>

<span data-ttu-id="f2fac-124">打包技术和现代化有什么关系？</span><span class="sxs-lookup"><span data-stu-id="f2fac-124">What does a packaging technology have to do with modernization?</span></span> <span data-ttu-id="f2fac-125">事实证明，打包是企业 IT 的根本，企业的大量资金便投入于此。</span><span class="sxs-lookup"><span data-stu-id="f2fac-125">Well, it turns out that packaging is fundamental for the enterprise IT with lots of money invested there.</span></span> <span data-ttu-id="f2fac-126">现代化不仅与使用最新技术有关。</span><span class="sxs-lookup"><span data-stu-id="f2fac-126">Modernization isn't only related to using the latest technologies.</span></span> <span data-ttu-id="f2fac-127">它还与缩短上市时间（从定义业务需求到公司向客户交付该功能）有关。</span><span class="sxs-lookup"><span data-stu-id="f2fac-127">It's also related to reducing time to market from the moment a business requirement is defined until your company delivers the feature to your client.</span></span>

## <a name="the-modern-application-lifecycle"></a><span data-ttu-id="f2fac-128">新式应用程序的生命周期</span><span class="sxs-lookup"><span data-stu-id="f2fac-128">The modern application lifecycle</span></span>

<span data-ttu-id="f2fac-129">如今，开发人员编写并生成应用代码，然后将生成的资产交给 IT 专业人员。</span><span class="sxs-lookup"><span data-stu-id="f2fac-129">Today, developers write and build the code for an app and then pass the generated assets to the IT Pros.</span></span> <span data-ttu-id="f2fac-130">IT 专业人员通常采用 MSI 或最近的 App-V 打包格式，对应用进行重新配置和重新打包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-130">Then, the IT Pros reconfigure the app and repackage it, typically in an MSI or more recently in an App-V packaging format.</span></span> <span data-ttu-id="f2fac-131">然后，应用通过各种渠道和工具进行部署。</span><span class="sxs-lookup"><span data-stu-id="f2fac-131">The app is then deployed through different channels and tools.</span></span> <span data-ttu-id="f2fac-132">这种方法的主要问题之一通常称为“打包瘫痪”。</span><span class="sxs-lookup"><span data-stu-id="f2fac-132">One of the main problems with this approach is commonly known as "packaging paralysis".</span></span> <span data-ttu-id="f2fac-133">其问题在于，每次有应用更新或操作系统更新时，都会重复这个打包周期。</span><span class="sxs-lookup"><span data-stu-id="f2fac-133">The problem is that this cycle repeats every time there's an app update or an OS update.</span></span>

<span data-ttu-id="f2fac-134">你可以看看下图所示的过程：</span><span class="sxs-lookup"><span data-stu-id="f2fac-134">You can see the process reflected on the following picture:</span></span>

![该图显示了新式 IT 生命周期](./media/deploy-modern-applications/modern-it-application-lifecycle.png)

<span data-ttu-id="f2fac-136">公司需要采用一种方法，将这个打包周期分成三个独立的周期：</span><span class="sxs-lookup"><span data-stu-id="f2fac-136">Companies need a way to break this packaging cycle into three independent cycles:</span></span>

- <span data-ttu-id="f2fac-137">OS 更新</span><span class="sxs-lookup"><span data-stu-id="f2fac-137">OS updates</span></span>
- <span data-ttu-id="f2fac-138">应用程序更新</span><span class="sxs-lookup"><span data-stu-id="f2fac-138">Application updates</span></span>
- <span data-ttu-id="f2fac-139">自定义</span><span class="sxs-lookup"><span data-stu-id="f2fac-139">Customization</span></span>

![该图显示了新式 IT 良性循环](./media/deploy-modern-applications/modern-it-virtuous-cycle.png)

<span data-ttu-id="f2fac-141">上图说明你可以：</span><span class="sxs-lookup"><span data-stu-id="f2fac-141">The previous diagram shows that you can:</span></span>

- <span data-ttu-id="f2fac-142">更新底层 OS，而无需重新打包应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-142">Update the underlying OS without having to repackage your apps.</span></span>
- <span data-ttu-id="f2fac-143">实现 IT 自定义，而无需重新打包原始开发人员包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-143">Enable customizations from IT without the need to repackage the original developer package.</span></span>

<span data-ttu-id="f2fac-144">这种根本性的变化将我们引向了新式 IT 生命周期，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="f2fac-144">This radical change leads us to the new and modern IT lifecycle as shown in the following picture:</span></span>

![该图显示了使用 Microsoft 工具的应用程序生命周期](./media/deploy-modern-applications/microsoft-it-tools.png)

<span data-ttu-id="f2fac-146">开发人员创建应用并生成 MSIX 包，IT 专业人员可以使用和配置该包，而无需重新打包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-146">Developers create the app and generate an MSIX package that IT Pros can consume and configure without the need of repackaging.</span></span> <span data-ttu-id="f2fac-147">除了 MSIX 技术，Microsoft 还创建了一些工具，让 IT 人员无需重新打包即可自定义和配置包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-147">Along with the MSIX technology, Microsoft has created tools to allow IT to customize and configure packages without repackaging.</span></span>

## <a name="msix-the-next-generation-of-deployment"></a><span data-ttu-id="f2fac-148">MSIX：下一代部署</span><span class="sxs-lookup"><span data-stu-id="f2fac-148">MSIX: The next generation of deployment</span></span>

<span data-ttu-id="f2fac-149">在 MSIX 之前，存在多种打包技术，例如设置向导、MSI、ClickOnce、App-V 和脚本。</span><span class="sxs-lookup"><span data-stu-id="f2fac-149">Before MSIX, there were several packaging technologies available like setup wizards, MSI, ClickOnce, App-V, and scripting.</span></span> <span data-ttu-id="f2fac-150">每种技术都有其各自的优势，于是，Microsoft 决定集百家之长来构建 MSIX。</span><span class="sxs-lookup"><span data-stu-id="f2fac-150">Each of these technologies has their own strengths and Microsoft has decided to pick the best of all to build MSIX.</span></span> <span data-ttu-id="f2fac-151">MSIX 建立在这些现有技术的基础上，并取其精华：</span><span class="sxs-lookup"><span data-stu-id="f2fac-151">MSIX is built on the foundations of these existing technologies picking the best of each:</span></span>

- <span data-ttu-id="f2fac-152">App-V =\> 容器化</span><span class="sxs-lookup"><span data-stu-id="f2fac-152">App-V =\> Containerization</span></span>
- <span data-ttu-id="f2fac-153">ClickOnce =\> 自动更新</span><span class="sxs-lookup"><span data-stu-id="f2fac-153">ClickOnce =\> Auto updating</span></span>
- <span data-ttu-id="f2fac-154">MSI =\> 易于分发</span><span class="sxs-lookup"><span data-stu-id="f2fac-154">MSI =\> Easy to distribute</span></span>

<span data-ttu-id="f2fac-155">有了 MSIX，就等于有了一项集这些功能于一身的安装程序技术。</span><span class="sxs-lookup"><span data-stu-id="f2fac-155">With MSIX, you get one installer technology with all these features.</span></span>

![该图显示了影响 MSIX 构建的各种技术](./media/deploy-modern-applications/msix.png)

### <a name="benefits-of-msix"></a><span data-ttu-id="f2fac-157">MSIX 的好处</span><span class="sxs-lookup"><span data-stu-id="f2fac-157">Benefits of MSIX</span></span>

#### <a name="never-regret-installing-an-app"></a><span data-ttu-id="f2fac-158">永远不会后悔安装了某个应用</span><span class="sxs-lookup"><span data-stu-id="f2fac-158">Never regret installing an app</span></span>

<span data-ttu-id="f2fac-159">MSIX 提供可预测、可靠、安全的部署。</span><span class="sxs-lookup"><span data-stu-id="f2fac-159">MSIX provides a predictable, reliable, and safe deployment.</span></span> <span data-ttu-id="f2fac-160">包清单中包含的声明性方法使操作系统可以跟踪应用程序所需的每项资产。</span><span class="sxs-lookup"><span data-stu-id="f2fac-160">The declarative method contained in the package manifest lets the OS keep track of every asset your application needs.</span></span> <span data-ttu-id="f2fac-161">它还提供真正的干净卸载，没有任何副作用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-161">It also provides a true clean uninstall with no side effects.</span></span>

#### <a name="disk-space-optimization"></a><span data-ttu-id="f2fac-162">磁盘空间优化</span><span class="sxs-lookup"><span data-stu-id="f2fac-162">Disk space optimization</span></span>

<span data-ttu-id="f2fac-163">MSIX 经过优化，可减少应用程序对用户计算机磁盘空间的占用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-163">MSIX is optimized to reduce the footprint that an application has on the user's machine disk space.</span></span> <span data-ttu-id="f2fac-164">它会创建文件的单个实例存储。</span><span class="sxs-lookup"><span data-stu-id="f2fac-164">It creates a single instance storage of your files.</span></span> <span data-ttu-id="f2fac-165">也就是说，如果你有两个具有相同 DLL 的不同包，则不会安装该 DLL 两次。</span><span class="sxs-lookup"><span data-stu-id="f2fac-165">That is, if you have two different packages with the same DLL, the DLL isn't installed twice.</span></span> <span data-ttu-id="f2fac-166">该平台解决了这个问题，因为它知道特定应用安装的所有文件，这要归功于它的声明性质。</span><span class="sxs-lookup"><span data-stu-id="f2fac-166">The platform takes care of that problem because it knows all the files that a particular app installed thanks to its declarative nature.</span></span> <span data-ttu-id="f2fac-167">它还允许你并行运行不同版本的 DLL。</span><span class="sxs-lookup"><span data-stu-id="f2fac-167">It also allows you to have different versions of a DLL working side by side.</span></span>

<span data-ttu-id="f2fac-168">通过使用资源包，你可以轻松创建多语言应用，并且操作系统会安装所使用的应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-168">With the use of resource packages, you can easily create multilingual apps and the OS takes care of installing the ones that are used.</span></span>

#### <a name="network-optimization"></a><span data-ttu-id="f2fac-169">网络优化</span><span class="sxs-lookup"><span data-stu-id="f2fac-169">Network optimization</span></span>

<span data-ttu-id="f2fac-170">MSIX 可在字节块级别检测到文件上的差异，从而实现了一项称为差异更新的功能。</span><span class="sxs-lookup"><span data-stu-id="f2fac-170">MSIX detects the differences on the files at the byte block level enabling a feature called differential updates.</span></span> <span data-ttu-id="f2fac-171">这意味着在应用程序更新时只下载更新的字节块。</span><span class="sxs-lookup"><span data-stu-id="f2fac-171">What this means is that only the updated byte blocks are downloaded on application updates.</span></span>

![该图显示了 MSIX 如何管理差异更新](./media/deploy-modern-applications/msix-managing-updates.png)

<span data-ttu-id="f2fac-173">使用流式安装，用户可以在后台下载应用程序其他部分的同时，快速开始使用应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-173">With streaming installation, the user can quickly start working on your application while other parts of the app are downloaded on the background.</span></span> <span data-ttu-id="f2fac-174">此功能有助于为用户带来引人入胜的体验。</span><span class="sxs-lookup"><span data-stu-id="f2fac-174">This feature contributes to an engaging experience for your users.</span></span>

<span data-ttu-id="f2fac-175">借助可选包功能，你可以在应用部署上实现组件化，这样你就可以在需要时下载它们。</span><span class="sxs-lookup"><span data-stu-id="f2fac-175">With the optional packages feature, you achieve componentization on your app deployment, so you can download them when needed.</span></span>

#### <a name="simple-packaging-and-deployment"></a><span data-ttu-id="f2fac-176">简单的打包和部署</span><span class="sxs-lookup"><span data-stu-id="f2fac-176">Simple packaging and deployment</span></span>

<span data-ttu-id="f2fac-177">AppManifest 以标准方式为每个应用程序声明版本控制、设备定位和标识。</span><span class="sxs-lookup"><span data-stu-id="f2fac-177">The AppManifest declares the versioning, device targeting and identify in a standard way for every application.</span></span> <span data-ttu-id="f2fac-178">它还提供一种对资产进行签名的方法，从而提供可靠的安全基础。</span><span class="sxs-lookup"><span data-stu-id="f2fac-178">It also provides a way to sign your assets providing a solid security foundation.</span></span>

#### <a name="os-managed"></a><span data-ttu-id="f2fac-179">操作系统托管</span><span class="sxs-lookup"><span data-stu-id="f2fac-179">OS managed</span></span>

<span data-ttu-id="f2fac-180">操作系统负责处理安装、更新和删除应用程序的所有过程。</span><span class="sxs-lookup"><span data-stu-id="f2fac-180">The OS handles all the processes for installing, updating, and removing an application.</span></span> <span data-ttu-id="f2fac-181">应用程序按用户安装，但仅下载一次，从而最大程度地减少了磁盘占用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-181">Applications are installed per user but downloaded only once, minimizing the disk footprint.</span></span> <span data-ttu-id="f2fac-182">Microsoft 正致力于在 Windows 7 上也提供 MSIX 体验。</span><span class="sxs-lookup"><span data-stu-id="f2fac-182">Microsoft is working on providing the MSIX experience also on Windows 7.</span></span>

#### <a name="windows-provides-integrity-for-the-app"></a><span data-ttu-id="f2fac-183">Windows 为应用提供完整性</span><span class="sxs-lookup"><span data-stu-id="f2fac-183">Windows provides integrity for the app</span></span>

<span data-ttu-id="f2fac-184">使用数字签名，可以确保不会从不受信任的源安装应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-184">With the use of digital signatures, you can guarantee that you don't install an application from untrusted sources.</span></span> <span data-ttu-id="f2fac-185">MSIX 还可以防止篡改，因为：</span><span class="sxs-lookup"><span data-stu-id="f2fac-185">MSIX also prevents tampering because:</span></span>

- <span data-ttu-id="f2fac-186">它会保留文件哈希的记录。</span><span class="sxs-lookup"><span data-stu-id="f2fac-186">It keeps a record of file hashes.</span></span>
- <span data-ttu-id="f2fac-187">它会检测是否在安装后修改了文件。</span><span class="sxs-lookup"><span data-stu-id="f2fac-187">It detects if a file has been modified after installation.</span></span>

#### <a name="works-for-the-entire-app-catalog"></a><span data-ttu-id="f2fac-188">适用于整个应用目录</span><span class="sxs-lookup"><span data-stu-id="f2fac-188">Works for the entire App Catalog</span></span>

<span data-ttu-id="f2fac-189">MSIX 最酷的一点是它适用于整个应用程序目录（Windows 窗体、WPF、MFC/ATL、Delphi），即使你想执行 xCopy 部署、ClickOnce 或进入 Store，也可以使用同一个 MSIX 包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-189">One of the coolest things about MSIX is that it works for the entire application catalog, Windows Forms, WPF, MFC/ATL, Delphi, even if you want to do xCopy deployment, ClickOnce, or going to the Store, you can use the same MSIX package.</span></span>

### <a name="tools"></a><span data-ttu-id="f2fac-190">工具</span><span class="sxs-lookup"><span data-stu-id="f2fac-190">Tools</span></span>

#### <a name="windows-application-packaging-project"></a><span data-ttu-id="f2fac-191">Windows 应用程序打包项目</span><span class="sxs-lookup"><span data-stu-id="f2fac-191">Windows Application Packaging Project</span></span>

<span data-ttu-id="f2fac-192">你可以使用 Visual Studio 中的 **Windows 应用程序打包项目** 项目为桌面应用生成程序包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-192">You can use the **Windows Application Packaging Project** project in Visual Studio to generate a package for your desktop app.</span></span> <span data-ttu-id="f2fac-193">然后，你可以将该包发布到 Microsoft Store，或将其旁加载到一台或多台电脑上。</span><span class="sxs-lookup"><span data-stu-id="f2fac-193">Then, you can publish that package to the Microsoft Store or sideload it onto one or more PCs.</span></span>

#### <a name="msix-packaging-tool"></a><span data-ttu-id="f2fac-194">MSIX 打包工具</span><span class="sxs-lookup"><span data-stu-id="f2fac-194">MSIX Packaging Tool</span></span>

<span data-ttu-id="f2fac-195">使用 MSIX 打包工具可将现有的 Win32 应用程序重新打包为 MSIX 格式。</span><span class="sxs-lookup"><span data-stu-id="f2fac-195">The MSIX Packaging Tool enables you to repackage your existing Win32 applications to the MSIX format.</span></span> <span data-ttu-id="f2fac-196">此工具提供交互式 UI 和命令行用于转换，并且可以在没有源代码的情况下转换应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-196">It offers both an interactive UI and a command line for conversions and gives you the ability to convert an application without having the source code.</span></span>

![MSIX 打包工具](./media/deploy-modern-applications/msix-packaging-tool.png)

#### <a name="package-support-framework"></a><span data-ttu-id="f2fac-198">包支持框架</span><span class="sxs-lookup"><span data-stu-id="f2fac-198">Package Support Framework</span></span>

<span data-ttu-id="f2fac-199">包支持框架是一个开源工具包，有助于在无权访问源代码时将修补程序应用于现有 Win32 应用程序，以便其在 MSIX 容器中运行。</span><span class="sxs-lookup"><span data-stu-id="f2fac-199">The Package Support Framework is an open-source kit that helps you apply fixes to your existing Win32 application when you don't have access to the source code, so that it can run in an MSIX container.</span></span> <span data-ttu-id="f2fac-200">包支持框架可帮助应用程序遵循新式运行时环境的最佳做法。</span><span class="sxs-lookup"><span data-stu-id="f2fac-200">The Package Support Framework helps your application follow the best practices of the modern runtime environment.</span></span>

#### <a name="app-installer"></a><span data-ttu-id="f2fac-201">应用安装程序</span><span class="sxs-lookup"><span data-stu-id="f2fac-201">App Installer</span></span>

<span data-ttu-id="f2fac-202">利用应用安装程序，可以通过双击应用包来安装 Windows 10 应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-202">App Installer allows Windows 10 apps to be installed by double-clicking the app package.</span></span> <span data-ttu-id="f2fac-203">这意味着用户无需使用 PowerShell 或其他开发人员工具，即可部署 Windows 10 应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-203">This means that users don't need to use PowerShell or other developer tools to deploy Windows 10 apps.</span></span> <span data-ttu-id="f2fac-204">应用安装程序还可以从 Web、可选包和相关的集中安装应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-204">The App Installer can also install an app from the web, optional packages, and related sets.</span></span>

## <a name="how-to-create-an-msix-package-from-an-existing-win32-desktop-application"></a><span data-ttu-id="f2fac-205">如何从现有的 Win32 桌面应用程序创建 MSIX 包</span><span class="sxs-lookup"><span data-stu-id="f2fac-205">How to create an MSIX package from an existing Win32 desktop application</span></span>

<span data-ttu-id="f2fac-206">让我们看一下从现有 Win32 应用程序创建 MSIX 包的过程。</span><span class="sxs-lookup"><span data-stu-id="f2fac-206">Let's go through the process to create an MSIX package from an existing Win32 application.</span></span> <span data-ttu-id="f2fac-207">在此示例中，我们将使用 Windows 窗体应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-207">In this example, we'll use a Windows Forms app.</span></span>

<span data-ttu-id="f2fac-208">首先，将一个新项目添加到解决方案中，选择“Windows 应用程序打包项目”，并为其命名。</span><span class="sxs-lookup"><span data-stu-id="f2fac-208">To start, add a new project to your solution, select the Windows Application Packaging Project, and give it a name.</span></span>

![添加新的 Windows 应用程序打包项目](./media/deploy-modern-applications/add-packaging-project.png)

<span data-ttu-id="f2fac-210">你会看到打包项目的结构，并注意到一个名为 Applications 的特殊文件夹。</span><span class="sxs-lookup"><span data-stu-id="f2fac-210">You'll see the structure of the packaging project and note a special folder called *Applications*.</span></span> <span data-ttu-id="f2fac-211">在此文件夹中，可以指定要包含在包中的应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-211">Inside this folder, you can specify which applications you want to include in the package.</span></span> <span data-ttu-id="f2fac-212">可以指定多个。</span><span class="sxs-lookup"><span data-stu-id="f2fac-212">It can be more than one.</span></span>

![打包项目的结构](./media/deploy-modern-applications/packaging-project.png)

<span data-ttu-id="f2fac-214">右键单击 Applications 文件夹，从 Visual Studio 解决方案中选择要打包的 Windows 窗体项目。</span><span class="sxs-lookup"><span data-stu-id="f2fac-214">Right-click on the *Applications* folder and select the Windows Forms project you want to package from the Visual Studio solution.</span></span>

![将 Windows 窗体项目添加到打包项目中](./media/deploy-modern-applications/add-winforms-project.png)

<span data-ttu-id="f2fac-216">此时，你可以编译并生成包，但我们要确认几件事。</span><span class="sxs-lookup"><span data-stu-id="f2fac-216">At this point, you can compile and generate the package but let's examine a couple of things.</span></span> <span data-ttu-id="f2fac-217">为了获得更好的用户体验，Visual Studio 可以自动生成新式应用程序处理磁贴栏和开始菜单的图标和磁贴资产所需的所有视觉资产。</span><span class="sxs-lookup"><span data-stu-id="f2fac-217">To have a better user experience, Visual Studio can autogenerate all the visual assets a modern application needs to handle icons and tile assets for the tile bar and start menu.</span></span> <span data-ttu-id="f2fac-218">打开 Package.appxmanifest 文件以访问清单设计器。</span><span class="sxs-lookup"><span data-stu-id="f2fac-218">Open the *Package.appxmanifest* file to access the Manifest Designer.</span></span> <span data-ttu-id="f2fac-219">然后，只需单击“创建”，即可根据项目中的给定图片生成所有视觉资产。</span><span class="sxs-lookup"><span data-stu-id="f2fac-219">You can then generate all the visual assets from a given image present on your project just by clicking **Create**.</span></span>

![Visual Studio 中清单设计器的屏幕截图](./media/deploy-modern-applications/manifest-designer.png)

<span data-ttu-id="f2fac-221">如果打开 Package.appxmanifest 文件的代码，你会看到一些有趣的事情。</span><span class="sxs-lookup"><span data-stu-id="f2fac-221">If you open the code for the *Package.appxmanifest* file, you can see a couple of interesting things.</span></span>

<span data-ttu-id="f2fac-222">就在 `<Package>` 下面，有一个 `<Identity>` 节点。</span><span class="sxs-lookup"><span data-stu-id="f2fac-222">Right under `<Package>`, there's an `<Identity>` node.</span></span> <span data-ttu-id="f2fac-223">这是打包应用程序获取标识的地方，它将由操作系统管理。</span><span class="sxs-lookup"><span data-stu-id="f2fac-223">This is where your packaged application is going to get its identity, which will be managed by the OS.</span></span>

![Identity 节点](./media/deploy-modern-applications/identity-node.png)

<span data-ttu-id="f2fac-225">在 `<Capabilities>` 节点中，你可以找到应用程序所需的所有必备组件，特别注意 `<rescap:Capability Name="runFullTrust" \>`，它会指示操作系统以完全信任模式运行应用，因为这是一个 Win32 应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-225">In the `<Capabilities>` node, you can find all the requirements the application needs, paying special attention to the `<rescap:Capability Name="runFullTrust" \>`, which tells the OS to run the app in full trust mode since it's a Win32 application.</span></span>

![Capabilities 节点](./media/deploy-modern-applications/capabilities-node.png)

<span data-ttu-id="f2fac-227">将打包项目设置为解决方案的启动项目，然后选择“运行”。</span><span class="sxs-lookup"><span data-stu-id="f2fac-227">Set the packaging project as the startup project for the solution and select *Run*.</span></span> <span data-ttu-id="f2fac-228">此操作将：</span><span class="sxs-lookup"><span data-stu-id="f2fac-228">This is going to:</span></span>

- <span data-ttu-id="f2fac-229">编译 Windows 窗体应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-229">Compile the Windows Forms application.</span></span>
- <span data-ttu-id="f2fac-230">根据生成结果创建 MSIX 包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-230">Create an MSIX package out of the build results.</span></span>
- <span data-ttu-id="f2fac-231">部署包。</span><span class="sxs-lookup"><span data-stu-id="f2fac-231">Deploy the packages.</span></span>
- <span data-ttu-id="f2fac-232">在开发计算机上进行本地安装。</span><span class="sxs-lookup"><span data-stu-id="f2fac-232">Install it locally on the development machine.</span></span>
- <span data-ttu-id="f2fac-233">启动应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-233">Launch the app.</span></span>

![我们安装的应用程序](./media/deploy-modern-applications/our-installed-application.png)

<span data-ttu-id="f2fac-235">你将借此获得 MSIX 提供的完全集成到 Windows 10 的干净安装和卸载体验。</span><span class="sxs-lookup"><span data-stu-id="f2fac-235">With this, you have the clean install and uninstall experience that MSIX provides fully integrated into Windows 10.</span></span>

<span data-ttu-id="f2fac-236">最后阶段介绍如何将 MSIX 包部署到另一台计算机。</span><span class="sxs-lookup"><span data-stu-id="f2fac-236">The final stage is about how you deploy the MSIX package to another machine.</span></span>

<span data-ttu-id="f2fac-237">右键单击打包项目，选择“Store”菜单，然后选择“创建应用包”选项。</span><span class="sxs-lookup"><span data-stu-id="f2fac-237">Right-click on the packaging project, select the **Store** menu, and then select the **Create App Packages** option.</span></span>

<span data-ttu-id="f2fac-238">然后，可以选择是创建包以上传到 Store，还是创建包以进行旁加载。</span><span class="sxs-lookup"><span data-stu-id="f2fac-238">Then, you can choose between creating a package to upload to the store or creating packages for sideloading.</span></span> <span data-ttu-id="f2fac-239">在大多数现代化方案中，你将选择“我想创建包以进行旁加载”。</span><span class="sxs-lookup"><span data-stu-id="f2fac-239">In most modernization scenarios, you'll choose **I want to create packages for sideloading**.</span></span>

![配置包](./media/deploy-modern-applications/configuring-packages.png)

<span data-ttu-id="f2fac-241">在这里，你可以选择要作为目标的不同体系结构，因为你可以在同一个 MSIX 包中包含任意数量的体系结构。</span><span class="sxs-lookup"><span data-stu-id="f2fac-241">There you can select the different architectures you want to target as you can include as many as you want into the same MSIX package.</span></span>

<span data-ttu-id="f2fac-242">最后一步是声明要在何处部署最终安装资产。</span><span class="sxs-lookup"><span data-stu-id="f2fac-242">The final step is to declare where you want to deploy the final installation assets.</span></span>

![配置更新设置](./media/deploy-modern-applications/configure-update-settings.png)

<span data-ttu-id="f2fac-244">你可以选择使用企业文件服务器上的共享 UNC 路径的 Web 服务器。</span><span class="sxs-lookup"><span data-stu-id="f2fac-244">You can choose to use a web server of a shared UNC path on your enterprise file servers.</span></span> <span data-ttu-id="f2fac-245">请注意用于指定应用程序更新方式的设置。</span><span class="sxs-lookup"><span data-stu-id="f2fac-245">Pay attention to the settings to specify how you want to update your application.</span></span> <span data-ttu-id="f2fac-246">下一部分将介绍应用程序更新。</span><span class="sxs-lookup"><span data-stu-id="f2fac-246">We'll cover application updates in the next section.</span></span>

<span data-ttu-id="f2fac-247">有关详细的分步指南，请参阅[使用 Visual Studio 从源代码打包桌面应用](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。</span><span class="sxs-lookup"><span data-stu-id="f2fac-247">For a detailed step-by-step guide, see [Package a desktop app from source code using Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).</span></span>

## <a name="auto-updates-in-msix"></a><span data-ttu-id="f2fac-248">MSIX 中的自动更新</span><span class="sxs-lookup"><span data-stu-id="f2fac-248">Auto Updates in MSIX</span></span>

<span data-ttu-id="f2fac-249">Windows 应用商店具有使用 Windows 更新的强大更新机制。</span><span class="sxs-lookup"><span data-stu-id="f2fac-249">The Windows Store has a great updating mechanism using Windows Update.</span></span> <span data-ttu-id="f2fac-250">在大多数企业方案中，你不会使用应用商店来分发桌面应用。</span><span class="sxs-lookup"><span data-stu-id="f2fac-250">In most enterprise scenarios, you don't use the Store to distribute your desktop apps.</span></span> <span data-ttu-id="f2fac-251">因此，你需要采用一种类似的方法来为应用程序配置更新并将其拉取给用户。</span><span class="sxs-lookup"><span data-stu-id="f2fac-251">So, you need a similar way to configure updates for your application and pull them to your users.</span></span>

<span data-ttu-id="f2fac-252">结合使用 Windows 10 功能和 MSIX 包，你可以为用户提供出色的更新体验。</span><span class="sxs-lookup"><span data-stu-id="f2fac-252">Using a combination of Windows 10 features and MSIX packages, you can provide a great updating experience for your users.</span></span> <span data-ttu-id="f2fac-253">事实上，用户不需要掌握任何技术，也能从无缝的应用程序更新体验中受益。</span><span class="sxs-lookup"><span data-stu-id="f2fac-253">In fact, the user doesn't need to be technical at all but still benefit from a seamless application update experience.</span></span>

<span data-ttu-id="f2fac-254">你可以配置更新，以便通过两种不同的方式与用户交互：</span><span class="sxs-lookup"><span data-stu-id="f2fac-254">You can configure your updates to interact with the user in two different ways:</span></span>

- <span data-ttu-id="f2fac-255">用户提示的更新：操作系统显示一些自动生成的精美 UI，以通知用户即将安装应用程序。</span><span class="sxs-lookup"><span data-stu-id="f2fac-255">User prompted updates: The OS shows some autogenerated nice UI to notify the user about the application is about to install.</span></span> <span data-ttu-id="f2fac-256">它基于你在安装文件上指定的属性来生成此 UI。</span><span class="sxs-lookup"><span data-stu-id="f2fac-256">It builds this UI based on the properties you specify on your installation files.</span></span>

- <span data-ttu-id="f2fac-257">后台无提示更新。</span><span class="sxs-lookup"><span data-stu-id="f2fac-257">Silent updates in the background.</span></span> <span data-ttu-id="f2fac-258">使用此选项，你的用户无需知道这些更新。</span><span class="sxs-lookup"><span data-stu-id="f2fac-258">With this option, your users don't need to be aware of the updates.</span></span>

<span data-ttu-id="f2fac-259">你还可以配置何时执行更新，是应用程序启动时更新还是定期更新。</span><span class="sxs-lookup"><span data-stu-id="f2fac-259">You can also configure when you want to perform updates, when the application launches or on a regular basis.</span></span> <span data-ttu-id="f2fac-260">凭借旁加载功能，你甚至可以在应用程序运行时获取这些更新。</span><span class="sxs-lookup"><span data-stu-id="f2fac-260">Thanks to the side-loading features, you can even get these updates while the application is running.</span></span>

<span data-ttu-id="f2fac-261">使用这种类型的部署时，系统会创建一个名为 .appinstaller 的特殊文件。</span><span class="sxs-lookup"><span data-stu-id="f2fac-261">When you use this type of deployment, a special file is created called *.appinstaller*.</span></span> <span data-ttu-id="f2fac-262">这个简单文件包含以下几部分：</span><span class="sxs-lookup"><span data-stu-id="f2fac-262">This simple file contains the following sections:</span></span>

- <span data-ttu-id="f2fac-263">.appinstaller 文件的位置</span><span class="sxs-lookup"><span data-stu-id="f2fac-263">The location of the *.appinstaller* file</span></span>
- <span data-ttu-id="f2fac-264">应用程序的主要 MSIX 包属性</span><span class="sxs-lookup"><span data-stu-id="f2fac-264">The application's main MSIX package properties</span></span>
- <span data-ttu-id="f2fac-265">更新行为</span><span class="sxs-lookup"><span data-stu-id="f2fac-265">The update behavior</span></span>

![.appinstaller 文件](./media/deploy-modern-applications/appinstaller-file.png)

<span data-ttu-id="f2fac-267">结合此文件，Microsoft 设计了一个特殊的 URL 协议，以从链接启动安装过程：</span><span class="sxs-lookup"><span data-stu-id="f2fac-267">In combination with this file, Microsoft has designed a special URL protocol to launch the installation process from a link:</span></span>

```xml
<a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/MyApp.msix">Install app package </a>
```

<span data-ttu-id="f2fac-268">此协议适用于所有浏览器，并在 Windows 10 上以良好的用户体验启动安装过程。</span><span class="sxs-lookup"><span data-stu-id="f2fac-268">This protocol works on all browsers and launches the installation process with a great user experience on Windows 10.</span></span> <span data-ttu-id="f2fac-269">由于操作系统负责管理安装过程，因此它知道此应用程序的安装位置，并跟踪受该过程影响的所有文件。</span><span class="sxs-lookup"><span data-stu-id="f2fac-269">Since the OS manages the installation process, it's aware of the location this application was installed from and tracks all the files affected by the process.</span></span>

<span data-ttu-id="f2fac-270">MSIX 会创建一个用于安装的用户界面，以自动显示包的某些属性。</span><span class="sxs-lookup"><span data-stu-id="f2fac-270">MSIX creates a user interface for installation automatically showing some properties of the package.</span></span> <span data-ttu-id="f2fac-271">这样可以为每个应用提供通用的安装体验。</span><span class="sxs-lookup"><span data-stu-id="f2fac-271">This allows for a common installation experience for every app.</span></span>

<span data-ttu-id="f2fac-272">生成新的 MSIX 包并将其迁移到部署服务器后，只需编辑 .appinstaller 文件以反映这些更改，主要包括版本和新 MSIX 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="f2fac-272">Once you've generated the new MSIX package and moved it to the deployment server, you just have to edit the *.appinstaller* file to reflect these changes, mainly the version and the path to the new MSIX file.</span></span> <span data-ttu-id="f2fac-273">下次用户启动应用程序时，系统将检测到更改并在后台下载新版本的文件。</span><span class="sxs-lookup"><span data-stu-id="f2fac-273">The next time the user launches the application, the system is going to detect the change and download the files for the new version in the background.</span></span> <span data-ttu-id="f2fac-274">完成此操作后，将在新应用程序启动时以对用户透明的方式执行安装。</span><span class="sxs-lookup"><span data-stu-id="f2fac-274">When this is done, installation will execute on new application launch transparently for your user.</span></span>

>[!div class="step-by-step"]
>[<span data-ttu-id="f2fac-275">上一页</span><span class="sxs-lookup"><span data-stu-id="f2fac-275">Previous</span></span>](example-migration.md)
