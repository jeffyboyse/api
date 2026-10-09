# api


API-dokumentation: Pages API
Dokumentation för sidhanteringsgränssnittet (pages.php)

Bas-URL	/pages.php
Content-Type	application/json; charset=utf-8
Beskrivning	API för att skapa, läsa, uppdatera och ta bort sidor samt hantera flerspråkighet (sv, en) och gruppering via group_id.

• id (int): Unikt ID för sidan (auto-increment).
• group_id (int): ID som kopplar samman översättningar av samma sida.
• title (string): Sidans rubrik (obligatorisk vid skapande).
• lang (string): Språkkod. Endast 'sv' eller 'en' är tillåtna.
• content (string): Innehåll i HTML-format.
• status (string): Publiceringsstatus ('draft' eller 'published').
1. GET /pages.php
Hämtar en enskild sida (via ID) eller en lista över sidor utifrån sökfilter.
Query-parametrar
• id (int, valfri): Unikt ID för sidan. Om angivet returneras endast den specifika sidan.
• lang (string, valfri): Filtrera på språkkod ('sv' eller 'en').
• status (string, valfri): Filtrera på status ('draft' eller 'published').
• q (string, valfri): Söktext som matchar i antingen titel eller innehåll.
Svar vid hämta specifik sida (?id=X)
Status: 200 OK
{
  "id": 1,
  "group_id": 1,
  "title": "Om oss",
  "lang": "sv",
  "content": "<h1>Om oss</h1><p>Vi är en liten webbplats.</p>",
  "status": "published"
}

Status: 404 Not Found (om sidan inte finns)
{
  "fel": "Sidan finns inte"
}

Svar vid hämta lista
Status: 200 OK (Returnerar matris sorterad fallande på ID)
[
  {
    "id": 1,
    "group_id": 1,
    "title": "Om oss",
    "lang": "sv",
    "status": "published"
  },
  {
    "id": 2,
    "group_id": 1,
    "title": "About us",
    "lang": "en",
    "status": "published"
  }
]

2. POST /pages.php
Skapar en ny sida i databasen.
Request Body (JSON)
• title (string, obligatorisk): Sidans rubrik.
• group_id (int, valfri): Koppla till en befintlig översättningsgrupp.
• lang (string, valfri): Språkkod (standard: "sv"). Endast 'sv' och 'en' tillåts.
• content (string, valfri): Innehåll i HTML-format (standard: "").
• status (string, valfri): Publiceringsstatus (standard: "draft").
Svar
Status: 201 Created
{
  "id": 3,
  "group_id": 2,
  "title": "Kontakt",
  "lang": "sv",
  "content": "<h1>Kontakt</h1>",
  "status": "draft"
}

Status: 400 Bad Request (om title saknas eller ogiltigt språk anges)
{
  "fel": "Endast språken 'sv' och 'en' är tillåtna."
}

3. PUT /pages.php?id={id}
Uppdaterar en befintlig sida.
Query-parametrar
• id (int, obligatorisk): ID på sidan som ska uppdateras.
Request Body (JSON)
Skicka endast de fält som ska ändras (group_id, title, lang, content, status).
{
  "title": "Ny titel",
  "lang": "en",
  "status": "published"
}

Svar
Status: 200 OK (Returnerar det uppdaterade objektet)
Status: 400 Bad Request (Om id saknas, ogiltigt språk anges eller inget finns att uppdatera)
Status: 404 Not Found (Om sidan inte finns)
4. DELETE /pages.php?id={id}
Tar bort en sida permanent från databasen.
Query-parametrar
• id (int, obligatorisk): ID på sidan som ska raderas.
Svar
Status: 200 OK
{
  "meddelande": "Sidan togs bort"
}

Status: 400 Bad Request (om id saknas)
Status: 404 Not Found (om sidan inte hittas)
