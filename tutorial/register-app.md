<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="34c1d-101">Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o centro de administração do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="34c1d-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="34c1d-102">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="34c1d-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="34c1d-103">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="34c1d-104">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="34c1d-105">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="34c1d-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="34c1d-106">Selecione **novo registro**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-106">Select **New registration**.</span></span> <span data-ttu-id="34c1d-107">Na página **registrar um aplicativo** , defina os valores da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="34c1d-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="34c1d-108">Defina \*\*\*\* o nome `Angular Graph Tutorial`como.</span><span class="sxs-lookup"><span data-stu-id="34c1d-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="34c1d-109">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="34c1d-110">Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:4200`valor como.</span><span class="sxs-lookup"><span data-stu-id="34c1d-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. <span data-ttu-id="34c1d-112">Escolha **registrar**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-112">Choose **Register**.</span></span> <span data-ttu-id="34c1d-113">Na página **tutorial do gráfico angular** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="34c1d-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)

1. <span data-ttu-id="34c1d-115">Selecione **autenticação** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="34c1d-116">Localize a seção **Grant implícita** e habilite tokens de **acesso** e tokens de **ID**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="34c1d-117">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="34c1d-117">Choose **Save**.</span></span>

    ![Uma captura de tela da seção Grant implícita](./images/aad-implicit-grant.png)
