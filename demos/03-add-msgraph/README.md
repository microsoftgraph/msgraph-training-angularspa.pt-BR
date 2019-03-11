# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="18a55-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="18a55-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18a55-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="18a55-102">Prerequisites</span></span>

<span data-ttu-id="18a55-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="18a55-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="18a55-104">[Node. js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="18a55-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="18a55-105">Se você não tiver o Node. js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="18a55-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="18a55-106">(**Observação:** este tutorial foi escrito com o nó versão 10.7.0.</span><span class="sxs-lookup"><span data-stu-id="18a55-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="18a55-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="18a55-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="18a55-108">[CLI angular](https://cli.angular.io/) instalada em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="18a55-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="18a55-109">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="18a55-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="18a55-110">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="18a55-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="18a55-111">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="18a55-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="18a55-112">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="18a55-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="18a55-113">Registrar um aplicativo Web com o portal de registro do aplicativo</span><span class="sxs-lookup"><span data-stu-id="18a55-113">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="18a55-114">Abra um navegador e navegue até o [portal de registro do aplicativo](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="18a55-114">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="18a55-115">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="18a55-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="18a55-116">Selecione **Adicionar um aplicativo** na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="18a55-116">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="18a55-117">**Observação:** Se você vir mais de um botão **Adicionar um aplicativo** na página, selecione aquele que corresponde à lista **aplicativos convergidos** .</span><span class="sxs-lookup"><span data-stu-id="18a55-117">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="18a55-118">Na página **registrar seu aplicativo** , defina o tutorial de **nome do aplicativo** para **gráfico angular** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="18a55-118">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Captura de tela da criação de um novo aplicativo no site do portal de registro de aplicativo](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="18a55-120">Na página **registro do tutorial do gráfico angular** , na seção **Propriedades** , copie a **ID do aplicativo** , pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="18a55-120">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de tela da ID do aplicativo recém-criado](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="18a55-122">Role para baixo até a seção **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="18a55-122">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="18a55-123">Selecione **Adicionar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="18a55-123">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="18a55-124">Na caixa de diálogo **Adicionar plataforma** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="18a55-124">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de tela criando uma plataforma para o aplicativo](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="18a55-126">Na caixa plataforma **da Web** , digite a URL `http://localhost:4200` das **URLs**de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="18a55-126">In the **Web** platform box, enter the URL `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Captura de tela da nova plataforma Web adicionada para o aplicativo](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="18a55-128">Role até a parte inferior da página e selecione **salvar**.</span><span class="sxs-lookup"><span data-stu-id="18a55-128">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="18a55-129">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="18a55-129">Configure the sample</span></span>

1. <span data-ttu-id="18a55-130">ReNomear `oauth.ts.example` o `oauth.ts`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="18a55-130">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="18a55-131">Edite `oauth.ts` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="18a55-131">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="18a55-132">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="18a55-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="18a55-133">Na sua interface de linha de comando (CLI), navegue até este diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="18a55-133">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="18a55-134">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="18a55-134">Run the sample</span></span>

1. <span data-ttu-id="18a55-135">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="18a55-135">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="18a55-136">Abra um navegador e navegue até `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="18a55-136">Open a browser and browse to `http://localhost:4200`.</span></span>