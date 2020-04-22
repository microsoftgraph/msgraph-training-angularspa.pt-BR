<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5cafb-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5cafb-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="5cafb-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5cafb-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="5cafb-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="5cafb-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="5cafb-104">Crie um novo arquivo no `./src/app` diretório chamado `event.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5cafb-104">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. <span data-ttu-id="5cafb-105">Adicione um novo serviço para manter todas as chamadas de gráfico.</span><span class="sxs-lookup"><span data-stu-id="5cafb-105">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="5cafb-106">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="5cafb-106">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="5cafb-107">Assim como o serviço de autenticação que você criou anteriormente, a criação de um serviço para isso permite que você o insira em qualquer componente que precise acessar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5cafb-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="5cafb-108">Quando o comando for concluído, abra o `./src/app/graph.service.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5cafb-108">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    <span data-ttu-id="5cafb-109">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="5cafb-109">Consider what this code is doing.</span></span>

    - <span data-ttu-id="5cafb-110">Ele Inicializa um cliente do Graph no construtor para o serviço.</span><span class="sxs-lookup"><span data-stu-id="5cafb-110">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="5cafb-111">Ele implementa uma `getEvents` função que usa o cliente gráfico da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5cafb-111">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="5cafb-112">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="5cafb-112">The URL that will be called is `/me/events`.</span></span>
      - <span data-ttu-id="5cafb-113">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="5cafb-113">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="5cafb-114">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="5cafb-114">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="5cafb-115">Criar um componente angular para chamar este novo método e exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="5cafb-115">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="5cafb-116">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="5cafb-116">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="5cafb-117">Depois que o comando for concluído, adicione o componente à `routes` matriz no `./src/app/app-routing.module.ts`.</span><span class="sxs-lookup"><span data-stu-id="5cafb-117">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="5cafb-118">Abra o `./src/app/calendar/calendar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5cafb-118">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';

    import { GraphService } from '../graph.service';
    import { Event, DateTimeTimeZone } from '../event';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: Event[];

      constructor(
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        this.graphService.getEvents()
          .then((events) => {
            this.events = events;
            // Temporary to display raw results
            this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
          });
      }
    }
    ```

<span data-ttu-id="5cafb-119">Por ora, isso apenas renderiza a matriz de eventos em JSON na página.</span><span class="sxs-lookup"><span data-stu-id="5cafb-119">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="5cafb-120">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5cafb-120">Save your changes and restart the app.</span></span> <span data-ttu-id="5cafb-121">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="5cafb-121">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="5cafb-122">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="5cafb-122">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="5cafb-123">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="5cafb-123">Display the results</span></span>

<span data-ttu-id="5cafb-124">Agora você pode atualizar o `CalendarComponent` componente para exibir os eventos de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="5cafb-124">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="5cafb-125">Remova o código temporário que adiciona um alerta da `ngOnInit` função.</span><span class="sxs-lookup"><span data-stu-id="5cafb-125">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="5cafb-126">A função updated deve ter a seguinte aparência.</span><span class="sxs-lookup"><span data-stu-id="5cafb-126">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="5cafb-127">Adicione uma função à `CalendarComponent` classe para formatar um `DateTimeTimeZone` objeto em uma cadeia de caracteres ISO.</span><span class="sxs-lookup"><span data-stu-id="5cafb-127">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="5cafb-128">Abra o `./src/app/calendar/calendar.component.html` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5cafb-128">Open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="5cafb-129">Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="5cafb-129">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="5cafb-130">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5cafb-130">Save the changes and restart the app.</span></span> <span data-ttu-id="5cafb-131">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="5cafb-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
