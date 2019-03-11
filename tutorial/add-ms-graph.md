<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9d60e-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9d60e-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="9d60e-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="9d60e-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="9d60e-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="9d60e-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="9d60e-104">Comece criando uma `Event` classe que define os campos que o aplicativo exibirá.</span><span class="sxs-lookup"><span data-stu-id="9d60e-104">Start by creating an `Event` class that defines the fields that the app will display.</span></span> <span data-ttu-id="9d60e-105">Crie um novo arquivo no `./src/app` diretório chamado `event.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="9d60e-105">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

```TypeScript
// For a full list of fields, see
// https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0
export class Event {
  subject: string;
  organizer: Recipient;
  start: DateTimeTimeZone;
  end: DateTimeTimeZone;
}

// https://docs.microsoft.com/graph/api/resources/recipient?view=graph-rest-1.0
export class Recipient {
  emailAddress: EmailAddress;
}

// https://docs.microsoft.com/graph/api/resources/emailaddress?view=graph-rest-1.0
export class EmailAddress {
  name: string;
  address: string;
}

// https://docs.microsoft.com/graph/api/resources/datetimetimezone?view=graph-rest-1.0
export class DateTimeTimeZone {
  dateTime: string;
  timeZone: string;
}
```

<span data-ttu-id="9d60e-106">Em seguida, adicione um novo serviço para manter todas as chamadas de gráfico.</span><span class="sxs-lookup"><span data-stu-id="9d60e-106">Next, add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="9d60e-107">Assim como o serviço de autenticação que você criou anteriormente, a criação de um serviço para isso permite que você o insira em qualquer componente que precise acessar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="9d60e-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span> <span data-ttu-id="9d60e-108">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="9d60e-108">Run the following command in your CLI.</span></span>

```Shell
ng generate service graph
```

<span data-ttu-id="9d60e-109">Quando o comando for concluído, abra o `./src/app/graph.service.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="9d60e-109">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Injectable } from '@angular/core';
import { Client } from '@microsoft/microsoft-graph-client';

import { AuthService } from './auth.service';
import { Event } from './event';
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
        let token = await this.authService.getAccessToken()
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

  async getEvents(): Promise<Event[]> {
    try {
      let result =  await this.graphClient
        .api('/me/events')
        .select('subject,organizer,start,end')
        .orderby('createdDateTime DESC')
        .get();

      return result.value;
    } catch (error) {
      this.alertsService.add('Could not get events', JSON.stringify(error, null, 2));
    }
  }
}
```

<span data-ttu-id="9d60e-110">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="9d60e-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="9d60e-111">Ele Inicializa um cliente do Graph no construtor para o serviço.</span><span class="sxs-lookup"><span data-stu-id="9d60e-111">It initializes a Graph client in the constructor for the service.</span></span>
- <span data-ttu-id="9d60e-112">Ele implementa uma `getEvents` função que usa o cliente gráfico da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9d60e-112">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
  - <span data-ttu-id="9d60e-113">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="9d60e-113">The URL that will be called is `/me/events`.</span></span>
  - <span data-ttu-id="9d60e-114">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="9d60e-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
  - <span data-ttu-id="9d60e-115">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="9d60e-115">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="9d60e-116">Agora, crie um componente angular para chamar este novo método e exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="9d60e-116">Now create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="9d60e-117">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="9d60e-117">Run the following command in your CLI.</span></span>

```Shell
ng generate component calendar
```

<span data-ttu-id="9d60e-118">Depois que o comando for concluído, adicione o componente à `routes` matriz no `./src/app/app-routing.module.ts`.</span><span class="sxs-lookup"><span data-stu-id="9d60e-118">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

<span data-ttu-id="9d60e-119">Abra o `./src/app/calendar/calendar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="9d60e-119">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

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

  private events: Event[];

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

<span data-ttu-id="9d60e-120">Por ora, isso apenas renderiza a matriz de eventos em JSON na página.</span><span class="sxs-lookup"><span data-stu-id="9d60e-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="9d60e-121">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9d60e-121">Save your changes and restart the app.</span></span> <span data-ttu-id="9d60e-122">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="9d60e-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="9d60e-123">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="9d60e-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="9d60e-124">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="9d60e-124">Display the results</span></span>

<span data-ttu-id="9d60e-125">Agora você pode atualizar o `CalendarComponent` componente para exibir os eventos de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="9d60e-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="9d60e-126">Primeiro, remova o código temporário que adiciona um alerta da `ngOnInit` função.</span><span class="sxs-lookup"><span data-stu-id="9d60e-126">First, remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="9d60e-127">A função updated deve ter a seguinte aparência.</span><span class="sxs-lookup"><span data-stu-id="9d60e-127">Your updated function should look like this.</span></span>

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

<span data-ttu-id="9d60e-128">Agora, adicione uma função à `CalendarComponent` classe para formatar um `DateTimeTimeZone` objeto em uma cadeia de caracteres ISO.</span><span class="sxs-lookup"><span data-stu-id="9d60e-128">Now add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

```TypeScript
formatDateTimeTimeZone(dateTime: DateTimeTimeZone): string {
  try {
    return moment.tz(dateTime.dateTime, dateTime.timeZone).format();
  }
  catch(error) {
    this.alertsService.add('DateTimeTimeZone conversion error', JSON.stringify(error));
  }
}
```

<span data-ttu-id="9d60e-129">Por fim, abra `./src/app/calendar/calendar.component.html` o arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="9d60e-129">Finally, open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <th scope="col">Organizer</th>
    <th scope="col">Subject</th>
    <th scope="col">Start</th>
    <th scope="col">End</th>
  </thead>
  <tbody>
    <tr *ngFor="let event of events">
      <td>{{event.organizer.emailAddress.name}}</td>
      <td>{{event.subject}}</td>
      <td>{{formatDateTimeTimeZone(event.start) | date:'short' }}</td>
      <td>{{formatDateTimeTimeZone(event.end) | date: 'short' }}</td>
    </tr>
  </tbody>
</table>
```

<span data-ttu-id="9d60e-130">Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="9d60e-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="9d60e-131">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9d60e-131">Save the changes and restart the app.</span></span> <span data-ttu-id="9d60e-132">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="9d60e-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)