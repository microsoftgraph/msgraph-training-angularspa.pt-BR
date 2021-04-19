<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ecb99-101">Neste exercício, você incorporará o Microsoft Graph ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecb99-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="ecb99-102">Para esse aplicativo, você usará a biblioteca [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ecb99-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="ecb99-103">Obtenha eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="ecb99-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="ecb99-104">Adicione um novo serviço para manter todas as chamadas do Graph.</span><span class="sxs-lookup"><span data-stu-id="ecb99-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="ecb99-105">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="ecb99-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="ecb99-106">Assim como com o serviço de autenticação criado anteriormente, a criação de um serviço para isso permite que você o injete em todos os componentes que precisem de acesso ao Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ecb99-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="ecb99-107">Depois que o comando é concluído, abra **./src/app/graph.service.ts** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ecb99-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from './auth.service';
    import { AlertsService } from './alerts.service';

    @Injectable({
      providedIn: 'root'
    })

    export class GraphService {

      private graphClient: Client;
      constructor(
        private authService: AuthService,
        private alertsService: AlertsService) {

        // Initialize the Graph client
        this.graphClient = Client.init({
          authProvider: async (done) => {
            // Get the token from the auth service
            const token = await this.authService.getAccessToken()
              .catch((reason) => {
                done(reason, null);
              });

            if (token)
            {
              done(null, token);
            } else {
              done("Could not get an access token", null);
            }
          }
        });
      }

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[] | undefined> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          const result =  await this.graphClient
            .api('/me/calendarview')
            .header('Prefer', `outlook.timezone="${timeZone}"`)
            .query({
              startDateTime: start,
              endDateTime: end
            })
            .select('subject,organizer,start,end')
            .orderby('start/dateTime')
            .top(50)
            .get();

          return result.value;
        } catch (error) {
          this.alertsService.addError('Could not get events', JSON.stringify(error, null, 2));
        }
        return undefined;
      }
    }
    ```

    <span data-ttu-id="ecb99-108">Considere o que este código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="ecb99-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="ecb99-109">Ele inicializa um cliente Graph no construtor do serviço.</span><span class="sxs-lookup"><span data-stu-id="ecb99-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="ecb99-110">Ele implementa uma `getCalendarView` função que usa o cliente Graph da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ecb99-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="ecb99-111">O URL que será chamado é `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="ecb99-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="ecb99-112">O `header` método inclui o header, que faz com que os horários de início e término dos eventos retornados sejam no fuso horário preferencial `Prefer: outlook.timezone` do usuário.</span><span class="sxs-lookup"><span data-stu-id="ecb99-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="ecb99-113">O `query` método adiciona os `startDateTime` `endDateTime` parâmetros e, definindo a janela de tempo para o modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="ecb99-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="ecb99-114">O método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição `select` realmente usará.</span><span class="sxs-lookup"><span data-stu-id="ecb99-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="ecb99-115">O `orderby` método classifica os resultados por hora de início.</span><span class="sxs-lookup"><span data-stu-id="ecb99-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="ecb99-116">Crie um componente Angular para chamar esse novo método e exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="ecb99-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="ecb99-117">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="ecb99-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="ecb99-118">Depois que o comando é concluído, adicione o componente à `routes` matriz em **./src/app/app-routing.module.ts**.</span><span class="sxs-lookup"><span data-stu-id="ecb99-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. <span data-ttu-id="ecb99-119">Abra **./tsconfig.jse** adicione a propriedade a seguir ao `compilerOptions` objeto.</span><span class="sxs-lookup"><span data-stu-id="ecb99-119">Open **./tsconfig.json** and add the following property to the `compilerOptions` object.</span></span>

    ```json
    "resolveJsonModule": true
    ```

1. <span data-ttu-id="ecb99-120">Abra **./src/app/calendar/calendar.component.ts** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ecb99-120">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findIana } from 'windows-iana';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from '../auth.service';
    import { GraphService } from '../graph.service';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events?: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findIana(this.authService.user?.timeZone ?? 'UTC');
        const timeZone = ianaName![0].valueOf() || this.authService.user?.timeZone || 'UTC';

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user?.timeZone ?? 'UTC')
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

<span data-ttu-id="ecb99-121">Por enquanto, isso apenas renderiza a matriz de eventos no JSON na página.</span><span class="sxs-lookup"><span data-stu-id="ecb99-121">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="ecb99-122">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecb99-122">Save your changes and restart the app.</span></span> <span data-ttu-id="ecb99-123">Entre e clique no link **Calendário** na barra de nav.</span><span class="sxs-lookup"><span data-stu-id="ecb99-123">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="ecb99-124">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="ecb99-124">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="ecb99-125">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="ecb99-125">Display the results</span></span>

<span data-ttu-id="ecb99-126">Agora você pode atualizar o componente para exibir os eventos de maneira mais `CalendarComponent` fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="ecb99-126">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="ecb99-127">Remova o código temporário que adiciona um alerta da `ngOnInit` função.</span><span class="sxs-lookup"><span data-stu-id="ecb99-127">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="ecb99-128">Sua função atualizada deve ter esta aparência.</span><span class="sxs-lookup"><span data-stu-id="ecb99-128">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="ecb99-129">Adicione uma função à classe `CalendarComponent` para formatar `DateTimeTimeZone` um objeto em uma cadeia de caracteres ISO.</span><span class="sxs-lookup"><span data-stu-id="ecb99-129">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="ecb99-130">Abra **./src/app/calendar/calendar.component.html** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="ecb99-130">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="ecb99-131">Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um deles.</span><span class="sxs-lookup"><span data-stu-id="ecb99-131">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="ecb99-132">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecb99-132">Save the changes and restart the app.</span></span> <span data-ttu-id="ecb99-133">Clique no link **Calendário** e o aplicativo deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="ecb99-133">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
