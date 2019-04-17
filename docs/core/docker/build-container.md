---
title: Docker öğreticisiyle bir uygulamayı kapsayıcılı hale getirme
description: Bu öğreticide, bir .NET Core uygulamasını Docker ile kapsayıcılı hale getirme öğreneceksiniz.
ms.date: 04/10/2019
ms.topic: tutorial
ms.custom: mvc, seodec18
ms.openlocfilehash: fcbac0e0d17d2481d42e715a7f2790586e31d085
ms.sourcegitcommit: 8080271c246b57f4fb68c28369634bff46843424
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59553842"
---
# <a name="tutorial-containerize-a-net-core-app"></a><span data-ttu-id="c6b76-103">Öğretici: Kapsayıcılı bir .NET Core uygulaması</span><span class="sxs-lookup"><span data-stu-id="c6b76-103">Tutorial: Containerize a .NET Core app</span></span>

<span data-ttu-id="c6b76-104">Bu öğreticide, .NET Core uygulamanızı içeren bir Docker görüntüsü oluşturmayı öğretiyor.</span><span class="sxs-lookup"><span data-stu-id="c6b76-104">This tutorial teaches you how to build a Docker image that contains your .NET Core application.</span></span> <span data-ttu-id="c6b76-105">Görüntü, kapsayıcılar, yerel geliştirme ortamı, özel Bulut veya genel bulut oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-105">The image can be used to create containers for your local development environment, private cloud, or public cloud.</span></span>

<span data-ttu-id="c6b76-106">Şunları öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="c6b76-106">You'll learn to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6b76-107">Oluşturma ve basit bir .NET Core uygulaması yayımlama</span><span class="sxs-lookup"><span data-stu-id="c6b76-107">Create and publish a simple .NET Core app</span></span>
> * <span data-ttu-id="c6b76-108">Oluşturma ve .NET Core için bir Dockerfile'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c6b76-108">Create and configure a Dockerfile for .NET Core</span></span>
> * <span data-ttu-id="c6b76-109">Bir Docker görüntüsü oluşturun</span><span class="sxs-lookup"><span data-stu-id="c6b76-109">Build a Docker image</span></span>
> * <span data-ttu-id="c6b76-110">Oluşturma ve bir Docker kapsayıcısında çalıştırma</span><span class="sxs-lookup"><span data-stu-id="c6b76-110">Create and run a Docker container</span></span>

<span data-ttu-id="c6b76-111">Docker kapsayıcı derleme ve dağıtım görevleri için bir .NET Core uygulaması anlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c6b76-111">You'll understand the Docker container build and deploy tasks for a .NET Core application.</span></span> <span data-ttu-id="c6b76-112">*Docker platformu* kullanan *Docker altyapısı* oluşturmayı ve uygulamaları olarak paketini *Docker görüntülerini*.</span><span class="sxs-lookup"><span data-stu-id="c6b76-112">The *Docker platform* uses the *Docker engine* to quickly build and package apps as *Docker images*.</span></span> <span data-ttu-id="c6b76-113">Bu görüntüleri yazılan *Dockerfile* dağıtılabilir ve katmanlı kapsayıcısında çalıştırma biçimi.</span><span class="sxs-lookup"><span data-stu-id="c6b76-113">These images are written in the *Dockerfile* format to be deployed and run in a layered container.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6b76-114">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c6b76-114">Prerequisites</span></span>

<span data-ttu-id="c6b76-115">Aşağıdaki önkoşulları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="c6b76-115">Install the following prerequisites:</span></span>

- <span data-ttu-id="c6b76-116">[.NET core SDK'sını 2.2](https://dotnet.microsoft.com/download)\\</span><span class="sxs-lookup"><span data-stu-id="c6b76-116">[.NET Core 2.2 SDK](https://dotnet.microsoft.com/download)\\</span></span>
<span data-ttu-id="c6b76-117">.NET Core yüklü varsa `dotnet --info` kullandığınız hangi SDK belirlemek için komutu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-117">If you have .NET Core installed, use the `dotnet --info` command to determine which SDK you're using.</span></span>

- [<span data-ttu-id="c6b76-118">Docker Community Edition</span><span class="sxs-lookup"><span data-stu-id="c6b76-118">Docker Community Edition</span></span>](https://www.docker.com/products/docker-desktop)

- <span data-ttu-id="c6b76-119">Geçici bir çalışma dizini için *Dockerfile* ve .NET Core örnek uygulaması.</span><span class="sxs-lookup"><span data-stu-id="c6b76-119">A temporary working directory for the *Dockerfile* and .NET Core example app.</span></span>

### <a name="use-sdk-version-22"></a><span data-ttu-id="c6b76-120">SDK 2.2 sürümünü kullanın</span><span class="sxs-lookup"><span data-stu-id="c6b76-120">Use SDK version 2.2</span></span>

<span data-ttu-id="c6b76-121">3.0 gibi yeni bir SDK kullanıyorsanız, uygulamanızı 2.2 SDK'yı kullanmaya zorlanır emin olun.</span><span class="sxs-lookup"><span data-stu-id="c6b76-121">If you're using an SDK that is newer, like 3.0, make sure that your app is forced to use the 2.2 SDK.</span></span> <span data-ttu-id="c6b76-122">Adlı bir dosya oluşturun `global.json` çalışma dizinine gidin ve aşağıdaki json kodunu yapıştırın:</span><span class="sxs-lookup"><span data-stu-id="c6b76-122">Create a file named `global.json` in your working directory and paste in the following json code:</span></span>

```json
{
  "sdk": {
    "version": "2.2.100"
  }
}
```

<span data-ttu-id="c6b76-123">Bu dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-123">Save this file.</span></span> <span data-ttu-id="c6b76-124">Sürüm 2.2 için kullanılacak .NET Core dosyasının varlığını zorlayacak `dotnet` bu dizinden ve aşağıdaki komutu olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-124">The presence of file will force .NET Core to use version 2.2 for any `dotnet` command called from this directory and below.</span></span>

## <a name="create-net-core-app"></a><span data-ttu-id="c6b76-125">Oluşturma .NET Core uygulaması</span><span class="sxs-lookup"><span data-stu-id="c6b76-125">Create .NET Core app</span></span>

<span data-ttu-id="c6b76-126">Docker kapsayıcısı çalıştıracak bir .NET Core uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-126">You need a .NET Core app that the Docker container will run.</span></span> <span data-ttu-id="c6b76-127">Terminalinizi açın, bir çalışma dizini oluşturun ve bunu girin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-127">Open your terminal, create a working directory, and enter it.</span></span> <span data-ttu-id="c6b76-128">Çalışma dizininde, uygulama adlı bir alt dizinde yeni bir proje oluşturmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c6b76-128">In the working directory, run the following command to create a new project in a subdirectory named app:</span></span>

```console
dotnet new console -o app -n myapp
```

<span data-ttu-id="c6b76-129">Komut adlı yeni bir dizin oluşturur *uygulama* ve bir "Hello World" uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6b76-129">That command creates a new directory named *app* and generates a "Hello World" app.</span></span> <span data-ttu-id="c6b76-130">Neler yaptığını görmek için bu uygulamayı test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-130">You can test this app to see what it does.</span></span> <span data-ttu-id="c6b76-131">Girin *uygulama* dizin ve bağlamını `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-131">Enter the *app* directory and execute the command `dotnet run`.</span></span> <span data-ttu-id="c6b76-132">Aşağıdaki çıktıyı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="c6b76-132">You'll see the following output:</span></span>

```console
> dotnet run
Hello World!
```

<span data-ttu-id="c6b76-133">Varsayılan şablonu, terminale yazdırır ve kapanır bir uygulama oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6b76-133">The default template creates an app that prints to the terminal and then exits.</span></span> <span data-ttu-id="c6b76-134">Bu öğreticide, sonsuza kadar döngü bir uygulama kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-134">For this tutorial, you'll use an app that loops indefinitely.</span></span> <span data-ttu-id="c6b76-135">Açık **Program.cs** dosyasını bir metin düzenleyicisinde.</span><span class="sxs-lookup"><span data-stu-id="c6b76-135">Open the **Program.cs** file in a text editor.</span></span> <span data-ttu-id="c6b76-136">Bu, şu anda şu kod gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="c6b76-136">It should currently look like the following code:</span></span>

```csharp
using System;

namespace myapp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

<span data-ttu-id="c6b76-137">Dosya, her saniye sayısını sayar aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c6b76-137">Replace the file with the following code that counts numbers every second:</span></span>

```csharp
using System;

namespace myapp
{
    class Program
    {
        static void Main(string[] args)
        {
            var counter = 0;
            var max = args.Length != 0 ? Convert.ToInt32(args[0]) : -1;
            while(max == -1 || counter < max)
            {
                counter++;
                Console.WriteLine($"Counter: {counter}");
                System.Threading.Tasks.Task.Delay(1000).Wait();
            }
        }
    }
}
```

<span data-ttu-id="c6b76-138">Dosyayı kaydedin ve programı yeniden test `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-138">Save the file and test the program again with `dotnet run`.</span></span> <span data-ttu-id="c6b76-139">Bu uygulama süresiz olarak çalıştığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-139">Remember that this app runs indefinitely.</span></span> <span data-ttu-id="c6b76-140">Cancel komutu kullanma <kbd>CTRL + C</kbd> durdurmak ister.</span><span class="sxs-lookup"><span data-stu-id="c6b76-140">Use the cancel command <kbd>CTRL + C</kbd> to stop it.</span></span> <span data-ttu-id="c6b76-141">Aşağıdaki çıktıyı görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="c6b76-141">You'll see the following output:</span></span>

```console
> dotnet run
Counter: 1
Counter: 2
Counter: 3
Counter: 4
^C
```

<span data-ttu-id="c6b76-142">Uygulama için komut satırında bir sayı geçirirseniz, bunu yalnızca kadar tutar ve ardından çıkış sayılır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-142">If you pass a number on the command line to the app, it will only count up to that amount and then exit.</span></span> <span data-ttu-id="c6b76-143">İle deneyin `dotnet run -- 5` beş sayısı.</span><span class="sxs-lookup"><span data-stu-id="c6b76-143">Try it with `dotnet run -- 5` to count to five.</span></span>

> [!NOTE]
> <span data-ttu-id="c6b76-144">Sonra herhangi bir parametre `--` uygulamanıza geçirilir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-144">Any parameters after `--` are passed to your application.</span></span>

## <a name="publish-net-core-app"></a><span data-ttu-id="c6b76-145">Yayımlama .NET Core uygulaması</span><span class="sxs-lookup"><span data-stu-id="c6b76-145">Publish .NET Core app</span></span>

<span data-ttu-id="c6b76-146">.NET Core uygulamanızı Docker görüntüsüne eklemeden önce yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-146">Before you add your .NET Core app to the Docker image, publish it.</span></span> <span data-ttu-id="c6b76-147">Başlatıldığında kapsayıcı uygulamanın yayımlanmış sürümü çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-147">The container will execute the published version of the app when it's started.</span></span>

<span data-ttu-id="c6b76-148">Çalışma dizinine girin **uygulama** directory örneği ile kaynak kodu ve şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c6b76-148">From the working directory, enter the **app** directory with the example source code and run the following command:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="c6b76-149">Bu komut, uygulamanızın derler **yayımlama** uygulamanızın çıkış klasöründeki klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-149">This command compiles your app to the **publish** folder in the output folder of your app.</span></span> <span data-ttu-id="c6b76-150">Yolu **yayımlama** klasöründen çalışma dizini olmalıdır. `.\app\bin\Release\netcoreapp2.2\publish\`</span><span class="sxs-lookup"><span data-stu-id="c6b76-150">The path to the **publish** folder from the working directory should be `.\app\bin\Release\netcoreapp2.2\publish\`</span></span>

<span data-ttu-id="c6b76-151">Doğrulamak için yayımlama klasörünün dizin bir listesini **myapp.dll** oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-151">Get a directory listing of the publish folder to verify that the **myapp.dll** was created.</span></span> <span data-ttu-id="c6b76-152">Gelen **uygulama** dizin, aşağıdaki komutlardan birini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c6b76-152">From the **app** directory, run one of the following commands:</span></span>

```console
> dir bin\Release\netcoreapp2.2\publish
 Directory of C:\path-to-working-dir\app\bin\Release\netcoreapp2.2\publish

04/05/2019  11:00 AM    <DIR>          .
04/05/2019  11:00 AM    <DIR>          ..
04/05/2019  11:00 AM               447 myapp.deps.json
04/05/2019  11:00 AM             4,608 myapp.dll
04/05/2019  11:00 AM               448 myapp.pdb
04/05/2019  11:00 AM               154 myapp.runtimeconfig.json
```

```bash
me@DESKTOP:/path-to-working-dir/app$ ls bin/Release/netcoreapp2.2/publish
myapp.deps.json  myapp.dll  myapp.pdb  myapp.runtimeconfig.json
```

<span data-ttu-id="c6b76-153">Terminalinizde bir dizini çalışma dizinine gidin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-153">In your terminal, go up a directory to the working directory.</span></span>

## <a name="create-the-dockerfile"></a><span data-ttu-id="c6b76-154">Dockerfile'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="c6b76-154">Create the Dockerfile</span></span>

<span data-ttu-id="c6b76-155">*Dockerfile* dosya tarafından kullanılan `docker build` bir kapsayıcı görüntüsü oluşturmak için komutu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-155">The *Dockerfile* file is used by the `docker build` command to create a container image.</span></span> <span data-ttu-id="c6b76-156">Adlı bir düz metin dosyası bu dosyadır *Dockerfile* uzantı yok.</span><span class="sxs-lookup"><span data-stu-id="c6b76-156">This file is a plaintext file named *Dockerfile* that does not have an extension.</span></span> <span data-ttu-id="c6b76-157">Adlı bir dosya oluşturun *Dockerfile* çalışma dizininizdeki ve bir metin düzenleyicisinde açın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-157">Create a file named *Dockerfile* in your working directory and open it in a text editor.</span></span> <span data-ttu-id="c6b76-158">Aşağıdaki komut dosyasının ilk satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c6b76-158">Add the following command as the first line of the file:</span></span>

```dockerfile
FROM mcr.microsoft.com/dotnet/core/runtime:2.2
```

<span data-ttu-id="c6b76-159">`FROM` Komutu bildirir etiketli görüntüyü çekmek için Docker **2.2** gelen **mcr.microsoft.com/dotnet/core/runtime** depo.</span><span class="sxs-lookup"><span data-stu-id="c6b76-159">The `FROM` command tells Docker to pull down the image tagged **2.2** from the **mcr.microsoft.com/dotnet/core/runtime** repository.</span></span> <span data-ttu-id="c6b76-160">Çalışma zamanı, SDK'sı tarafından hedeflenen eşleşen .NET Core çalışma zamanı çekme emin olun.</span><span class="sxs-lookup"><span data-stu-id="c6b76-160">Make sure that you pull the .NET Core runtime that matches the runtime targeted by your SDK.</span></span> <span data-ttu-id="c6b76-161">Örneğin, önceki oluşturulan uygulama bölümünde kullanılan .NET Core 2.2 SDK ve .NET Core 2.2 hedefleyen bir uygulama oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-161">For example, the app created in the previous section used the .NET Core 2.2 SDK and created an app that targeted .NET Core 2.2.</span></span> <span data-ttu-id="c6b76-162">Temel görüntü başvurulan şekilde *Dockerfile* ile etiketlenmiş **2.2**.</span><span class="sxs-lookup"><span data-stu-id="c6b76-162">So the base image referred to in the *Dockerfile* is tagged with **2.2**.</span></span>

<span data-ttu-id="c6b76-163">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-163">Save the file.</span></span> <span data-ttu-id="c6b76-164">Uygulamanızı terminalden çalıştırın `docker build -t myimage .` ve Docker, her satır işleyecek *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="c6b76-164">From your terminal, run `docker build -t myimage .` and Docker will process each line in the *Dockerfile*.</span></span> <span data-ttu-id="c6b76-165">`.` İçinde `docker build` komut geçerli dizinde bulmak için kullanmak üzere docker söyleyen bir *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="c6b76-165">The `.` in the `docker build` command tells docker to use the current directory to find a *Dockerfile*.</span></span> <span data-ttu-id="c6b76-166">Bu komut, görüntüyü oluşturur ve adlı yerel bir depo oluşturur **myımage** görüntüsünü işaret eder.</span><span class="sxs-lookup"><span data-stu-id="c6b76-166">This command builds the image and creates a local repository named **myimage** that points to that image.</span></span> <span data-ttu-id="c6b76-167">Bu komut bittikten sonra Çalıştır `docker images` yüklü görüntülerin listesini görmek için:</span><span class="sxs-lookup"><span data-stu-id="c6b76-167">After this command finishes, run `docker images` to see a list of images installed:</span></span>

```console
> docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
mcr.microsoft.com/dotnet/core/runtime   2.2                 d51bb4452469        2 days ago          314MB
myimage                                 latest              d51bb4452469        2 days ago          314MB
```

<span data-ttu-id="c6b76-168">İki görüntü aynı paylaşım bildirimi **görüntü kimliği** değeri.</span><span class="sxs-lookup"><span data-stu-id="c6b76-168">Notice that the two images share the same **IMAGE ID** value.</span></span> <span data-ttu-id="c6b76-169">Değer her iki görüntü arasında aynı çünkü yalnızca komutta *Dockerfile* üzerinde mevcut bir görüntüyü yeni görüntüyü temel oluşturmaktı.</span><span class="sxs-lookup"><span data-stu-id="c6b76-169">The value is the same between both images because the only command in the *Dockerfile* was to base the new image on an existing image.</span></span> <span data-ttu-id="c6b76-170">İki komutu ekleyelim *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="c6b76-170">Let's add two commands to the *Dockerfile*.</span></span> <span data-ttu-id="c6b76-171">Her komut, görüntünün temsil eden son komutu ile yeni bir görüntü katmanı oluşturur **myımage** depo işaret.</span><span class="sxs-lookup"><span data-stu-id="c6b76-171">Each command creates a new image layer with the final command representing the image the **myimage** repository will point to.</span></span>

```dockerfile
COPY app/bin/Release/netcoreapp2.2/publish/ app/

ENTRYPOINT ["dotnet", "app/myapp.dll"]
```

<span data-ttu-id="c6b76-172">`COPY` Komut Docker kapsayıcısında bir klasöre bilgisayarınız üzerindeki belirtilen klasöre kopyalamak için bildirir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-172">The `COPY` command tells Docker to copy the specified folder on your computer to a folder in the container.</span></span> <span data-ttu-id="c6b76-173">Bu örnekte, **yayımlama** klasör adlı bir klasöre kopyalanır **uygulama** kapsayıcısında.</span><span class="sxs-lookup"><span data-stu-id="c6b76-173">In this example, the **publish** folder is copied to a folder named **app** in the container.</span></span>

<span data-ttu-id="c6b76-174">Sonraki komut `ENTRYPOINT`, yürütülebilir dosya olarak çalıştırmak için kapsayıcı yapılandırmak için docker söyler.</span><span class="sxs-lookup"><span data-stu-id="c6b76-174">The next command, `ENTRYPOINT`, tells docker to configure the container to run as an executable.</span></span> <span data-ttu-id="c6b76-175">Kapsayıcı başlatıldığında, `ENTRYPOINT` komutunu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-175">When the container starts, the `ENTRYPOINT` command runs.</span></span> <span data-ttu-id="c6b76-176">Bu komut sona erdiğinde, kapsayıcı otomatik olarak durdurulur.</span><span class="sxs-lookup"><span data-stu-id="c6b76-176">When this command ends, the container will automatically stop.</span></span>

<span data-ttu-id="c6b76-177">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-177">Save the file.</span></span> <span data-ttu-id="c6b76-178">Uygulamanızı terminalden çalıştırın `docker build -t myimage .` ve tamamlandığında, komut, çalıştırılabilir `docker images`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-178">From your terminal, run `docker build -t myimage .` and when that command finishes, run `docker images`.</span></span>

```console
> docker build -t myimage .
Sending build context to Docker daemon  819.7kB
Step 1/3 : FROM mcr.microsoft.com/dotnet/core/runtime:2.2
 ---> d51bb4452469
Step 2/3 : COPY app/bin/Release/netcoreapp2.2/publish/ app/
 ---> a1e98ac62017
Step 3/3 : ENTRYPOINT ["dotnet", "app/myapp.dll"]
 ---> Running in f34da5c18e7c
Removing intermediate container f34da5c18e7c
 ---> ddcc6646461b
Successfully built ddcc6646461b
Successfully tagged myimage:latest


> docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
myimage                                 latest              ddcc6646461b        10 seconds ago      314MB
mcr.microsoft.com/dotnet/core/runtime   2.2                 d51bb4452469        2 days ago          314MB
```

<span data-ttu-id="c6b76-179">Her komut, *Dockerfile* katmanındaki oluşturulur ve oluşturulan bir **görüntü kimliği**.</span><span class="sxs-lookup"><span data-stu-id="c6b76-179">Each command in the *Dockerfile* generated a layer and created an **IMAGE ID**.</span></span> <span data-ttu-id="c6b76-180">En son **görüntü kimliği** (sizinki farklı olacaktır) olan **ddcc6646461b** ve ardından bu görüntüyü temel alarak bir kapsayıcı oluşturursunuz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-180">The final **IMAGE ID** (yours will be different) is **ddcc6646461b** and next you'll create a container based on this image.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="c6b76-181">Bir kapsayıcı oluşturma</span><span class="sxs-lookup"><span data-stu-id="c6b76-181">Create a container</span></span>

<span data-ttu-id="c6b76-182">Uygulamanızı içeren bir görüntünüz olduğuna göre bir kapsayıcı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-182">Now that you have an image that contains your app, you can create a container.</span></span> <span data-ttu-id="c6b76-183">Bir kapsayıcı iki şekilde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-183">You can create a container in two ways.</span></span> <span data-ttu-id="c6b76-184">İlk olarak durduruldu yeni bir kapsayıcı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c6b76-184">First, create a new container that is stopped.</span></span>

```console
> docker create myimage
0e8f3c2ca32ce773712a5cca38750f41259a4e54e04bdf0946087e230ad7066c
```

<span data-ttu-id="c6b76-185">`docker create` Yukarıdaki komut, temel bir kapsayıcı oluşturur **myımage** görüntü.</span><span class="sxs-lookup"><span data-stu-id="c6b76-185">The `docker create` command from above will create a container based on the **myimage** image.</span></span> <span data-ttu-id="c6b76-186">Bu komutun çıktısı, gösterir **KAPSAYICI kimliği** (sizinki farklı olacaktır) oluşturulan kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="c6b76-186">The output of that command shows you the **CONTAINER ID** (yours will be different) of the created container.</span></span> <span data-ttu-id="c6b76-187">Bir listesini görmek için *tüm* kapsayıcılarını, `docker ps -a` komutu:</span><span class="sxs-lookup"><span data-stu-id="c6b76-187">To see a list of *all* containers, use the `docker ps -a` command:</span></span>

```console
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS        PORTS   NAMES
0e8f3c2ca32c        myimage             "dotnet app/myapp.dll"   4 seconds ago       Created               boring_matsumoto
```

### <a name="manage-the-container"></a><span data-ttu-id="c6b76-188">Kapsayıcı yönetme</span><span class="sxs-lookup"><span data-stu-id="c6b76-188">Manage the container</span></span>

<span data-ttu-id="c6b76-189">Her kapsayıcı, bir kapsayıcı örneğe atıfta için kullanabileceğiniz rastgele bir ad atanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-189">Each container is assigned a random name that you can use to refer to that container instance.</span></span> <span data-ttu-id="c6b76-190">Örneğin, otomatik olarak oluşturulan kapsayıcı adı seçtiğiniz **boring_matsumoto** (sizinki farklı olacaktır) ve bu ad, kapsayıcı başlatma için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-190">For example, the container that was created automatically chose the name **boring_matsumoto** (yours will be different) and that name can be used to start the container.</span></span> <span data-ttu-id="c6b76-191">Otomatik adıyla kullanarak belirli bir geçersiz kılma `docker create --name` parametresi.</span><span class="sxs-lookup"><span data-stu-id="c6b76-191">You override the automatic name with a specific one by using the `docker create --name` parameter.</span></span>

<span data-ttu-id="c6b76-192">Aşağıdaki örnekte `docker start` kapsayıcı ve ardından kullandığı başlatmak için komut `docker ps` komutu yalnızca çalışan kapsayıcılar göstermek için:</span><span class="sxs-lookup"><span data-stu-id="c6b76-192">The following example uses the `docker start` command to start the container, and then uses the `docker ps` command to only show containers that are running:</span></span>

```console
> docker start boring_matsumoto
boring_matsumoto

> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS         PORTS   NAMES
0e8f3c2ca32c        myimage             "dotnet app/myapp.dll"   7 minutes ago       Up 8 seconds           boring_matsumoto
```

<span data-ttu-id="c6b76-193">Benzer şekilde, `docker stop` komutu, kapsayıcı durdurur.</span><span class="sxs-lookup"><span data-stu-id="c6b76-193">Similarly, the `docker stop` command will stop the container.</span></span> <span data-ttu-id="c6b76-194">Aşağıdaki örnekte `docker stop` kapsayıcı ve ardından kullandığı durdurmak için komutu `docker ps` kapsayıcı çalıştığını göstermek için komutu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-194">The following example uses the `docker stop` command to stop the container, and then uses the `docker ps` command to show that no containers are running.</span></span>

```console
> docker stop boring_matsumoto
boring_matsumoto

> docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS     PORTS   NAMES
```

### <a name="connect-to-a-container"></a><span data-ttu-id="c6b76-195">Bir kapsayıcıya bağlanma</span><span class="sxs-lookup"><span data-stu-id="c6b76-195">Connect to a container</span></span>

<span data-ttu-id="c6b76-196">Bir kapsayıcı çalışmaya başladıktan sonra çıktıyı görmek için bağlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-196">After a container is running, you can connect to it to see the output.</span></span> <span data-ttu-id="c6b76-197">Kullanım `docker start` ve `docker attach` kapsayıcı ve çıkış akışına göz at'ı başlatmak için komutları.</span><span class="sxs-lookup"><span data-stu-id="c6b76-197">Use the `docker start` and `docker attach` commands to start the container and peek at the output stream.</span></span> <span data-ttu-id="c6b76-198">Bu örnekte, <kbd>CTRL + C</kbd> komutu, çalışmakta olan kapsayıcıyı ayırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-198">In this example, the <kbd>CTRL + C</kbd> command is used to detach from the running container.</span></span> <span data-ttu-id="c6b76-199">Bu, gerçekten kapsayıcı durdurur kapsayıcı işlemde sonlandırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-199">This may actually end the process in the container, which will stop the container.</span></span> <span data-ttu-id="c6b76-200">`--sig-proxy=false` Parametresi sağlar <kbd>CTRL + C</kbd> kapsayıcısında işlemi durdurur.</span><span class="sxs-lookup"><span data-stu-id="c6b76-200">The `--sig-proxy=false` parameter ensures that <kbd>CTRL + C</kbd> will not stop the process in the container.</span></span>

<span data-ttu-id="c6b76-201">Kapsayıcıdan ayırdıktan sonra bunu hala çalışan sayım ve olduğunu doğrulamak için yeniden bağlayın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-201">After you detach from the container, reattach to verify that it's still running and counting.</span></span>

```console
> docker start boring_matsumoto
boring_matsumoto

> docker attach --sig-proxy=false boring_matsumoto
Counter: 7
Counter: 8
Counter: 9
^C

> docker attach --sig-proxy=false boring_matsumoto
Counter: 17
Counter: 18
Counter: 19
^C
```

### <a name="delete-a-container"></a><span data-ttu-id="c6b76-202">Bir kapsayıcıyı Sil</span><span class="sxs-lookup"><span data-stu-id="c6b76-202">Delete a container</span></span>

<span data-ttu-id="c6b76-203">Bu makalenin amaçları yalnızca geçici bir çözüm birşey asılı kapsayıcıları istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-203">For the purposes of this article you don't want containers just hanging around doing nothing.</span></span> <span data-ttu-id="c6b76-204">Daha önce oluşturduğunuz kapsayıcıya silin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-204">Delete the container you previously created.</span></span> <span data-ttu-id="c6b76-205">Kapsayıcı çalışıyorsa durdurun.</span><span class="sxs-lookup"><span data-stu-id="c6b76-205">If the container is running, stop it.</span></span>

```console
> docker stop boring_matsumoto
```

<span data-ttu-id="c6b76-206">Aşağıdaki örnek, tüm kapsayıcıları listeler.</span><span class="sxs-lookup"><span data-stu-id="c6b76-206">The following example lists all containers.</span></span> <span data-ttu-id="c6b76-207">Ardından kullanır `docker rm` kapsayıcısını silme komutunu ve ardından ikinci bir kez çalıştıran tüm kapsayıcıları için denetler.</span><span class="sxs-lookup"><span data-stu-id="c6b76-207">It then uses the `docker rm` command to delete the container, and then checks a second time for any running containers.</span></span>

```console
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS     PORTS   NAMES
0e8f3c2ca32c        myimage             "dotnet app/myapp.dll"   19 minutes ago      Exited             boring_matsumoto

> docker rm boring_matsumoto
boring_matsumoto

> docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS     PORTS    NAMES
```

### <a name="single-run"></a><span data-ttu-id="c6b76-208">Tek çalışma</span><span class="sxs-lookup"><span data-stu-id="c6b76-208">Single run</span></span>

<span data-ttu-id="c6b76-209">Docker sağlar `docker run` oluşturma ve kapsayıcı tek bir komut çalıştırmak için komutu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-209">Docker provides the `docker run` command to create and run the container as a single command.</span></span> <span data-ttu-id="c6b76-210">Bu komutu çalıştırma ihtiyacı ortadan kaldırır. `docker create` ardından `docker start`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-210">This command eliminates the need to run `docker create` and then `docker start`.</span></span> <span data-ttu-id="c6b76-211">Kapsayıcı durduğunda kapsayıcı otomatik olarak silmek için bu komutu da ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6b76-211">You can also set this command to automatically delete the container when the container stops.</span></span> <span data-ttu-id="c6b76-212">Örneğin, `docker run -it --rm` iki işlem gerçekleştirmesi için ilk olarak, otomatik olarak kapsayıcıya bağlanmak için geçerli terminal kullanın ve kapsayıcı sona erdiğinde, kaldırın:</span><span class="sxs-lookup"><span data-stu-id="c6b76-212">For example, use `docker run -it --rm` to do two things, first, automatically use the current terminal to connect to the container, and then when the container finishes, remove it:</span></span>

```
> docker run -it --rm myimage
Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
^C
```

<span data-ttu-id="c6b76-213">İle `docker run -it`, <kbd>CTRL + C</kbd> komutu sırayla kapsayıcısını durdurur kapsayıcı içinde çalışmakta olan işlemi durdurur.</span><span class="sxs-lookup"><span data-stu-id="c6b76-213">With `docker run -it`, the <kbd>CTRL + C</kbd> command will stop process that is running in the container, which in turn, stops the container.</span></span> <span data-ttu-id="c6b76-214">Bu yana `--rm` parametresi sağlanmadığından, işlem durduğunda kapsayıcı otomatik olarak silinir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-214">Since the `--rm` parameter was provided, the container is automatically deleted when the process is stopped.</span></span> <span data-ttu-id="c6b76-215">Mevcut olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="c6b76-215">Verify that it does not exist:</span></span>

```
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS    PORTS   NAMES
```

### <a name="change-the-entrypoint"></a><span data-ttu-id="c6b76-216">Giriş noktası değiştirme</span><span class="sxs-lookup"><span data-stu-id="c6b76-216">Change the ENTRYPOINT</span></span>

<span data-ttu-id="c6b76-217">`docker run` Komut ayrıca, değiştirmenize imkan tanır `ENTRYPOINT` komutunu *Dockerfile* ve başka bir ancak için yalnızca o kapsayıcı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-217">The `docker run` command also lets you modify the `ENTRYPOINT` command from the *Dockerfile* and run something else, but only for that container.</span></span> <span data-ttu-id="c6b76-218">Örneğin, çalıştırmak için aşağıdaki komutu kullanın `bash` veya `cmd.exe`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-218">For example, use the following command to run `bash` or `cmd.exe`.</span></span> <span data-ttu-id="c6b76-219">Komutu, gerektiği gibi düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-219">Edit the command as necessary.</span></span>

#### <a name="windows"></a><span data-ttu-id="c6b76-220">Windows</span><span class="sxs-lookup"><span data-stu-id="c6b76-220">Windows</span></span>
<span data-ttu-id="c6b76-221">Bu örnekte `ENTRYPOINT` değiştirilir `cmd.exe`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-221">In this example the `ENTRYPOINT` is changed to `cmd.exe`.</span></span> <span data-ttu-id="c6b76-222"><kbd>CTRL + C</kbd> işlemi sona erdirmek ve kapsayıcıyı durdurmak için basılan.</span><span class="sxs-lookup"><span data-stu-id="c6b76-222"><kbd>CTRL + C</kbd> is pressed to end the process and stop the container.</span></span>

```console
> docker run -it --rm --entrypoint "cmd.exe" myimage

Microsoft Windows [Version 10.0.17763.379]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 3005-1E84

 Directory of C:\

04/09/2019  08:46 AM    <DIR>          app
03/07/2019  10:25 AM             5,510 License.txt
04/02/2019  01:35 PM    <DIR>          Program Files
04/09/2019  01:06 PM    <DIR>          Users
04/02/2019  01:35 PM    <DIR>          Windows
               1 File(s)          5,510 bytes
               4 Dir(s)  21,246,517,248 bytes free

C:\>^C
```

#### <a name="linux"></a><span data-ttu-id="c6b76-223">Linux</span><span class="sxs-lookup"><span data-stu-id="c6b76-223">Linux</span></span>

<span data-ttu-id="c6b76-224">Bu örnekte `ENTRYPOINT` değiştirilir `bash`.</span><span class="sxs-lookup"><span data-stu-id="c6b76-224">In this example the `ENTRYPOINT` is changed to `bash`.</span></span> <span data-ttu-id="c6b76-225">`quit` İşlemi sonlandırır ve kapsayıcı Durdur komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-225">The `quit` command is run which ends the process and stop the container.</span></span>

```bash
root@user:~# docker run -it --rm --entrypoint "bash" myimage
root@8515e897c893:/# ls app
myapp.deps.json  myapp.dll  myapp.pdb  myapp.runtimeconfig.json
root@8515e897c893:/# exit
exit
```

## <a name="essential-commands"></a><span data-ttu-id="c6b76-226">Gerekli komutları</span><span class="sxs-lookup"><span data-stu-id="c6b76-226">Essential commands</span></span>

<span data-ttu-id="c6b76-227">Docker, kapsayıcı ve görüntülerle yapmak istediğiniz kapsayan çok sayıda farklı komutlar vardır.</span><span class="sxs-lookup"><span data-stu-id="c6b76-227">Docker has many different commands that cover what you want to do with your container and images.</span></span> <span data-ttu-id="c6b76-228">Bu Docker komutlarını kapsayıcıları yönetmek için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="c6b76-228">These Docker commands are essential to managing your containers:</span></span>

* [<span data-ttu-id="c6b76-229">docker derleme</span><span class="sxs-lookup"><span data-stu-id="c6b76-229">docker build</span></span>](https://docs.docker.com/engine/reference/commandline/build/)
* [<span data-ttu-id="c6b76-230">docker Run</span><span class="sxs-lookup"><span data-stu-id="c6b76-230">docker run</span></span>](https://docs.docker.com/engine/reference/commandline/run/)
* [<span data-ttu-id="c6b76-231">docker ps</span><span class="sxs-lookup"><span data-stu-id="c6b76-231">docker ps</span></span>](https://docs.docker.com/engine/reference/commandline/ps/)
* [<span data-ttu-id="c6b76-232">docker durdurma</span><span class="sxs-lookup"><span data-stu-id="c6b76-232">docker stop</span></span>](https://docs.docker.com/engine/reference/commandline/stop/)
* [<span data-ttu-id="c6b76-233">docker rm</span><span class="sxs-lookup"><span data-stu-id="c6b76-233">docker rm</span></span>](https://docs.docker.com/engine/reference/commandline/rm/)
* [<span data-ttu-id="c6b76-234">docker RMI</span><span class="sxs-lookup"><span data-stu-id="c6b76-234">docker rmi</span></span>](https://docs.docker.com/engine/reference/commandline/rmi/)
* [<span data-ttu-id="c6b76-235">docker görüntüsü</span><span class="sxs-lookup"><span data-stu-id="c6b76-235">docker image</span></span>](https://docs.docker.com/engine/reference/commandline/image/)

## <a name="clean-up-resources"></a><span data-ttu-id="c6b76-236">Kaynakları temizleme</span><span class="sxs-lookup"><span data-stu-id="c6b76-236">Clean up resources</span></span>

<span data-ttu-id="c6b76-237">Bu öğretici sırasında kapsayıcılar ve görüntüleri oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-237">During this tutorial you created containers and images.</span></span> <span data-ttu-id="c6b76-238">İsterseniz, bu kaynakları silin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-238">If you want, delete these resources.</span></span> <span data-ttu-id="c6b76-239">İçin aşağıdaki komutları kullanın</span><span class="sxs-lookup"><span data-stu-id="c6b76-239">Use the following commands to</span></span>

01. <span data-ttu-id="c6b76-240">Tüm kapsayıcıları listesi</span><span class="sxs-lookup"><span data-stu-id="c6b76-240">List all containers</span></span>

    ```console
    > docker ps -a
    ```

02. <span data-ttu-id="c6b76-241">Çalışan kapsayıcıları durdurmak.</span><span class="sxs-lookup"><span data-stu-id="c6b76-241">Stop containers that are running.</span></span> <span data-ttu-id="c6b76-242">`CONTAINER_NAME` Kapsayıcıya otomatik olarak atanan adını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6b76-242">The `CONTAINER_NAME` represents the name automatically assigned to the container.</span></span>

    ```console
    > docker stop CONTAINER_NAME
    ```

03. <span data-ttu-id="c6b76-243">Kapsayıcıyı Sil</span><span class="sxs-lookup"><span data-stu-id="c6b76-243">Delete the container</span></span>

    ```console
    > docker rm CONTAINER_NAME
    ```

<span data-ttu-id="c6b76-244">Ardından, makinenizde artık istemediğiniz tüm görüntüleri silin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-244">Next, delete any images that you no longer want on your machine.</span></span> <span data-ttu-id="c6b76-245">Tarafından oluşturulan görüntüyü silin, *Dockerfile* ve .NET Core görüntüyü silin *Dockerfile* temel.</span><span class="sxs-lookup"><span data-stu-id="c6b76-245">Delete the image created by your *Dockerfile* and then delete the .NET Core image the *Dockerfile* was based on.</span></span> <span data-ttu-id="c6b76-246">Kullanabileceğiniz **görüntü kimliği** veya **DEPO: Etiket** biçimlendirilmiş dize.</span><span class="sxs-lookup"><span data-stu-id="c6b76-246">You can use the **IMAGE ID** or the **REPOSITORY:TAG** formatted string.</span></span>

```console
docker rmi myimage:latest
docker rmi mcr.microsoft.com/dotnet/core/runtime:2.2
```

<span data-ttu-id="c6b76-247">Kullanım `docker images` yüklü görüntülerin listesini görmek için komutu.</span><span class="sxs-lookup"><span data-stu-id="c6b76-247">Use the `docker images` command to see a list of images installed.</span></span>

> [!NOTE]
> <span data-ttu-id="c6b76-248">Görüntü dosyaları büyük olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6b76-248">Image files can be large.</span></span> <span data-ttu-id="c6b76-249">Genellikle, test ve uygulama geliştirme sırasında oluşturulan geçici kapsayıcıları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="c6b76-249">Typically, you would remove temporary containers you created while testing and developing your app.</span></span> <span data-ttu-id="c6b76-250">Genellikle, bu çalışma zamanına göre diğer görüntüleri oluşturmaya düşünüyorsanız yüklü olan çalışma zamanı ile temel görüntüleri tutun.</span><span class="sxs-lookup"><span data-stu-id="c6b76-250">You usually keep the base images with the runtime installed if you plan on building other images based on that runtime.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6b76-251">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c6b76-251">Next steps</span></span>

* [<span data-ttu-id="c6b76-252">ASP.NET Core mikro hizmet öğreticisini deneyin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-252">Try the ASP.NET Core Microservice Tutorial.</span></span>](https://dotnet.microsoft.com/learn/web/aspnet-microservice-tutorial/intro)
* [<span data-ttu-id="c6b76-253">Kapsayıcıları destekleyen Azure Hizmetleri gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="c6b76-253">Review the Azure services that support containers.</span></span>](https://azure.microsoft.com/en-us/overview/containers/)
* [<span data-ttu-id="c6b76-254">Dockerfile komutlarının hakkında okuyun.</span><span class="sxs-lookup"><span data-stu-id="c6b76-254">Read about Dockerfile commands.</span></span>](https://docs.docker.com/engine/reference/builder/)