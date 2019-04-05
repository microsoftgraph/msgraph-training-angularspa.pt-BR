# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="c4a6a-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c4a6a-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4a6a-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c4a6a-102">Prerequisites</span></span>

<span data-ttu-id="c4a6a-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="c4a6a-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="c4a6a-104">[Node. js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="c4a6a-105">Se você não tiver o Node. js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="c4a6a-106">(**Observação:** este tutorial foi escrito com o nó versão 10.7.0.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="c4a6a-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="c4a6a-108">[CLI angular](https://cli.angular.io/) instalada em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="c4a6a-109">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="c4a6a-110">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="c4a6a-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="c4a6a-111">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="c4a6a-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="c4a6a-112">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="c4a6a-113">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4a6a-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="c4a6a-114">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c4a6a-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="c4a6a-115">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="c4a6a-116">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="c4a6a-117">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="c4a6a-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="c4a6a-118">Selecione **novo registro**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-118">Select **New registration**.</span></span> <span data-ttu-id="c4a6a-119">Na página **registrar um aplicativo** , defina os valores da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="c4a6a-120">Defina \*\*\*\* o nome `Angular Graph Tutorial`como.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="c4a6a-121">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="c4a6a-122">Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:4200`valor como.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-122">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="c4a6a-124">Escolha **registrar**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-124">Choose **Register**.</span></span> <span data-ttu-id="c4a6a-125">Na página **tutorial do gráfico angular** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="c4a6a-127">Selecione **autenticação** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-127">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="c4a6a-128">Localize a seção **Grant implícita** e habilite tokens de **acesso** e tokens de **ID**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-128">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="c4a6a-129">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-129">Choose **Save**.</span></span>

    ![Uma captura de tela da seção Grant implícita](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="c4a6a-131">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="c4a6a-131">Configure the sample</span></span>

1. <span data-ttu-id="c4a6a-132">ReNomear `oauth.ts.example` o `oauth.ts`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-132">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="c4a6a-133">Edite `oauth.ts` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-133">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="c4a6a-134">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-134">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="c4a6a-135">Na sua interface de linha de comando (CLI), navegue até este diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-135">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="c4a6a-136">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="c4a6a-136">Run the sample</span></span>

1. <span data-ttu-id="c4a6a-137">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-137">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="c4a6a-138">Abra um navegador e navegue até `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="c4a6a-138">Open a browser and browse to `http://localhost:4200`.</span></span>