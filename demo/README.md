# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="92d52-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="92d52-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92d52-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="92d52-102">Prerequisites</span></span>

<span data-ttu-id="92d52-103">Para executar o projeto concluído nesta pasta, você precisa do seguinte:</span><span class="sxs-lookup"><span data-stu-id="92d52-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="92d52-104">[Node.js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="92d52-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="92d52-105">Se você não tiver Node.js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="92d52-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="92d52-106">(**Observação:** este tutorial foi escrito com o Nó versão 14.15.0.</span><span class="sxs-lookup"><span data-stu-id="92d52-106">(**Note:** This tutorial was written with Node version 14.15.0.</span></span> <span data-ttu-id="92d52-107">As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)</span><span class="sxs-lookup"><span data-stu-id="92d52-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="92d52-108">[CLI angular](https://cli.angular.io/) instalada em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="92d52-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="92d52-109">Uma conta pessoal da Microsoft com uma caixa de correio Outlook.com ou uma conta de estudante ou de trabalho da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="92d52-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="92d52-110">Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="92d52-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="92d52-111">Você pode [se inscrever em uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="92d52-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="92d52-112">Você pode se inscrever no Programa de Desenvolvedores do [Office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="92d52-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="92d52-113">Registrar um aplicativo Web no Centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="92d52-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="92d52-114">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="92d52-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="92d52-115">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="92d52-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="92d52-116">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="92d52-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="92d52-117">Uma captura de tela dos registros do aplicativo</span><span class="sxs-lookup"><span data-stu-id="92d52-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="92d52-118">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="92d52-118">Select **New registration**.</span></span> <span data-ttu-id="92d52-119">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="92d52-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="92d52-120">Defina **Nome** para `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="92d52-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="92d52-121">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="92d52-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="92d52-122">Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Single-page application (SPA)` e defina o valor como `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="92d52-122">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:4200`.</span></span>

    ![Captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="92d52-124">Escolha **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="92d52-124">Choose **Register**.</span></span> <span data-ttu-id="92d52-125">Na página **Tutorial do Angular Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dela na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="92d52-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="92d52-127">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="92d52-127">Configure the sample</span></span>

1. <span data-ttu-id="92d52-128">Renomeie o `oauth.ts.example` arquivo para `oauth.ts` .</span><span class="sxs-lookup"><span data-stu-id="92d52-128">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="92d52-129">Edite `oauth.ts` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="92d52-129">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="92d52-130">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo que** você recebeu do Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="92d52-130">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="92d52-131">Em sua interface de linha de comando (CLI), navegue até esse diretório e execute o seguinte comando para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="92d52-131">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="92d52-132">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="92d52-132">Run the sample</span></span>

1. <span data-ttu-id="92d52-133">Execute o seguinte comando em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="92d52-133">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="92d52-134">Abra um navegador e navegue até `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="92d52-134">Open a browser and browse to `http://localhost:4200`.</span></span>
