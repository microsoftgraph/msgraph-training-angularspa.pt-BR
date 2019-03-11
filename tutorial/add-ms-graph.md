<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

Comece criando uma `Event` classe que define os campos que o aplicativo exibirá. Crie um novo arquivo no `./src/app` diretório chamado `event.ts` e adicione o código a seguir.

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

Em seguida, adicione um novo serviço para manter todas as chamadas de gráfico. Assim como o serviço de autenticação que você criou anteriormente, a criação de um serviço para isso permite que você o insira em qualquer componente que precise acessar o Microsoft Graph. Execute o seguinte comando em sua CLI.

```Shell
ng generate service graph
```

Quando o comando for concluído, abra o `./src/app/graph.service.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

Considere o que esse código está fazendo.

- Ele Inicializa um cliente do Graph no construtor para o serviço.
- Ele implementa uma `getEvents` função que usa o cliente gráfico da seguinte maneira:
  - A URL que será chamada é `/me/events`.
  - O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
  - O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

Agora, crie um componente angular para chamar este novo método e exibir os resultados da chamada. Execute o seguinte comando em sua CLI.

```Shell
ng generate component calendar
```

Depois que o comando for concluído, adicione o componente à `routes` matriz no `./src/app/app-routing.module.ts`.

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

Abra o `./src/app/calendar/calendar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

Por ora, isso apenas renderiza a matriz de eventos em JSON na página. Salve suas alterações e reinicie o aplicativo. Entre e clique no link **calendário** na barra de navegação. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode atualizar o `CalendarComponent` componente para exibir os eventos de forma mais amigável. Primeiro, remova o código temporário que adiciona um alerta da `ngOnInit` função. A função updated deve ter a seguinte aparência.

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

Agora, adicione uma função à `CalendarComponent` classe para formatar um `DateTimeTimeZone` objeto em uma cadeia de caracteres ISO.

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

Por fim, abra `./src/app/calendar/calendar.component.html` o arquivo e substitua seu conteúdo pelo seguinte.

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

Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um. Salve as alterações e reinicie o aplicativo. Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)