# Crm

En liten webapp for kundebehandling (CRM). Dette er startpunktet for API-oppgaven (1B) i Delmodul 1. Koden er med vilje enkel, og du skal bygge den ut og forbedre den steg for steg.

## Hva den gjør

- **Kunder:** liste med søk og statusfilter, detaljside, opprett, endre og slett.
- **Aktiviteter:** registrer møter, samtaler, e-poster og oppgaver for en kunde, merk dem som ferdige, og se alle åpne aktiviteter i én oversikt. Forfalte aktiviteter er markert.
- **API (JSON):** to endepunkter som leverer kundedata til andre programmer. Du bygger ut resten selv i oppgaven.

## Krav

- [.NET 10 SDK](https://dotnet.microsoft.com/download)
- Visual Studio (en nyere versjon, som støtter `.http`-filer), eller en annen editor sammen med kommandolinjeverktøyet `dotnet`

Sjekk at installasjonen fungerer:

```
dotnet --version
```

## Lag din egen kopi

1. Klikk **Use this template** øverst på denne siden, og deretter **Create a new repository**.
2. Klone det nye repoet ditt:

```
git clone <url-to-your-repository>
```

## Kjør appen

**Visual Studio:** åpne `Crm.slnx` (eller `Crm.csproj`) og trykk `F5`. Nettleseren åpner <http://localhost:5080>.

**Kommandolinjen:** i mappen som inneholder `Crm.csproj`:

```
dotnet restore
dotnet run
```

Åpne deretter <http://localhost:5080>.

> **Start alltid appen fra prosjektmappen.** Maler og datafiler leses med relative stier, så sidene feiler hvis appen startes fra en annen mappe.

## API-et

Endepunktene under `/api` returnerer data som JSON, ikke HTML.

| Endepunkt | Beskrivelse | Svar |
|---|---|---|
| `GET /api/customers` | Alle kunder | 200 |
| `GET /api/customers/{id}` | Én kunde | 200, eller 404 hvis kunden ikke finnes |

Navnene i JSON-svaret starter med liten bokstav (`name`, `email`), selv om `customers.json` bruker store (`Name`, `Email`). Det er standard JSON-oppførsel i ASP.NET Core.

### Test med `.http`-filer

- `Crm.http` inneholder de tre kallene fra API-videoen.
- `Assignment.http` inneholder alle kallene som tester oppgaven, med forventet statuskode i kommentaren over hvert kall. De tre første virker allerede. Resten virker etter hvert som du lager endepunktene.

Åpne filen i Visual Studio, start appen, og trykk **Send request** over et kall. Kjør kallene ovenfra og ned. Noen kall bruker variabler øverst i filen (`@newCustomerId`, `@activityId` og `@tempCustomerId`). Sett dem til id-en i svaret fra opprett-kallet før du kjører de neste. Hvis appen kjører på en annen port enn 5080, endrer du `@host`.

Statuskoden alene er ikke alltid nok. `TestResults.md` sier hva du også skal sjekke i svaret for noen av kallene.

## Prosjektstruktur

```
Program.cs          Endepunkter og hjelpefunksjoner
Models/             Customer og Activity
Services/           CustomerService og ActivityService (lagrer data i JSON-filer)
Templates/          HTML-maler med {{placeholders}}
wwwroot/            CSS og JavaScript
customers.json      Eksempelkunder (appen endrer denne filen)
Crm.http            De tre kallene fra API-videoen
Assignment.http     Alle kallene som tester oppgaven, med forventet statuskode
Log.md              Logg og designnotater som du fører underveis (del av leveransen)
TestResults.md      Resultatene fra kallene i Assignment.http (del av leveransen)
```

## Data

- `customers.json` inneholder fem eksempelkunder. Appen skriver til denne filen når du oppretter, endrer eller sletter kunder, så den vises som endret i Git. Slik gjenoppretter du eksempeldataene:

```
git checkout customers.json
```

- `activities.json` opprettes første gang du legger til en aktivitet. Den følges ikke av Git. Slett filen for å fjerne alle aktiviteter.

Nullstill dataene før du kjører hele `Assignment.http` på nytt. Standardverdiene for variablene øverst i filen stemmer bare med ferske data.

## Logg og testresultater

Repoet inneholder to filer som du fyller ut underveis. Begge er en del av leveransen, sammen med koden og `Assignment.http`.

- **`Log.md`:** før logg mens du jobber med oppgaven: feil du møter, og korte designnotater om valg du tar. Malene ligger i filen. Commit loggen sammen med koden.
- **`TestResults.md`:** før inn statuskoden du faktisk får for hvert kall i `Assignment.http`. For noen kall står det også hva du skal sjekke i svaret.

## Feilsøking

| Problem | Hva du bør sjekke |
|---|---|
| Feilen `NETSDK1004` (assets file not found) | Kjør `dotnet restore` i mappen med `Crm.csproj`, og bygg på nytt. |
| Feil eller blank side rett etter oppstart | Appen er startet fra feil mappe. Start den fra mappen med `Crm.csproj`. |
| Port 5080 er opptatt | Endre `applicationUrl` i `Properties/launchSettings.json`, og `@host` i `.http`-filene. |
| `Send request` vises ikke over kallene i `.http`-filene | Filen må ha endelsen `.http`, og Visual Studio må være en nyere versjon. |
| Dataene ser rare ut etter eksperimentering | Gjenopprett `customers.json` og slett `activities.json`. |
| Et kall i `Assignment.http` gir 404 for en id som skulle finnes | Variablene øverst i filen peker på feil id. Sett dem til id-en i svaret fra opprett-kallet. |