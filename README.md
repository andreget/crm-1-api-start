# Crm

En liten webapp for kundebehandling (CRM). Dette er startpunktet for API-oppgavene i modul 1. Koden er med vilje enkel, og du skal bygge den ut og forbedre den steg for steg i modulene.

## Hva den gjør

- **Kunder:** liste med søk og statusfilter, detaljside, opprett, endre og slett.
- **Aktiviteter:** registrer møter, samtaler, e-poster og oppgaver for en kunde, merk dem som ferdige, og se alle åpne aktiviteter i én oversikt. Forfalte aktiviteter er markert.
- **API (JSON):** to endepunkter som leverer kundedata til andre programmer. Du bygger ut resten selv i oppgavene.

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

**Visual Studio:** åpne `Crm.sln` (eller `Crm.csproj`) og trykk `F5`. Nettleseren åpner <http://localhost:5080>.

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

### Test med `Crm.http`

Filen `Crm.http` inneholder ferdige kall mot API-et. Åpne den i Visual Studio, start appen, og trykk **Send request** over et kall. Hvis appen kjører på en annen port enn 5080, endrer du `@host` øverst i filen. Når du lager nye endepunkter, legger du til nye kall i samme fil og tester dem der.

## Prosjektstruktur

```
Program.cs          Endepunkter og hjelpefunksjoner
Models/             Customer og Activity
Services/           CustomerService og ActivityService (lagrer data i JSON-filer)
Templates/          HTML-maler med {{placeholders}}
wwwroot/            CSS og JavaScript
customers.json      Eksempelkunder (appen endrer denne filen)
Crm.http            Kall for å teste API-et
```

## Data

- `customers.json` inneholder fem eksempelkunder. Appen skriver til denne filen når du oppretter, endrer eller sletter kunder, så den vises som endret i Git. Slik gjenoppretter du eksempeldataene:

```
git checkout customers.json
```

- `activities.json` opprettes første gang du legger til en aktivitet. Den følges ikke av Git. Slett filen for å fjerne alle aktiviteter.

## Feilsøking

| Problem | Hva du bør sjekke |
|---|---|
| Feilen `NETSDK1004` (assets file not found) | Kjør `dotnet restore` i mappen med `Crm.csproj`, og bygg på nytt. |
| Feil eller blank side rett etter oppstart | Appen er startet fra feil mappe. Start den fra mappen med `Crm.csproj`. |
| Port 5080 er opptatt | Endre `applicationUrl` i `Properties/launchSettings.json`, og `@host` i `Crm.http`. |
| `Send request` vises ikke over kallene i `Crm.http` | Filen må ha endelsen `.http`, og Visual Studio må være en nyere versjon. |
| Dataene ser rare ut etter eksperimentering | Gjenopprett `customers.json` og slett `activities.json`. |