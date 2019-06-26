<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="af4e5-101">Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o centro de administração do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="af4e5-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="af4e5-102">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af4e5-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="af4e5-103">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="af4e5-104">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="af4e5-105">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="af4e5-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="af4e5-106">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-106">Select **New registration**.</span></span> <span data-ttu-id="af4e5-107">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="af4e5-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="af4e5-108">Defina **Nome** para `Angular Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="af4e5-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="af4e5-109">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="af4e5-110">Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Web` e defina o valor como `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="af4e5-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. <span data-ttu-id="af4e5-112">Selecione **registrar**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-112">Select **Register**.</span></span> <span data-ttu-id="af4e5-113">Na página **tutorial do gráfico angular** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="af4e5-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)

1. <span data-ttu-id="af4e5-115">Selecione **Autenticação** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="af4e5-116">Localize a seção **Grant implícita** e habilite tokens de **acesso** e tokens de **ID**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="af4e5-117">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="af4e5-117">Select **Save**.</span></span>

    ![Uma captura de tela da seção Grant implícita](./images/aad-implicit-grant.png)
