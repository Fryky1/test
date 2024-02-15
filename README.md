# TEAM-EIFFEL - FTE-Bezetting

## Doel van dit project

Dit project geeft inzicht in {invullen}

## Databronnen van dit project

### Bestand: Afas data.xlsx

**Bron: EIFFEL - Contract**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: EIFFEL - Functie**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: EIFFEL - Rooster employee**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: EIFFEL - Voorcalculatie**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

---

### Bestand: Projects data.xlsx

**Bron: EP - Contract & Functie**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: EP - Rooster Employee**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: EP - Voorcalculatie**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

---

### Bestand: Budget.xlsx

**Bron: Eiffel - Budget**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

---

### Bestand: General data.xlsx

**Bron: Date last refresh**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: DateDimension**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

**Bron: Row Level Security**
- Beschrijving: {invullen}
- Oorsprong / Eigenaar: {invullen}

Kolommen ⬇️
- kolom 1: {invullen}

---

# Power Query (back-end)

## Map: AFAS ➡️ Hier zijn alle queries te vinden die gebaseerd zijn op de databron Afas data.xlsx.
### Query: EIFFEL - Contract

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
Workspaces = Source{[Id="Workspaces"]}[Data],
#"e988236d-03e5-4b28-b839-40716c037784" = Workspaces{[workspaceId="e988236d-03e5-4b28-b839-40716c037784"]}[Data],
#"14a24ff4-d26e-414e-ba87-29dc4c2a224e" = #"e988236d-03e5-4b28-b839-40716c037784"{[dataflowId="14a24ff4-d26e-414e-ba87-29dc4c2a224e"]}[Data],
#"EIFFEL - Contract_" = Source{[Item="EIFFEL - Contract",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"EIFFEL - Contract_", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Custom1
Code:
```
Custom1 = Table.TransformColumnTypes(#"Promoted Headers",{{"Medewerker", Int64.Type}, {"Naam", type text}, {"Begindatum_contract", type datetime}, {"Einddatum_contract", type datetime}, {"Dienstbetrekking", type text}, {"Type_contract", type text}, {"Werkgevernr",   Int64.Type}, {"Werkgever", type text}, {"Kostendrager", type text}, {"KD_oms", type text}, {"Medw_Datum_in_dienst", type date}, {"Medw_Datum_uit_dienst", type text}}),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(Custom1,{{"Begindatum_contract", type date}, {"Einddatum_contract", type date}}),
```

#### Toegepaste stap: ➕ Added Matching ID
Code:
```
// Toevoegen van een Matching ID kolom,
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"➕ Added Matching ID" = Table.AddColumn(#"Changed Type", "Matching ID", each [Medewerker], type text),
```
Beschrijving inhoudelijk ➡️ Voor de bron AFAS is de "Matching ID" kolom het personeelsnummer.

#### Toegepaste stap: 🔍 Filtered Einddatum_contract
Code:
```
// Alle einddatum eruit filteren die langer dan 12 maanden geleden zijn beindigd.
#"🔍 Filtered Einddatum_contract" = Table.SelectRows(#"➕ Added Matching ID", each Date.IsInPreviousNMonths([Einddatum_contract], 12) or Date.IsInCurrentMonth([Einddatum_contract]) or [Einddatum_contract] > Date.From(DateTime.LocalNow()) or [Einddatum_contract] = null)
```
Beschrijving inhoudelijk ➡️ {invullen}

Een query in Power Query heeft altijd ook een einde:
```
in
#"🔍 Filtered Einddatum_contract"
```

---

### Query: EIFFEL - Functie

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"EIFFEL - Functie_Sheet" = Source{[Item="EIFFEL - Functie",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers1
Code:
```
#"Promoted Headers1" = Table.PromoteHeaders(#"EIFFEL - Functie_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers1",{{"Medewerker", Int64.Type}, {"Naam", type text}, {"Begin", type datetime}, {"Eind", type datetime}, {"Functie", type text}, {"Kostenpl", type text}, {"Kostenplaats", type text}, {"Kostendrager", type text}, {"GUID", type text}, {"Volgnummer_Organisatorische_eenheid_functie", Int64.Type}, {"Datum_in_dienst", type datetime}, {"Datum_uit_dienst", type text}, {"Definitief_indienst", type logical}, {"Business_Line", type text}, {"skip", Int64.Type}, {"take", Int64.Type}}),
```

#### Toegepaste stap: 🎨 Changed Type
Code:
```
#"🎨 Changed Type" = Table.TransformColumnTypes(#"Changed Type",{{"Eind", type datetime}, {"Begin", type datetime}, {"Datum_in_dienst", type datetime}}),
```

#### Toegepaste stap: 🎨 Changed Type1
Code:
```
#"🎨 Changed Type1" = Table.TransformColumnTypes(#"🎨 Changed Type",{{"Eind", type date}, {"Begin", type date}, {"Datum_in_dienst", type date}}),
```

#### Toegepaste stap: ➕ Added Matching ID
Code:
```
// Toevoegen van een Matching ID kolom,
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"➕ Added Matching ID" = Table.AddColumn(#"🎨 Changed Type1", "Matching ID", each [Medewerker], type text),
```
Beschrijving inhoudelijk ➡️ Voor de bron AFAS is de "Matching ID" kolom het personeelsnummer.

#### Toegepaste stap: 🚫 Removed columns
Code:
```
// verwijderen van overbodige kolommen
#"🚫 Removed columns" = Table.SelectColumns(#"➕ Added Matching ID",{"Medewerker", "Naam", "Begin", "Eind", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager", "Matching ID"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔍 Filtered Eind
Code:
```
// eruit filteren van functies die in meer dan 12 maanden geleden zijn geeindigd
#"🔍 Filtered Eind" = Table.SelectRows(#"🚫 Removed columns", each Date.IsInPreviousNMonths([Eind], 12) or Date.IsInCurrentMonth([Eind]) or [Eind] > Date.From(DateTime.LocalNow()) or [Eind] = null),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🖍️ Renamed Columns
Code:
```
// kolom Begin herbenoemen naar "Begindatum_functie"
// kolom Eind herbenoemen naar "Einddatum_functie"
#"🖍️ Renamed Columns" = Table.RenameColumns(#"🔍 Filtered Eind",{{"Begin", "Begindatum_functie"}, {"Eind", "Einddatum_functie"}})
```
Beschrijving inhoudelijk ➡️ {invullen}

Een query in Power Query heeft altijd ook een einde:
```
in
#"🖍️ Renamed Columns"
```

---

### Query: EIFFEL - Rooster employee

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"EIFFEL - Rooster employee_Sheet" = Source{[Item="EIFFEL - Rooster employee",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"EIFFEL - Rooster employee_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Medewerker", Int64.Type}, {"Begindatum_rooster", type datetime}, {"Einddatum_rooster", type datetime}, {"Uren_per_week", Int64.Type}, {"Parttime", Int64.Type}, {"Aantal_FTE", Int64.Type}, {"Uren_Zondag", Int64.Type}, {"Uren_Maandag", Int64.Type}, {"Uren_Dinsdag", Int64.Type}, {"Uren_Woensdag", Int64.Type}, {"Uren_Donderdag", Int64.Type}, {"Uren_Vrijdag", Int64.Type}, {"Uren_Zaterdag", Int64.Type}, {"Wisselend_arbeidspatroon", type logical}}),
```

#### Toegepaste stap: 🎨 Changed Type
Code:
```
#"🎨 Changed Type" = Table.TransformColumnTypes(#"Changed Type",{{"Begindatum_rooster", type date}, {"Einddatum_rooster", type date}})
```

Een query in Power Query heeft altijd ook een einde:
```
in
#"🎨 Changed Type"
```
---

### Query: EIFFEL - Voorcalculatie

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"EIFFEL - Voorcalculatie_Sheet" = Source{[Item="EIFFEL - Voorcalculatie",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"EIFFEL - Voorcalculatie_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Project", type text}, {"Omschrijving", type text}, {"AccounNaam", type text}, {"Medewerker", Int64.Type}, {"Medewerkernaam", type text}, {"Urensoort", Int64.Type}, {"Uursoort oms", type text}, {"Begindatum", type date}, {"Einddatum", type date}, {"Aantal_eenheden", Int64.Type}, {"Projectgroep", type text}, {"Omschrijving_3", type text}})
```

Een query in Power Query heeft altijd ook een einde:
```
in
#"Changed Type"
```

---

## Map: Projects ➡️ Hier zijn alle queries te vinden die gebaseerd zijn op de databron Projects data.xlsx.
### Query: EP - Contract & Functie

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"EP - Contract & Functie_Sheet" = Source{[Item="EP - Contract & Functie",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"EP - Contract & Functie_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Medewerker", Int64.Type}, {"Naam", type text}, {"Begindatum_contract", type date}, {"Einddatum_contract_org", type date}, {"Dienstbetrekking", type text}, {"Type_contract", type text}, {"Werkgevernr", Int64.Type}, {"Werkgever", type text}, {"Begindatum_functie", type date}, {"Einddatum_functie_org", type date}, {"Functie", type text}, {"Kostenpl", type text}, {"Kostenplaats", type text}, {"Kostendrager", type text}, {"Einddatum_contract", type date}, {"Einddatum_functie", type date}, {"Huidig Contract?", type text}, {"Functie geldig tijdens Contract", type text}, {"hrm_employeeid", type text}, {"Medw_Datum_in_dienst", type date}, {"Medw_Datum_uit_dienst", type date}}),
```

#### Toegepaste stap: 🖍️ Renamed Columns
Code:
```
// Herbenoemen naar Matching ID kolom, 
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"🖍️ Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"hrm_employeeid", "Matching ID"}}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔍 Filtered Einddatum_contract
Code:
```
// Alle einddatum eruit filteren die langer dan 12 maanden geleden zijn beindigd.
#"🔍 Filtered Einddatum_contract" = Table.SelectRows(#"🖍️ Renamed Columns", each Date.IsInPreviousNMonths([Einddatum_contract], 12) or Date.IsInCurrentMonth([Einddatum_contract]) or [Einddatum_contract] > Date.From(DateTime.LocalNow()) or [Einddatum_contract] = null),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔍 Filtered Rows 
Code:
```
// Eruit filteren van regels die geen Begindatum contract en Begindatum functie hebben. 
#"🔍 Filtered Rows " = Table.SelectRows(#"🔍 Filtered Einddatum_contract", each ([Begindatum_contract] <> null) and ([Begindatum_functie] <> null))
```
Beschrijving inhoudelijk ➡️ {invullen}

Een query in Power Query heeft altijd ook een einde:
```
in
#"🔍 Filtered Rows "
```

---

### Query: EP - Rooster Employee

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"EP - Rooster Employee_Sheet" = Source{[Item="EP - Rooster Employee",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"EP - Rooster Employee_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Medewerker", Int64.Type}, {"Begindatum_rooster", type date}, {"Einddatum_rooster", type date}, {"Uren_per_week", Int64.Type}, {"Uren_contract_pd", type number}, {"hrm_employee", type text}}),
```

#### Toegepaste stap: 🖍️ Renamed Columns
Code:
```
// Herbenoemen naar Matching ID kolom, 
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"🖍️ Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"hrm_employee", "Matching ID"}})
```
Beschrijving inhoudelijk ➡️ Voor de bron Projects is de "Matching ID" kolom het personeelsnummer.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🖍️ Renamed Columns"
```

---

### Query: EP - Voorcalculatie

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"EP - Voorcalculatie_Sheet" = Source{[Item="EP - Voorcalculatie",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"EP - Voorcalculatie_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Project", type text}, {"Omschrijving", type text}, {"AccounNaam", type text}, {"Medewerker", Int64.Type}, {"Medewerkernaam", type text}, {"Uursoort oms", type text}, {"Begindatum", type date}, {"Einddatum", type date}, {"Aantal_eenheden", Int64.Type}, {"Projectgroep", type text}, {"Omschrijving_3", type text}, {"EP-Medewerker_id", type text}}),
```

#### Toegepaste stap: 🖍️ Renamed Columns
Code:
```
// Herbenoemen naar Matching ID kolom, 
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"🖍️ Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"EP-Medewerker_id", "Matching ID"}})
```
Beschrijving inhoudelijk ➡️ Voor de bron Projects is de "Matching ID" kolom het personeelsnummer.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🖍️ Renamed Columns"
```

---

## Map: Budget ➡️ Hier zijn alle queries te vinden die gebaseerd zijn op de databron Budget.xlsx.
### Query: Eiffel - Budget

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"Eiffel - Budget_Sheet" = Source{[Item="Eiffel - Budget",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"Eiffel - Budget_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Hoofdrubriek", type text}, {"Categorie", type text}, {"Unit", type text}, {"Vertical", type text}, {"KPI", type text}, {"Maand", Int64.Type}, {"Waarde", Int64.Type}, {"Bron", type text}, {"Jaar", Int64.Type}, {"Kostenplaatsnummer", type text}}),
```

#### Toegepaste stap: 🎨 Changed Type
Code:
```
#"🎨 Changed Type" = Table.TransformColumnTypes(#"Changed Type",{{"Hoofdrubriek", type text}, {"Categorie", type text}, {"Unit", type text}, {"Vertical", type text}, {"KPI", type text}, {"Maand", Int64.Type}, {"Waarde", type number}, {"Bron", type text}, {"Jaar", Int64.Type}, {"Kostenplaatsnummer", type text}}),
```

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden de relevante meetwaardes aangemaakt vanuit het budget.

#### Toegepaste stap: 🔍 Filtered Categorie & KPI
Code:
```
// Voor dit bezettingsoverzicht willen we alleen de waarde "D" hebben uit de kolom Categorie
// Vanuit de kolom KPI willen we alleen de volgende waardes hebben: "FTE", "Facturabele uren", "Feestdag uren", "Overige uren","Contracturen" & "Verlofuren" met de andere waardes gaan we geen berekening maken
#"🔍 Filtered Categorie & KPI" = Table.SelectRows(#"------------", each ([Categorie] = "D") and ([KPI] = "Facturabele uren" or [KPI] = "Feestdag uren" or [KPI] = "FTE" or [KPI] = "Overige uren" or [KPI] = "Contracturen" or [KPI] = "Verlofuren")),
```
Beschrijving inhoudelijk ➡️ Met de andere waardes in de kolom "Categorie" en kolom "KPI" zullen er geen berekeningen worden gedaan.

#### Toegepaste stap: ⨊ Grouped Rows
Code:
```
// Controle of er type KPI per Maand-Jaar maar 1 regel is per Unit. Anders worden die via deze stap samengevoegd
#"⨊ Grouped Rows" = Table.Group(#"🔍 Filtered Categorie & KPI", {"Kostenplaatsnummer", "Jaar", "Maand", "KPI"}, {{"Waarde", each List.Sum([Waarde]), type nullable number}}),
```
Beschrijving inhoudelijk ➡️ Hier vindt een controle plaats of type KPI per Maand-Jaar maar 1 regel is per Unit.

#### Toegepaste stap: ↗️ Pivoted KPI & Waarde
Code:
```
// Per soort KPI wil ik een kolom hebben om de vervolg berekeningen mogelijk te maken
#"↗️ Pivoted KPI & Waarde" = Table.Pivot(#"⨊ Grouped Rows", List.Distinct(#"⨊ Grouped Rows"[KPI]), "KPI", "Waarde", List.Sum),
```
Beschrijving inhoudelijk ➡️ De pivot is nodig om de berekeningen die hieronder volgen mogelijk te maken.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt een maand-jaar kolom toegevoegd om straks in het model een relatie mee te kunnen leggen.

#### Toegepaste stap: ➕ Added Datum
Code:
```
// om straks in het model een relatie te kunnen leggen
#"➕ Added Datum" = Table.AddColumn(#"--------------------", "Datum", each Date.From(#date([Jaar],[Maand],1)), type date),
```
Beschrijving inhoudelijk ➡️ De kolom "Datum" is nodig zodat er een relatie hiermee in het datamodel aangelegd kan worden.

#### Toegepaste stap: 🚫 Removed Columns
Code:
```
// Verwijderen van overbodige kolommen
#"🚫 Removed Columns" = Table.SelectColumns(#"➕ Added Datum",{"Kostenplaatsnummer", "Overige uren", "Verlofuren", "Facturabele uren", "Contracturen", "FTE", "Feestdag uren", "Datum"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔍 Filtered Datum
Code:
```
// Het Budget van 2023 word eruit gefiltert omdat dit geen inhoudelijk goede budget is. 
#"🔍 Filtered Datum" = Table.SelectRows(#"🚫 Removed Columns", each [Datum] > #date(2023, 12, 31)),
```
Beschrijving inhoudelijk ➡️ Het budget van 2023 wordt hier weggefilterd omdat het budget van 2023 geen inhoudelijk goed budget is.

#### Toegepaste stap: Custom1
Code:
```
Custom1 = Table.SelectRows(#"🔍 Filtered Datum", each ([Kostenplaatsnummer] <> "" and [Kostenplaatsnummer] <> "KP0049" and [Kostenplaatsnummer] <> "KP0081" and [Kostenplaatsnummer] <> "KP0085" and [Kostenplaatsnummer] <> "KP0100" ) ),
```

#### Toegepaste stap: 🖍️ Replaced Value in "FTE"
Code:
```
// Veranderen van null waardes naar 0 waardes zodat de DAX beter loopt
#"🖍️ Replaced Value in ""FTE""" = Table.ReplaceValue(Custom1,null,0,Replacer.ReplaceValue,{"FTE"}),
```
Beschrijving inhoudelijk ➡️ De waarden "null" worden hier door "0" vervangen in de kolom "FTE" zodat de DAX die gebaseerd is op deze kolom soepel kan functioneren.

#### Toegepaste stap: 🖍️ Replaced Value Kostenplaatsnummer
Code:
```
// eKP310 vervangen door eKP297.
// In deze budget sheet stond nog het oude kostenplaatsnummer
#"🖍️ Replaced Value Kostenplaatsnummer" = Table.ReplaceValue(#"🖍️ Replaced Value in ""FTE""","eKP310","eKP297",Replacer.ReplaceText,{"Kostenplaatsnummer"})
```
Beschrijving inhoudelijk ➡️ Hier wordt de waarde eKP310 vervangen door eKP297 in de kolom "Kostenplaatsnummer" omdat het andere kostenplaatsnummer verouderd is maar nog wel erin stond.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🖍️ Replaced Value Kostenplaatsnummer"
```

---

## Map: Afas & Projects samen ➡️ Hier worden de Rooster employee data vanuit AFAS en Projects toegevoegd tot 1 tabel, en de Voorcalculatie data vanuit AFAS en Projects worden ook toegevoegd tot 1 tabel.
### Query: Rooster employee samen

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = #"EIFFEL - Rooster employee",
```

#### Toegepaste stap: ➕ Added Matching ID
Code:
```
// Toevoegen van een Matching ID kolom,
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"➕ Added Matching ID" = Table.AddColumn(Source, "Matching ID", each [Medewerker], type text),
```
Beschrijving inhoudelijk ➡️ Voor de bron AFAS is de "Matching ID" kolom het personeelsnummer.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt de gemiddelde FTE inzet per dag berekend.

#### Toegepaste stap: 🚫 Removed Columns
Code:
```
#"🚫 Removed Columns" = Table.SelectColumns(#"--------",{"Medewerker", "Begindatum_rooster", "Einddatum_rooster", "Uren_per_week", "Matching ID"}),
```

#### Toegepaste stap: ➕ Added Uren_contract_pd
Code:
```
// Het gemiddelde aantal uren per dag zijn de weekuren gedeeld door het aantal doordeweeksedagen (geen rekening houdend met feestdagen)
#"➕ Added Uren_contract_pd" = Table.AddColumn(#"🚫 Removed Columns", "Uren_contract_pd", each [Uren_per_week]/5, type number),
```
Beschrijving inhoudelijk ➡️ De kolom "Uren_per_week" wordt hier gedeeld door 5 om het aantal uren per week te converteren naar het aantal uren per dag (zonder rekening te houden met feestdagen).

#### Toegepaste stap: ⏬ Appended Query
Code:
```
// Onder elkaar zetten van de AFAS data en de Projects data
#"⏬ Appended Query" = Table.Combine({#"➕ Added Uren_contract_pd", #"EP - Rooster Employee"}),
```
Beschrijving inhoudelijk ➡️ Hier worden de queries "EIFFEL - Rooster employee" en "EP - Rooster Employee" onder elkaar toegevoegd zodat zowel de employee data van AFAS als die van Projects in een tabel samen zitten.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden regels zonder begindatum en regels met een einddatum die te ver geleden is eruit gefilterd.

#### Toegepaste stap: 🔍 Filtered Einddatum_rooster
Code:
```
// Eruit filteren van projecten die al langer dan 12 maanden geleden zijn afgelopen
#"🔍 Filtered Einddatum_rooster" = Table.SelectRows(#"-----------", each Date.IsInPreviousNMonths([Einddatum_rooster], 12) or Date.IsInCurrentMonth([Einddatum_rooster]) or [Einddatum_rooster] > Date.From(DateTime.LocalNow()) or [Einddatum_rooster] = null),
```
Beschrijving inhoudelijk ➡️ Projecten die al meer dan 12 maanden geleden afgelopen zijn, zijn hier niet interessant.

#### Toegepaste stap: 🔍 Filtered Begindatum_rooster
Code:
```
// Eruit filteren van Roosters die geen begindatum hebben. 
// Dit is een error preventie
#"🔍 Filtered Begindatum_rooster" = Table.SelectRows(#"🔍 Filtered Einddatum_rooster", each [Begindatum_rooster] <> null)
```
Beschrijving inhoudelijk ➡️ Roosters die geen begindatum hebben worden hier eruit gefilterd als error preventie.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🔍 Filtered Begindatum_rooster"
```

---

### Query: Voorcalculatie samen

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = #"EIFFEL - Voorcalculatie",
```

#### Toegepaste stap: ➕ Added Matching ID
Code:
```
// Toevoegen van een Matching ID kolom,
// voor AFAS is dit het personeelsnummer en voor Projects het hrm-employeeid
// 
#"➕ Added Matching ID" = Table.AddColumn(Source, "Matching ID", each [Medewerker], type text),
```
Beschrijving inhoudelijk ➡️ Voor de bron AFAS is de "Matching ID" kolom het personeelsnummer.

#### Toegepaste stap: ⏬ Appended Query
Code:
```
#"⏬ Appended Query" = Table.Combine({#"➕ Added Matching ID", #"EP - Voorcalculatie"}),
```
Beschrijving inhoudelijk ➡️ Hier worden de queries "EIFFEL - Voorcalculatie" en "EP - Voorcalculatie" onder elkaar toegevoegd zodat zowel de voorcalculatie data van AFAS als die van Projects in een tabel samen zitten.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden de irrelevante projecten eruit gefilterd.

#### Toegepaste stap: 🔍 Filtered Begindatum
Code:
```
// Alle null waardes worden eruit gehaald
#"🔍 Filtered Begindatum" = Table.SelectRows(#"---------", each [Begindatum] <> null),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔍 Filtered Einddatum
Code:
```
// Eruit filteren van projecten die al langer dan 12 maanden geleden zijn afgelopen
#"🔍 Filtered Einddatum" = Table.SelectRows(#"🔍 Filtered Begindatum", each Date.IsInPreviousNMonths([Einddatum], 12) or Date.IsInCurrentMonth([Einddatum]) or [Einddatum] > Date.From(DateTime.LocalNow()) or [Einddatum] = null),
```
Beschrijving inhoudelijk ➡️ Projecten die al meer dan 12 maanden geleden zijn afgelopen zijn hier niet interessant.

#### Toegepaste stap: 🔍 Filtered Projectgroep
Code:
```
// De volgende soort projecten moeten niet meegenomen worden:
// "Bank", "DECL" & "NDECL"
#"🔍 Filtered Projectgroep" = Table.SelectRows(#"🔍 Filtered Einddatum", each ([Projectgroep] <> "BANK" and [Projectgroep] <> "DECL" and [Projectgroep] <> "NDECL"))
```
Beschrijving inhoudelijk ➡️ De specifieke projectgroepen die niet interessant zijn worden hier niet meegenomen.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🔍 Filtered Projectgroep"
```

---

## Map: General ➡️ Hier zijn alle queries te vinden die gebaseerd zijn op de databron General data.xlsx.
### Query: Date last refresh

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"Date last refresh_Sheet" = Source{[Item="Date last refresh",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"Date last refresh_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Date last refresh", type datetime}})
``` 

Een query in Power Query heeft altijd ook een einde:
```
in
#"Changed Type"
```

---

### Query: DateDimension

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
DateDimension_Sheet = Source{[Item="DateDimension",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(DateDimension_Sheet, [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Datum", type date}, {"Jaar", Int64.Type}, {"Halfjaar", type text}, {"Jaar & Halfjaar", type text}, {"Kwartaal", type text}, {"Jaar & Kwartaal", type text}, {"Maand", Int64.Type}, {"Maandnr", Int64.Type}, {"Jaar & Maand", Int64.Type}, {"Maandnaam", type text}, {"Maand (afk)", type text}, {"Maand & Jaar", type date}, {"Maand (afk) & Jaar", type date}, {"Dag van de maand", Int64.Type}, {"Dag van jaar", Int64.Type}, {"Weeknr", Int64.Type}, {"Dag van week", Int64.Type}, {"Dagnaam", type text}, {"Dagnaam (afk)", type text}, {"ISO Week", Int64.Type}, {"ISO Week Index", Int64.Type}, {"Fiscale maand", Int64.Type}, {"Fiskaaljaar & Maand", Int64.Type}, {"Jaar terug", Int64.Type}, {"Kwartaal terug", Int64.Type}, {"Maand terug", Int64.Type}, {"Is 13 Maand terug", type logical}, {"Dag terug", Int64.Type}, {"Week terug", Int64.Type}, {"Feestdagnaam", type text}, {"Is Feestdag", type logical}, {"Is weekend", type logical}, {"Is Werkdag", type logical}, {"Laatste werkdag", type date}, {"Is laatste werkdag", type logical}, {"Is actuele betaalmaand", type logical}, {"ISO jaar", Int64.Type}, {"ISO Week nr.", Int64.Type}, {"Start of week", type date}}),
```

#### Toegepaste stap: 🚫 Removed columns
Code:
```
// alleen behouden van relevante kolommen
#"🚫 Removed columns" = Table.SelectColumns(#"Changed Type",{"Datum", "Jaar", "Maandnr", "Maandnaam", "Maand (afk)", "Weeknr", "Dag van week", "Is Werkdag", "Start of week"}),
```
Beschrijving inhoudelijk ➡️ {} 

#### Toegepaste stap: 🔍 Filtered datum
Code:
```
// Filteren van datum op relevante datums, dus in de vorige 12 maanden of minder dan 3 maanden in de toekomst
#"🔍 Filtered datum" = Table.SelectRows(#"🚫 Removed columns", each   [Datum] >= Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) and [Datum] <= Date.From(Date.AddMonths(DateTime.LocalNow(), 3))),
```
Beschrijving inhoudelijk ➡️ De relevante datums hier zijn alle datums die binnen de vorige 12 maanden vallen en binnen de komende 3 maanden.

#### Toegepaste stap: ➕ Added Meenemen in lange termijn visual
Code:
```
// Indien een datum in de laatste 2 maanden ligt of in de volgende 3 maanden. dan moet die in die visual komen
    #"➕ Added Meenemen in lange termijn visual" = Table.AddColumn(#"🔍 Filtered datum", "Meenemen in lange termijn visual", each if  [Datum] > Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) and [Datum] < Date.From(Date.AddMonths(DateTime.LocalNow(), 3))
then "Ja"
else "Nee", type text),
```
Beschrijving inhoudelijk ➡️ Hier worden er categorieën gemaakt op basis van de datums die binnen de afgelopen 2 maanden en de komende 3 maanden liggen. Als dit het geval is dan wordt de categorie "Ja" en anders "Nee" om te bepalen of ze wel of niet in een visual voor/over de lange termijn meegenomen moeten worden.

#### Toegepaste stap: 🎨 Changed Type
Code:
```
#"🎨 Changed Type" = Table.TransformColumnTypes(#"➕ Added Meenemen in lange termijn visual",{{"Maandnr", Int64.Type}, {"Jaar", Int64.Type}}),
```

#### Toegepaste stap: ➕ Added Datum in slicer?
Code:
```
// Toevoegen van een indiactor die aangeeft of de datum vandaag is, een maandag in de toekomst of de eerste werkdag van de maand
#"➕ Added Datum in slicer?" = Table.AddColumn(#"🎨 Changed Type", "Datum in slicer?", each /* eerst kijken of de dag vandaag is*/
if [Datum] = Date.From(DateTime.LocalNow()) then "Ja" 
/* anders kijken of de dag op een maandag na vandaag ligt */
else if [Datum] > Date.From(DateTime.LocalNow()) and [Dag van week] =1 then "Ja" 
/*anders kijken naar de eerste werkdag van de maand in de toekomst*/
else if 
([Datum] = Date.StartOfMonth([Datum]) and [Dag van week] <6 and [Datum] > Date.From(DateTime.LocalNow()) ) then "Ja" 
else if ([Datum] = Date.AddDays(Date.StartOfMonth([Datum]),1) and [Dag van week] =1 and [Datum] > Date.From(DateTime.LocalNow())) 
then "Ja" 
else if ([Datum] = Date.AddDays(Date.StartOfMonth([Datum]),2) and [Dag van week] =1 and [Datum] > Date.From(DateTime.LocalNow()))
 then "Ja" 

else "Nee", type text)
```
Beschrijving inhoudelijk ➡️ Hier wordt een indicator toegevoegd die aangeeft of de datum vandaag is, een maandag in de toekomst of de eerste werkdag van de maand.

Een query in Power Query heeft altijd ook een einde:
```
in
#"➕ Added Datum in slicer?"
```

---

### Query: Row Level Secority

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents(""), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"Row Level Security_Sheet" = Source{[Item="Row Level Security",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"Row Level Security_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Aanmeldnaam", type text}, {"Functie", type text}, {"Aanmeldmail", type text}, {"Vertical code", Int64.Type}, {"Vertical Naam", type text}, {"ObjectId", type text}, {"Security", type text}}),
```

#### Toegepaste stap: 🔀 Merged Queries DIM - Kostenplaatsen
Code:
```
#"🔀 Merged Queries DIM - Kostenplaatsen" = Table.NestedJoin(#"Changed Type", {"Vertical code"}, #"DIM - Structuur Team Eiffel", {"Vertical code"}, "DIM - Kostenplaatsen", JoinKind.LeftOuter),
```

#### Toegepaste stap: ↪️ Expanded DIM - Kostenplaatsen
Code:
```
// Hiermee zet je de toegang op vertical om naar een granualiteit dieper: Kostenplaatsniveau
#"↪️ Expanded DIM - Kostenplaatsen" = Table.ExpandTableColumn(#"🔀 Merged Queries DIM - Kostenplaatsen", "DIM - Kostenplaatsen", {"Kostenplaats code"}, {"Kostenplaats code"})
```
Beschrijving inhoudelijk ➡️ De kolom "Kostenplaats code" wordt hier toegevoegd vanuit de query "DIM - Structuur Team Eiffel" en hiermee wordt de toegang op vertical omgezet naar een granulariteit dieper: Kostenplaatsniveau.

Een query in Power Query heeft altijd ook een einde:
```
in
#"↪️ Expanded DIM - Kostenplaatsen"
```

---

## Map: Bewerkingen ➡️ Hier wordt de Rooster employee data verwerkt naar werkdagniveau. De Contract & Functie data wordt verwerkt naar werkdagniveau waarna de Rooster employee data eraan samengevoegd wordt. De Voorcalculatie data wordt verwerkt naar projecten per werkdag niveau.
### Query: Rooster employee bewerkt

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = #"Rooster employee samen",
```

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt eerst bepaald wat de begin en einddatum moeten worden waarvoor datums aangemaakt moeten worden en daarna wordt er een lijst gemaakt met de tussenliggende datums.

#### Toegepaste stap: ➕ Added Begindatum berekening
Code:
```
// Bepalen wat de begindatum moet worden van deze berekening. 
// Waarbij rekening gheouden wordt met de relatieve tijdspanne waarin we geintresseerd zijn en de combinatie tussen contract en functie
#"➕ Added Begindatum berekening" = Table.AddColumn(#"----------", "Begindatum berekening", each if [Begindatum_rooster] <= Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
Date.From(Date.AddMonths(DateTime.LocalNow(), -12))

else if [Begindatum_rooster] > Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
[Begindatum_rooster] 

else "nnb", type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt bepaald wat de begindatum moet worden van deze berekening, waarbij rekening gehouden wordt met de relatieve tijdspanne die relevant is en de combinatie tussen contract en functie.

#### Toegepaste stap: ➕ Added Einddatum berekening
Code:
```
// Bepalen wat de einddatum is waarvoor straks een lijst met datums aangemaakt gaat worden. 
// Indien er geen einddatum is dan wordt de datum 3 maanden van nu gebruikt
#"➕ Added Einddatum berekening" = Table.AddColumn(#"➕ Added Begindatum berekening", "Einddatum berekening", each if [Einddatum_rooster] > Date.From(Date.AddMonths(DateTime.LocalNow(), 3)) or [Einddatum_rooster] = null then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))

else if [Einddatum_rooster] <= Date.From(Date.AddMonths(DateTime.LocalNow(), 3)) then 
[Einddatum_rooster]

else "nnb", type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt bepaald wat de einddatum is waarvoor straks een lijst met datums aangemaakt gaat worden. Indien er geen einddatum is dan wordt de datum 3 maanden van nu gebruikt.

#### Toegepaste stap: ➕ Added DateDifference
Code:
```
// Hiermee kan straks een lijst gemaakt worden per dag. uiteindelijk hebben dit nodig om goed te mergen met de contract en project uren
#"➕ Added DateDifference" = Table.AddColumn(#"➕ Added Einddatum berekening", "DateDifference", each if Duration.Days([Einddatum berekening] - [Begindatum berekening]) >= 0 
then Duration.Days([Einddatum berekening] - [Begindatum berekening])
else 0),
```
Beschrijving inhoudelijk ➡️ De kolom is hier toegevoegd want uiteindelijk is dit nodig om goed te kunnen mergen met de contract en project uren.

#### Toegepaste stap: ➕ Added Dates
Code:
```
// Lijst klaarzetten om in volgende stap uit te klappen. 
// 
// Je maakt hier een lijst van je tussenliggende dagen +1 zodat je ook de begin en eind datum meekrijgt
#"➕ Added Dates" = Table.AddColumn(#"➕ Added DateDifference", "Dates", each List.Dates ([Begindatum berekening] , [DateDifference]+1, #duration (1, 0, 0, 0) )),
```
Beschrijving inhoudelijk ➡️ Hier wordt een lijst klaargezet om in de volgende stap uit te klappen. Deze lijst bevat tussenliggende dagen +1 zodat er ook de begin en einddatum meegekregen worden.

#### Toegepaste stap: ↪️ Expanded Dates
Code:
```
// Met deze actie komt er een regel per dag die ligt tussen begin en einddatum van het dienstverband. dit is inclusief de begin en einddatum
#"↪️ Expanded Dates" = Table.ExpandListColumn(#"➕ Added Dates", "Dates"),
```
Beschrijving inhoudelijk ➡️ Met deze actie komt er een regel per dag die ligt tussen begin en einddatum van het dienstverband. Dit is inclusief de begin en einddatum.

#### Toegepaste stap: 🎨 Changed Type Dates
Code:
```
// Date type geven
#"🎨 Changed Type Dates" = Table.TransformColumnTypes(#"↪️ Expanded Dates",{{"Dates", type date}}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ // In onderstaande code block worden de weekenddagen eruit gehaald. Dit wordt bereikt door een merge met de DateDimension tabel aangezien die info al daar staat.

#### Toegepaste stap: 🔀 Merged met DateDimension
Code:
```
// Met de lijsten hebben we nu ook weekenddagen aangemaakt, die wil ik er weer uit hebben. Wat weekenddagen zijn hebben we al in de DateDimension zitten. Daarnaast hebben we start of Week nodig voor een verdere berekening in de merge PLanned Hours & Contract
#"🔀 Merged met DateDimension" = Table.NestedJoin(#"---------", {"Dates"}, DateDimension, {"Datum"}, "DateDimension", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ In de lijst zijn er ook weekenddagen aangemaakt, maar die moeten eruit. De weekenddagen kunnen al geïdentificeerd met de DateDimension. Daarnaast is start of Week nodig voor een verdere berekening in de merge tussen Planned Hours en Contract.

#### Toegepaste stap: ↪️ Expanded DateDimension
Code:
```
// Alleen Dag van de Week & Start of Week eruit halen
#"↪️ Expanded DateDimension" = Table.ExpandTableColumn(#"🔀 Merged met DateDimension", "DateDimension", {"Dag van week", "Datum"}, {"Dag van week", "Datum"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔍 Filtered Dag van Week
Code:
```
// Op kolom Dag van Week gefiltert zodat de weekend dagen (6&7) eruit gaan
#"🔍 Filtered Dag van Week" = Table.SelectRows(#"↪️ Expanded DateDimension", each ([Dag van week] <> 6 and [Dag van week] <> 7)),
```
Beschrijving inhoudelijk ➡️ Hier wordt op de kolom "Dag van Week" gefilterd zodat de weekend dagen (6&7) eruit gaan.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt een kolom aangemaakt die aangeeft hoeveel uren er per dag geschreven mogen worden voor dit project.

#### Toegepaste stap: 🚫 Removed Columns
Code:
```
// Verwijderen van overbodige kolommen
#"🚫 Removed Columns" = Table.SelectColumns(#"-----------",{"Medewerker", "Matching ID", "Uren_contract_pd", "Datum"}),
```
Beschrijving inhoudelijk ➡️ Alleen de kolommen "Medewerker", "Matching ID", "Uren_contract_pd", "Datum" zijn hier interessant.

#### Toegepaste stap: 🔄️ Replaced Value in Uren_contract_pd
Code:
```
// Als er een null waarde is, dan wordt hier omgezet in een 0 waarde.
// dit weer als een soort error preventie
#"🔄️ Replaced Value in Uren_contract_pd" = Table.ReplaceValue(#"🚫 Removed Columns",null,0,Replacer.ReplaceValue,{"Uren_contract_pd"})
```
Beschrijving inhoudelijk ➡️ De waarde "null" wordt vervangen door "0" in de kolom "Uren_contract_pd" als een soort error preventie.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🔄️ Replaced Value in Uren_contract_pd"
```

---

### Query: Contract & Functie

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
// Match vindt plaatst op Matching ID
Source = Table.NestedJoin(#"EIFFEL - Contract", {"Matching ID"}, #"EIFFEL - Functie", {"Matching ID"}, "Eiffel - Employee", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ Met deze merge tussen Contract en Functie kan relevante data over Functie toegevoegd worden aan de data over Contract.

#### Toegepaste stap: ↪️ Expanded EIFFEL - Functie
Code:
```
// relevante kolommen uit de Functie tabel toevoegen aan de Contract tabel
#"↪️ Expanded EIFFEL - Functie" = Table.ExpandTableColumn(Source, "Eiffel - Employee", {"Begindatum_functie", "Einddatum_functie", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager"}, {"Begindatum_functie", "Einddatum_functie", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager.1"}),
```
Beschrijving inhoudelijk ➡️ De relevante kolommen vanuit de queries "EIFFEL - Contract" en "EIFFEL - Functie" worden hier behouden.

#### Toegepaste stap: ➕ Added Kostendrager Samen
Code:
```
// Een nieuwe Kostendrager aanmaken, waarbij de Kostendrager vanuit de Functie tabel leidend is. 
#"➕ Added Kostendrager Samen" = Table.AddColumn(#"↪️ Expanded EIFFEL - Functie", "Kostendrager samen", each if [Kostendrager.1] = null or [Kostendrager] = " " then [Kostendrager] 
else [Kostendrager.1], type text),
```
Beschrijving inhoudelijk ➡️ Hier wordt een nieuwe Kostendrager aangemaakt, waarbij de Kostendrager vanuit de Functie tabel leidend is. 

#### Toegepaste stap: 🚫 Removed Columns oud
Code:
```
// Oude Kostendrager kolommen verwijderen
#"🚫 Removed Columns oud" = Table.SelectColumns(#"➕ Added Kostendrager Samen",{"Medewerker", "Naam", "Begindatum_contract", "Einddatum_contract", "Dienstbetrekking", "Type_contract", "Werkgevernr", "Werkgever", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Matching ID", "Begindatum_functie", "Einddatum_functie", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager samen"}),
```
Beschrijving inhoudelijk ➡️ De oude Kostendrager kolommen zijn hier niet meer nodig.

#### Toegepaste stap: 🖍️ Renamed Kostendrager samen
Code:
```
// Herbenoemen naar Kostendrager
#"🖍️ Renamed Kostendrager samen" = Table.RenameColumns(#"🚫 Removed Columns oud",{{"Kostendrager samen", "Kostendrager"}}),
```
Beschrijving inhoudelijk ➡️ De kolom "Kostendrager samen" wordt hernoemd naar "Kostendrager".

#### Toegepaste stap: 🔍 Filtered Matching ID
Code:
```
// Voor de zekerheid regels eruit halen die geen Matching ID hebben. deze kunnen later toch niet gematched worden
#"🔍 Filtered Matching ID" = Table.SelectRows(#"🖍️ Renamed Kostendrager samen", each [Matching ID] <> null),
```
Beschrijving inhoudelijk ➡️ Hier wordt voor de zekerheid regels eruit gefilterd die geen Matching ID hebben omdat die regels later toch niet gematched kunnen worden.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ // In onderstaande code block worden de relevante functies/contracten overgehouden en wordt gecorrigeerd voor lopende contracten/functies die een null waarde hebben bij de einddatum.

#### Toegepaste stap: 🔍 Filtered op begindatum
Code:
```
// Eruit filteren van personen die geen begindatum hebben. dit komt doordat functies weleens langer willen doorlopen dan contracten. er door de tijdsfilters kan je dus een funcie hebben zonder contract. die gaan er hierdoor uit
#"🔍 Filtered op begindatum" = Table.SelectRows(#"-------", each ([Begindatum_contract] <> null)),
```
Beschrijving inhoudelijk ➡️ {Freek mag invullen}

#### Toegepaste stap: ➕ Added nieuwe einddatum contract
Code:
```
// Nieuwe einddatum maken, om verder berekeningen mee te doen. Lopende Contracten hebben nog geen einddatum dus wordt de datum waarop het rapport gerefreshed wordt gebruikt.
#"➕ Added nieuwe einddatum contract" = Table.AddColumn(#"🔍 Filtered op begindatum", "Custom", each if [Einddatum_contract] =null then Date.From(Date.AddMonths(DateTime.LocalNow(), 3)) else [Einddatum_contract], type date),
```
Beschrijving inhoudelijk ➡️ Hier worden nieuwe einddatums gemaakt om verder berekeningen mee te kunnen doen. Lopende Contracten hebben nog geen einddatum dus wordt de datum waarop het rapport gerefreshed wordt gebruikt.

#### Toegepaste stap: ➕ Added nieuwe einddatum functie
Code:
```
// Nieuwe einddatum maken, om verder berekeningen mee te doen. Lopende Functies hebben nog geen einddatum dus wordt de datum waarop het rapport gerefreshed wordt gebruikt.
#"➕ Added nieuwe einddatum functie" = Table.AddColumn(#"➕ Added nieuwe einddatum contract", "Custom1", each if [Einddatum_functie] =null then Date.From(Date.AddMonths(DateTime.LocalNow(), 3)) else [Einddatum_functie], type date),
```
Beschrijving inhoudelijk ➡️ Hier worden nieuwe einddatums gemaakt om verder berekeningen mee te kunnen doen. Lopende Functies hebben nog geen einddatum dus wordt de datum waarop het rapport gerefreshed wordt gebruikt.

#### Toegepaste stap: 🖍️ Renamed Columns _org
Code:
```
// Herbenoemen van de orginele einddatum kolommen zodat we die in latere berekningen nog kunnen gebruiken
#"🖍️ Renamed Columns _org" = Table.RenameColumns(#"➕ Added nieuwe einddatum functie",{{"Einddatum_functie", "Einddatum_functie_org"}, {"Einddatum_contract", "Einddatum_contract_org"}}),
```
Beschrijving inhoudelijk ➡️ Hier wordt "Einddatum_functie" hernoemd naar "Einddatum_functie_org" en "Einddatum_contract" wordt hernoemd naar "Einddatum_contract_org" zodat deze kolommen in latere berekeningen gebruikt kunnen worden.

#### Toegepaste stap: 🖍️ Renamed Columns
Code:
```
// Nieuwe bepaalde einddatums herbenoemt naar oude naam
#"🖍️ Renamed Columns" = Table.RenameColumns(#"🖍️ Renamed Columns _org",{{"Custom", "Einddatum_contract"}, {"Custom1", "Einddatum_functie"}}),
```
Beschrijving inhoudelijk ➡️ Hier worden bepaalde einddatumkolommen hernoemd naar de oorspronkelijke naam.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ 

#### Toegepaste stap: ➕ Added Huidig Contract
Code:
```
// Bepalen wat het huidige Contract is, hiermee worden straks regels eruit gheaald voor oude contracten.
#"➕ Added Huidig Contract" = Table.AddColumn(#"--------", "Huidig Contract?", each if [Einddatum_contract] = null then "Lopend Contract" else if [Einddatum_contract] >= Date.From(DateTime.LocalNow()) then "Lopend Contract" else "Oud Contract", type text),
```
Beschrijving inhoudelijk ➡️ Hier wordt een nieuwe kolom toegevoegd die categoriseert welk contract lopend is en welk contract oud is. Hiermee worden straks regels eruit gehaald voor oude contracten.

#### Toegepaste stap: ➕ Added Functie geldig tijdens Contract
Code:
```
// Bekijkt of een functie geldig was tijdens het contract, is het antwoord nee, dan filteren we die eruit in de volgende stap
#"➕ Added Functie geldig tijdens Contract" = Table.AddColumn(#"➕ Added Huidig Contract", "Functie geldig tijdens Contract", each if ([Begindatum_functie] = null) or (([Begindatum_functie] >= [Begindatum_contract]) and ([Einddatum_functie] <= [Einddatum_contract]))
then "Ja"


else if ([Begindatum_functie] >= [Begindatum_contract] and [Begindatum_functie] < [Einddatum_contract]) or ([Einddatum_functie] > [Begindatum_contract] and [Einddatum_functie] < [Einddatum_contract])
then "Gedeeltelijk"

else "Nee", type text),
```
Beschrijving inhoudelijk ➡️ Hier wordt een nieuwe kolom toegevoegd die categoriseert of een functie geldig was tijdens het contract zodat alle functies die niet geldig zijn eruit gefilterd kunnen worden.

#### Toegepaste stap: 🔍 Filtered Functie geldig tijdens contract
Code:
```
// Eruit filteren van Nee waardes, dit zijn functies waar geen geldig contract bij hoort
#"🔍 Filtered Functie geldig tijdens contract" = Table.SelectRows(#"➕ Added Functie geldig tijdens Contract", each ([Functie geldig tijdens Contract] <> "Nee")),
```
Beschrijving inhoudelijk ➡️ Hier worden "Nee" waarden in de kolom "Functie geldig tijdens Contract" eruit gefilterd want dit zijn functies waar geen geldig contract bij hoort.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden de gegevens van "Projects" onder elkaar toegevoegd (append).

#### Toegepaste stap: ⏬ Appended Query
Code:
```
// Onder elkaar zetten van AFAS data en Projects data
#"⏬ Appended Query" = Table.Combine({#"------------", #"EP - Contract & Functie"}),
```
Beschrijving inhoudelijk ➡️ Hier wordt de query "EP - Contract & Functie" appended aan deze query want dit zorgt ervoor dat alle Contract en Functie data (vanuit AFAS en Projects) in dezelfde tabel samen staan.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden begin en einddatums gemaakt die rekening houden met contract- en functiedatums.

#### Toegepaste stap: ➕ Added Begindatum berekening
Code:
```
// Bepalen wat de begindatum moet worden van deze berekening. 
// Waarbij rekening gheouden wordt met de relatieve tijdspanne waarin we geintresseerd zijn en de combinatie tussen contract en functie
#"➕ Added Begindatum berekening" = Table.AddColumn(#"----------", "Begindatum berekening", each /* Indien de functie volledig geldig was binnen het contract dan is de begindatum hetzelfde als de begindatum van de functie indien die niet meer dan 12 maanden geleden was */

if [Functie geldig tijdens Contract] = "Ja" and [Begindatum_functie] = null and [Begindatum_contract] <= Date.From(Date.AddMonths(DateTime.LocalNow(), -12))   then 
Date.From(Date.AddMonths(DateTime.LocalNow(), -12))
else if [Functie geldig tijdens Contract] = "Ja" and [Begindatum_functie] = null and [Begindatum_contract] > Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
[Begindatum_contract] 

else if [Functie geldig tijdens Contract] = "Ja" and [Begindatum_functie] <= Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
Date.From(Date.AddMonths(DateTime.LocalNow(), -12))
else if [Functie geldig tijdens Contract] = "Ja" and [Begindatum_functie] > Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
[Begindatum_functie] 


/* Indien de functie gedeeltelijk geldig was binnen het contract dan is de begindatum hetzelfde als de begindatum van de functie indien die begon tijdens een contract, indien de functie al met een vorige contract is begonnen dan is de begindatum van het contract de begindatum van de berekening*/
else if [Functie geldig tijdens Contract] = "Gedeeltelijk" and ([Begindatum_functie] >= [Begindatum_contract] and [Begindatum_functie] <= [Einddatum_contract])  and ([Begindatum_functie] >= Date.From(Date.AddMonths(DateTime.LocalNow(), -12))) 
then [Begindatum_functie] 

else if [Functie geldig tijdens Contract] = "Gedeeltelijk" and ([Begindatum_functie] >= [Begindatum_contract] and [Begindatum_functie] <= [Einddatum_contract])  and ([Begindatum_functie] < Date.From(Date.AddMonths(DateTime.LocalNow(), -12))) 
then Date.From(Date.AddMonths(DateTime.LocalNow(), -12))


else if [Functie geldig tijdens Contract] = "Gedeeltelijk" and ([Einddatum_functie] >= [Begindatum_contract] and [Einddatum_functie] <= [Einddatum_contract]) and ([Begindatum_functie] >= Date.From(Date.AddMonths(DateTime.LocalNow(), -12)))
then [Begindatum_contract]
else if [Functie geldig tijdens Contract] = "Gedeeltelijk" and ([Einddatum_functie] >= [Begindatum_contract] and [Einddatum_functie] <= [Einddatum_contract]) and ([Begindatum_functie] < Date.From(Date.AddMonths(DateTime.LocalNow(), -12))) 
then Date.From(Date.AddMonths(DateTime.LocalNow(), -12))

else "nnb", type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt bepaald wat de begindatum moet worden van deze berekening, waarbij rekening gehouden wordt met de relatieve tijdspanne waarin we geïntresseerd zijn en de combinatie tussen contract en functie.

#### Toegepaste stap: ➕ Added Einddatum berekening
Code:
```
// Bepalen wat de einddatum is waarvoor straks een lijst met datums aangemaakt gaat worden
#"➕ Added Einddatum berekening" = Table.AddColumn(#"➕ Added Begindatum berekening", "Einddatum berekening", each /* indien er bij zowel de functie als contract geen einddatum is dan wordt de datum over 3 maanden genomen*/
if [Einddatum_functie_org] = null and [Einddatum_contract_org] = null then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))
/* Indien de functie gedeeltelijk geldig was binnen het contract dan is de einddatum hetzelfde als de einddatum van het contract indien die binnen 3 maanden afloopt, Indien die verder in de tijd ligt dan is het de datum over 18 maanden*/
else if ([Functie geldig tijdens Contract] = "Gedeeltelijk" and [Einddatum_contract_org] = null) and  [Einddatum_functie_org] <= Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then [Einddatum_functie_org]

else if ([Functie geldig tijdens Contract] = "Gedeeltelijk" and [Einddatum_contract_org] = null)and  [Einddatum_functie_org] > Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))

else if ([Functie geldig tijdens Contract] = "Gedeeltelijk" and [Einddatum_functie_org] >= [Einddatum_contract_org]) and  [Einddatum_contract_org] <= Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
[Einddatum_contract_org]
else if ([Functie geldig tijdens Contract] = "Gedeeltelijk" and [Einddatum_functie_org] >= [Einddatum_contract_org]) and  [Einddatum_contract_org] > Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  

/* indien er bij de functie geen einddatum is dan wordt gekeken of de einddatum van het contract verder ligt dan 3 maanden in de toekomst. als dat zo is krijgt ie die datum en anders de einddatum van het contract*/
else if [Einddatum_functie_org] = null and [Einddatum_contract_org] >= Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))

else if [Einddatum_functie_org] = null and [Einddatum_contract_org] < Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
[Einddatum_contract_org]

/* indien er bij de functie een einddatum is dan wordt gekeken of de einddatum van het functie verder ligt dan 3 maanden in de toekomst. als dat zo is krijgt ie die datum en anders de einddatum van de functie*/
else if [Functie geldig tijdens Contract] = "Ja" and [Einddatum_functie_org] >= Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))

else if [Functie geldig tijdens Contract] = "Ja" and [Einddatum_functie_org] < Date.From(Date.AddMonths(DateTime.LocalNow(), 3))  then 
[Einddatum_functie_org]

else "nnb", type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt bepaald wat de einddatum is waarvoor straks een lijst met datums aangemaakt gaat worden.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt bepaald wat het laatste contract is en wordt de zojuist berekende einddatum hierop aangepast indien de medewerker geen datum uit dienst heeft maar wel een contract voor bepaalde tijd.

#### Toegepaste stap: ∑ Grouped Rows
Code:
```
// Groeperen op laatste Begindatum en einddatum berekening. 
#"∑ Grouped Rows" = Table.Group(#"----", {"Matching ID"}, {{"Laaste Begindatum berekening", each List.Max([Begindatum berekening]), type date}, {"Laatste Einddatum berekening", each List.Max([Einddatum berekening]), type date}}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: ➕ Added Laatste Contract/Functie
Code:
```
// Toevoegen van een kolom die aangeeft dat dit de laatste contract/functie regel is van die persoon als we straks hebben ge-self-merged
#"➕ Added Laatste Contract/Functie" = Table.AddColumn(#"∑ Grouped Rows", "Laatste Contract/Functie", each "Ja", type text),
```
Beschrijving inhoudelijk ➡️ Hier wordt een kolom toegevoegd die aangeeft dat dit de laatste contract/functie regel is van die persoon als we straks hebben ge-self-merged.

#### Toegepaste stap: 🔀 Self Merged Queries
Code:
```
// Samenvoegen van stap "---" en stap "➕ Added Laatste Contract/Functie"
#"🔀 Self Merged Queries" = Table.NestedJoin(#"----", {"Matching ID", "Begindatum berekening", "Einddatum berekening"}, #"➕ Added Laatste Contract/Functie", {"Matching ID", "Laaste Begindatum berekening", "Laatste Einddatum berekening"}, "Grouped Rows", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ Hier gebeurt een samenvoeging van stap "---" en stap "➕ Added Laatste Contract/Functie".

#### Toegepaste stap: ↪️ Expanded Grouped Rows
Code:
```
 // Eruit halen van de koom Laate Contract/Functie
#"↪️ Expanded Grouped Rows" = Table.ExpandTableColumn(#"🔀 Self Merged Queries", "Grouped Rows", {"Laatste Contract/Functie"}, {"Laatste Contract/Functie"}),
```
Beschrijving inhoudelijk ➡️ De kolom "Laatste Contract/Functie" wordt hier aan de tabel toegevoegd.

#### Toegepaste stap: 🖍️ Replaced Value in Laatste Contract/Functie
Code:
```
// Regels die niet gematched zijn in de merge zijn dus niet de laatste Contract/Functie regels
#"🖍️ Replaced Value in Laatste Contract/Functie" = Table.ReplaceValue(#"↪️ Expanded Grouped Rows",null,"Nee",Replacer.ReplaceValue,{"Laatste Contract/Functie"}),
```
Beschrijving inhoudelijk ➡️ Hier worden de null waarden in de kolom "Laatste Contract/Functie" vervangen door de waarde "Nee". Dit zijn de regels die geen match hebben gehad tijdens de merge.

#### Toegepaste stap: ➕ Added Einddatum berekening nieuw
Code:
```
// Toevoegen een kolom die de kolom "Einddatum berekening" corrigeerd voor Bepaalde tijd contracten die gaan aflopen, niet verlengd zijn en waar er geen datum uitdienst bestaat
#"➕ Added Einddatum berekening nieuw" = Table.AddColumn(#"🖍️ Replaced Value in Laatste Contract/Functie", "Einddatum berekening nieuw", each if [#"Laatste Contract/Functie"] = "Ja" and [Medw_Datum_uit_dienst] = null then Date.From(Date.AddMonths(DateTime.LocalNow(), 3))
else [Einddatum berekening], type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt de kolom "Einddatum berekening nieuw" toegevoegd die de kolom "Einddatum berekening" corrigeert voor Bepaalde tijd contracten die gaan aflopen, niet verlengd zijn en waar er geen datum uitdienst bestaat.

#### Toegepaste stap: 🚫 Removed Einddatum berekening
Code:
```
// Verwijderen van de oude kolom
#"🚫 Removed Einddatum berekening" = Table.SelectColumns(#"➕ Added Einddatum berekening nieuw",{"Medewerker", "Naam", "Begindatum_contract", "Einddatum_contract_org", "Dienstbetrekking", "Type_contract", "Werkgevernr", "Werkgever", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Matching ID", "Begindatum_functie", "Einddatum_functie_org", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager", "Einddatum_contract", "Einddatum_functie", "Huidig Contract?", "Functie geldig tijdens Contract", "Begindatum berekening", "Einddatum berekening nieuw"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🖍️ Renamed Einddatum berekening nieuw
Code:
```
// Herbenoemen naar Einddatum berekening
#"🖍️ Renamed Einddatum berekening nieuw" = Table.RenameColumns(#"🚫 Removed Einddatum berekening",{{"Einddatum berekening nieuw", "Einddatum berekening"}}),
```
Beschrijving inhoudelijk ➡️ Hier wordt de kolom "Einddatum berekening nieuw" hernoemd naar "Einddatum berekening".

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt een lijst aangemaakt met de tussenliggende datums.

#### Toegepaste stap: ➕ Added DateDifference
Code:
```
// Hiermee kan straks een lijst gemaakt worden per dag. uiteindelijk hebben dit nodig om goed te mergen met de contract en project uren
#"➕ Added DateDifference" = Table.AddColumn(#"------", "DateDifference", each if Duration.Days([Einddatum berekening] - [Begindatum berekening]) >= 0 
then Duration.Days([Einddatum berekening] - [Begindatum berekening])
else 0),
```
Beschrijving inhoudelijk ➡️ Hier wordt de kolom "DateDifference" aangemaakt die het verschil geeft tussen "Einddatum berekening" en "Begindatum berekening" als dat verschil groter of gelijk is aan 0, anders is de waarde 0. Hiermee kan straks een lijst gemaakt worden per dag. Uiteindelijk is dit nodig om goed te kunnen mergen met de contract en project uren.

#### Toegepaste stap: ➕ Added Dates
Code:
```
// Lijst klaarzetten om in volgende stap uit te klappen. 
// 
// Je maakt hier een lijst van je tussenliggende dagen +1 zodat je ook de begin en eind datum meekrijgt
#"➕ Added Dates" = Table.AddColumn(#"➕ Added DateDifference", "Dates", each List.Dates ([Begindatum berekening] , [DateDifference]+1, #duration (1, 0, 0, 0) )),
```
Beschrijving inhoudelijk ➡️ Hier wordt een lijst gemaakt van de tussenliggende dagen +1 zodat er ook de begin en eind datum meegekregen kunnen worden.

#### Toegepaste stap: ↪️ Expanded Dates
Code:
```
// Met deze actie komt er een regel per dag die ligt tussen begin en einddatum van het dienstverband. dit is inclusief de begin en einddatum
#"↪️ Expanded Dates" = Table.ExpandListColumn(#"➕ Added Dates", "Dates"),
```
Beschrijving inhoudelijk ➡️ Hier komt er een regel per dag die ligt tussen begin en einddatum van het dienstverband. Dit is inclusief de begin en einddatum.

#### Toegepaste stap: 🎨 Changed Type Dates
Code:
```
// Date type geven
#"🎨 Changed Type Dates" = Table.TransformColumnTypes(#"↪️ Expanded Dates",{{"Dates", type date}}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden de weekenddagen eruit gehaald. Dit wordt gedaan door een merge met de DateDimension tabel aangezien die info al daar staat.

#### Toegepaste stap: 🔀 Merged met DateDimension
Code:
```
// Met de lijsten hebben we nu ook weekenddagen aangemaakt, die wil ik er weer uit hebben. Wat weekenddagen zijn hebben we al in de DateDimension zitten. Daarnaast hebben we start of Week nodig voor een verdere berekening in de merge PLanned Hours & Contract
#"🔀 Merged met DateDimension" = Table.NestedJoin(#"---------", {"Dates"}, DateDimension, {"Datum"}, "DateDimension", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ Hier wordt een merge gedaan tussen deze query en de query "DateDimension". Met de zojuist gemaakte lijsten zijn nu ook weekenddagen aangemaakt en die moeten eruit. Weekenddagen zijn te identificeren in de DateDimension. Daarnaast is start of Week nodig voor een verdere berekening in de merge Planned Hours & Contract.

#### Toegepaste stap: ↪️ Expanded DateDimension
Code:
```
// Alleen Dag van de Week & Start of Week eruit halen
#"↪️ Expanded DateDimension" = Table.ExpandTableColumn(#"🔀 Merged met DateDimension", "DateDimension", {"Dag van week", "Datum", "Start of week"}, {"Dag van week", "Datum", "Start of week"}),
```
Beschrijving inhoudelijk ➡️ Hier worden alleen Dag van de Week en Start of Week eruit gehaald.

#### Toegepaste stap: 🔍 Filtered Dag van Week
Code:
```
// Op kolom Dag van Week gefiltert zodat de weekend dagen (6&7) eruit gaan
#"🔍 Filtered Dag van Week" = Table.SelectRows(#"↪️ Expanded DateDimension", each ([Dag van week] <> 6 and [Dag van week] <> 7)),
```
Beschrijving inhoudelijk ➡️ Weekenddagen worden hier uit de kolom Dag van Week eruit gefilterd.

#### Toegepaste stap: 🚫 Removed Columns
Code:
```
// Verwijderen van overbodige kolommen
#"🚫 Removed Columns" = Table.SelectColumns(#"🔍 Filtered Dag van Week",{"Medewerker", "Naam", "Dienstbetrekking", "Type_contract", "Werkgevernr", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Matching ID", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager", "Einddatum_contract", "Huidig Contract?", "Datum"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt toegevoegd hoeveel iemand zou moeten werken volgens zijn/haar op dat moment geldende contract.

#### Toegepaste stap: 🔀 Merged Queries Rooster employee bewerkt
Code:
```
// Merge op basis van Medewerker en Datum. zodat je de contracturen erbij kan zetten
#"🔀 Merged Queries Rooster employee bewerkt" = Table.NestedJoin(#"-----------", {"Matching ID", "Datum"}, #"Rooster employee bewerkt", {"Matching ID", "Datum"}, "Rooster employee bewerkt", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ Hier wordt een merge gedaan tussen deze query en de query "Rooster employee bewerkt" op basis van de kolommen "Matching ID" en "Datum" zodat de contracturen erbij gezet kunnen worden.

#### Toegepaste stap: ↪️ Expanded EIFFEL - Rooster employee bewerkt
Code:
```
// Contracturen erbij zetten voor die dag
#"↪️ Expanded EIFFEL - Rooster employee bewerkt" = Table.ExpandTableColumn(#"🔀 Merged Queries Rooster employee bewerkt", "Rooster employee bewerkt", {"Uren_contract_pd"}, {"Uren_contract_pd"}),
```
Beschrijving inhoudelijk ➡️ Hier komt de kolom "Uren_contract_pd" erbij waarin de contracturen per dag staan.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt er eerst gesorteerd en vervolgens worden de roosters aangevuld.

#### Toegepaste stap: ⤵️ Sorted Rows
Code:
```
// Soorten van de rijen op MatchingID en Datum
// Zo staat alles op chronologische volgorde
#"⤵️ Sorted Rows" = Table.Sort(#"---------------------",{{"Matching ID", Order.Ascending}, {"Datum", Order.Ascending}}),
```
Beschrijving inhoudelijk ➡️ Hier wordt een A-Z sortering gemaakt op basis van de kolommen "Matching ID" en "Datum" zodat alles op chronologische volgorde staat.

#### Toegepaste stap: ⬇️ Filled Down Uren_contract_pd
Code:
```
// Roosters zijn alleen ingevuld voor contractperiodes. 
// Er zijn echter medewerkers die een aflopend contract hebben maar nog geen datum uit dienst. die moeten vor die periode wel weergeven worden. Via deze manier wordt hier het rooster gebruikt van de laatste beschikbare periode
#"⬇️ Filled Down Uren_contract_pd" = Table.FillDown(#"⤵️ Sorted Rows",{"Uren_contract_pd"})
```
Beschrijving inhoudelijk ➡️ Lege regels in de kolom "Uren_contract_pd" krijgen hier geïmputeerde waardes door middel van een fill-down omdat roosters alleen zijn ingevuld voor contractperiodes. Er zijn echter medewerkers die een aflopend contract hebben maar nog geen datum uit dienst. Die medewerkers moeten voor die periode wel weergegeven worden. Via deze manier wordt hier het rooster gebruikt van de laatste beschikbare periode.

Een query in Power Query heeft altijd ook een einde:
```
in
#"⬇️ Filled Down Uren_contract_pd"
```

---

### Query: Voorcalculatie bewerkt

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = #"Voorcalculatie samen",
```

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt eerst bepaald wat de begin en einddatum moeten worden waarvoor datums aangemaakt moeten worden. Daarna wordt er een lijst gemaakt met de tussenliggende datums.

#### Toegepaste stap: ➕ Added Begindatum berekening
Code:
```
// Bepalen wat de begindatum moet worden van deze berekening. 
// Waarbij rekening gheouden wordt met de relatieve tijdspanne waarin we geintresseerd zijn en de combinatie tussen contract en functie
#"➕ Added Begindatum berekening" = Table.AddColumn(#"----------", "Begindatum berekening", each if [Begindatum] <= Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
Date.From(Date.AddMonths(DateTime.LocalNow(), -12))

else if [Begindatum] > Date.From(Date.AddMonths(DateTime.LocalNow(), -12)) then 
[Begindatum] 

else "nnb", type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt bepaald wat de begindatum moet worden van deze berekening, waarbij rekening gehouden wordt met de relatieve tijdspanne waarin we geïnteresseerd zijn en de combinatie tussen contract en functie.

#### Toegepaste stap: ➕ Added Einddatum berekening
Code:
```
// Bepalen wat de einddatum is waarvoor straks een lijst met datums aangemaakt gaat worden. 
// 
// Indien er geen einddatum is dan wordt de datum over 3 maanden genomen
#"➕ Added Einddatum berekening" = Table.AddColumn(#"➕ Added Begindatum berekening", "Einddatum berekening", each if [Einddatum] > Date.From(Date.AddMonths(DateTime.LocalNow(), 3)) or [Einddatum] = null then 
Date.From(Date.AddMonths(DateTime.LocalNow(), 3))

else if [Einddatum] <= Date.From(Date.AddMonths(DateTime.LocalNow(), 3)) then 
[Einddatum]

else "nnb", type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt bepaald wat de einddatum is waarvoor straks een lijst met datums aangemaakt gaat worden. Indien er geen einddatum is dan wordt de datum over 3 maanden genomen.

#### Toegepaste stap: ➕ Added #Duur project
Code:
```
// Berekenen van het aantal doordeweekse dagen tussen de Start berekening datum en de einddatum.
// Hier wordt expliciet geen rekening gehouden met feestdagen.
// Indien er geen einddatum is dan wordt er 0 neergezet, omdat we het niet kunnen berekenen
#"➕ Added #Duur project" = Table.AddColumn(#"➕ Added Einddatum berekening", "#Duur project", each if [Einddatum] = null then 0 else
/* 
eerst berekenen we hoeveel werkdagen er in de eerste week zitten van het dienstverband.*/ 
(if (Date.DayOfWeek([Begindatum], Day.Monday)) >5 then 0 else (7- (Date.DayOfWeek([Begindatum], Day.Monday)) -2))
+ 
/* het aantal werkdagen tussen de eerste en laatste werkweek */ 
((Duration.Days(Date.StartOfWeek([Einddatum], Day.Monday) - Date.EndOfWeek([Begindatum], Day.Monday)) -1)/7*5)
+
/* het aantal werkdagen in de laatste werkweek */
(if (Date.DayOfWeek([Einddatum], Day.Monday))>=4 then 5 else (Date.DayOfWeek([Einddatum], Day.Monday)+1)), type number),
```
Beschrijving inhoudelijk ➡️ Hier wordt het aantal doordeweekse dagen tussen de Start berekening datum en de einddatum berekend. Er wordt expliciet geen rekening gehouden met feestdagen. Indien er geen einddatum is dan wordt er 0 neergezet, omdat het niet berekend kan worden.

#### Toegepaste stap: ➕ Added DateDifference
Code:
```
// Hiermee kan straks een lijst gemaakt worden per dag. uiteindelijk hebben dit nodig om goed te mergen met de contract en project uren
#"➕ Added DateDifference" = Table.AddColumn(#"➕ Added #Duur project", "DateDifference", each if Duration.Days([Einddatum berekening] - [Begindatum berekening]) >= 0 
then Duration.Days([Einddatum berekening] - [Begindatum berekening])
else 0),
```
Beschrijving inhoudelijk ➡️ Hier wordt het verschil berekend tussen begindatum en einddatum. Met deze berekening kan straks een lijst gemaakt worden per dag. Uiteindelijk is dit nodig om goed te mergen met de contract en project uren.

#### Toegepaste stap: ➕ Added Dates
Code:
```
// Lijst klaarzetten om in volgende stap uit te klappen. 
// 
// Je maakt hier een lijst van je tussenliggende dagen +1 zodat je ook de begin en eind datum meekrijgt
#"➕ Added Dates" = Table.AddColumn(#"➕ Added DateDifference", "Dates", each List.Dates ([Begindatum berekening] , [DateDifference]+1, #duration (1, 0, 0, 0) )),
```
Beschrijving inhoudelijk ➡️ Hier wordt een kolom met een lijst met datums klaargezet. Deze lijst bevat de tussenliggende dagen +1 zodat er ook de begin en eind datum in zitten. Deze lijst is nodig om uitgeklapt te worden in de volgende stap.

#### Toegepaste stap: ↪️ Expanded Dates
Code:
```
// Met deze actie komt er een regel per dag die ligt tussen begin en einddatum van het dienstverband. dit is inclusief de begin en einddatum
#"↪️ Expanded Dates" = Table.ExpandListColumn(#"➕ Added Dates", "Dates"),
```
Beschrijving inhoudelijk ➡️ Hier wordt de kolom met de lijst erin uitgeklapt. Dit is inclusief de begin en einddatum. Dit wordt gedaan zodat er een regel per dag die ligt tussen begin en einddatum van het dienstverband komt.

#### Toegepaste stap: 🎨 Changed Type Dates
Code:
```
// Date type geven
#"🎨 Changed Type Dates" = Table.TransformColumnTypes(#"↪️ Expanded Dates",{{"Dates", type date}}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden de weekenddagen eruit gehaald. Dit wordt gedaan door een merge met de DateDimension tabel aangezien die info al daar staat.

#### Toegepaste stap: 🔀 Merged met DateDimension
Code:
```
// Met de lijsten hebben we nu ook weekenddagen aangemaakt, die wil ik er weer uit hebben. Wat weekenddagen zijn hebben we al in de DateDimension zitten. Daarnaast hebben we start of Week nodig voor een verdere berekening in de merge PLanned Hours & Contract
#"🔀 Merged met DateDimension" = Table.NestedJoin(#"---------", {"Dates"}, DateDimension, {"Datum"}, "DateDimension", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ In de lijst zijn er ook weekenddagen aangemaakt, maar die moeten eruit. De weekenddagen kunnen al geïdentificeerd met de DateDimension. Daarnaast is start of Week nodig voor een verdere berekening in de merge tussen Planned Hours en Contract.

#### Toegepaste stap: ↪️ Expanded DateDimension
Code:
```
// Alleen Dag van de Week & Start of Week eruit halen
#"↪️ Expanded DateDimension" = Table.ExpandTableColumn(#"🔀 Merged met DateDimension", "DateDimension", {"Dag van week", "Datum"}, {"Dag van week", "Datum"}),
```
Beschrijving inhoudelijk ➡️ Alleen Dag van de Week & Start of Week worden hier eruit gehaald.

#### Toegepaste stap: 🔍 Filtered Dag van Week
Code:
```
// Op kolom Dag van Week gefiltert zodat de weekend dagen (6&7) eruit gaan
#"🔍 Filtered Dag van Week" = Table.SelectRows(#"↪️ Expanded DateDimension", each ([Dag van week] <> 6 and [Dag van week] <> 7)),
```
Beschrijving inhoudelijk ➡️ Hier wordt op kolom Dag van Week gefilterd zodat de weekend dagen (6&7) eruit gaan.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt een kolom aangemaakt die aangeeft hoeveel uren er per dag geschreven mogen worden voor dit project.

#### Toegepaste stap: ➕ Added Uren_inzet_pd
Code:
```
// Toewijzen van de projecturen naar een gemiddelde per werkdag
#"➕ Added Uren_inzet_pd" = Table.AddColumn(#"-----------", "Uren_inzet_pd", each /* Indien het project een Sourcing of FixedFee project is, dan moet het aantal werkbare uren per dag berekend worden door het aantal_eenheden te delen door het totaal aantal doordeweeksedagen gedurende de projectperiode */

if ([Projectgroep] =   "SOURC" or [Projectgroep] = "FIXFEE") and [#"#Duur project"] <> 0
then [Aantal_eenheden]/[#"#Duur project"] 

/* Indien de duur van de projectperiode gelijk is aan 0, doordat er geen einddatum is, dan laten we het aantal eenheden zien, Hierdoor krijg je meteen een hele grote afwijking die opvalt. */

else if ([Projectgroep] =   "SOURC" or [Projectgroep] = "FIXFEE") and [#"#Duur project"] = 0
then 0

else ([Aantal_eenheden]/5), type number),
```
Beschrijving inhoudelijk ➡️ Hier worden de projecturen toegewezen naar een gemiddelde per werkdag.

#### Toegepaste stap: 🚫 Removed Columns
Code:
```
// Verwijderen van overbodige kolommen
#"🚫 Removed Columns" = Table.SelectColumns(#"➕ Added Uren_inzet_pd",{"Project", "Omschrijving", "AccounNaam", "Medewerker", "Urensoort", "Uursoort oms", "Begindatum", "Einddatum", "Projectgroep", "Matching ID", "Datum", "Uren_inzet_pd"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🔄️ Replaced Value Uren_inzet_pd
Code:
```
// Indien er een null waarde is dan betekent dat dat die medewerker op die dag niet aan dat project werkt, dus is het geen onbekende waarde maar een 0 waarde
// dit is een error preventie
#"🔄️ Replaced Value Uren_inzet_pd" = Table.ReplaceValue(#"🚫 Removed Columns",null,0,Replacer.ReplaceValue,{"Uren_inzet_pd"})
```
Beschrijving inhoudelijk ➡️ Hier worden null waarden vervangen door "0". Indien er een null waarde is dan betekent dat dat die medewerker op die dag niet aan dat project werkt, dus is het geen onbekende waarde maar een 0 waarde. Dit is een error preventie.

Een query in Power Query heeft altijd ook een einde:
```
in
#"🔄️ Replaced Value Uren_inzet_pd"
```

---

## Map: Eindbestand ➡️ Hier worden de Voorcalculatie bewerkt data en de Contract & Functie data op project per werkdag niveau aan elkaar samengevoegd. Uiteindelijk komt er de tabel "Project uren & Contract info" uit die aangeeft of medewerkers wel of niet beschikbaar zijn en ook per wanneer een medewerker beschikbaar is. Daarnaast is er hier ook een tabel met de structuur van Team Eiffel te vinden zodat er Row Level Security op kostenplaatsniveau toegepast kan worden.

### Query: Project uren & Contract info

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Table.NestedJoin(#"Voorcalculatie bewerkt", {"Matching ID", "Datum"}, #"Contract & Functie", {"Matching ID", "Datum"}, "Contract & Functie", JoinKind.FullOuter),
```

#### Toegepaste stap: ↪️ Expanded Contract & Functie
Code:
```
 #"↪️ Expanded Contract & Functie" = Table.ExpandTableColumn(Source, "Contract & Functie", {"Medewerker", "Naam", "Dienstbetrekking", "Type_contract", "Werkgevernr", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Matching ID", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager", "Einddatum_contract", "Huidig Contract?", "Datum", "Uren_contract_pd"}, {"Medewerker.1", "Naam", "Dienstbetrekking", "Type_contract", "Werkgevernr", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Matching ID.1", "Functie", "Kostenpl", "Kostenplaats", "Kostendrager", "Einddatum_contract", "Huidig Contract?", "Datum.1", "Uren_contract_pd"}),
```

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden kolommen samengevoegd.

#### Toegepaste stap:  ➕ Added Datum samen
Code:
```
// Samenvoegen van de 2 losse datums voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie (wat heel raar is, maar toch)
#" ➕ Added Datum samen" = Table.AddColumn(#"--------", "Datum samen", each if [Datum] = null then [Datum.1] else [Datum], type date),
```
Beschrijving inhoudelijk ➡️ Hier worden de 2 Datumkolommen samengevoegd tot 1 kolom voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie (wat heel raar is, maar toch).

#### Toegepaste stap: ➕ Added Medewerker samen
Code:
```
// Samenvoegen van de 2 losse medewerker kolommen voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie (wat heel raar is, maar toch)
#"➕ Added Medewerker samen" = Table.AddColumn(#" ➕ Added Datum samen", "Medewerker samen", each if [Medewerker] = null then [Medewerker.1] else [Medewerker], type text),
```
Beschrijving inhoudelijk ➡️ Hier worden de 2 Medewerkerkolommen samengevoegd tot 1 kolom voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie (wat heel raar is, maar toch).

#### Toegepaste stap: ➕ Added Matching ID samen
Code:
```
// Samenvoegen van de 2 losse Matchig ID kolommen voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie (wat heel raar is, maar toch)
#"➕ Added Matching ID samen" = Table.AddColumn(#"➕ Added Medewerker samen", "Matching ID samen", each if [Matching ID] = null then [Matching ID.1] else [Matching ID], type text),
```
Beschrijving inhoudelijk ➡️ Hier worden de 2 Matching ID-kolommen samengevoegd tot 1 kolom voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie (wat heel raar is, maar toch).

#### Toegepaste stap: ➕ Added Kostenplaats samen
Code:
```
// Samenvoegen van de 2 losse kostenplaatskolommen voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie. De Kostenplaats vanuit de Functie is leidend en niet die vanuit het project. 
#"➕ Added Kostenplaats samen" = Table.AddColumn(#"➕ Added Matching ID samen", "Kostenpl samen", each if [Kostenpl] = null then [Kostenplaats] else [Kostenpl], type text),
```
Beschrijving inhoudelijk ➡️ Hier worden de 2 Kostenplaatskolommen samengevoegd tot 1 kolom voor het geval er voor iemand geen projecten zijn of er wel projecten zijn maar geen functie/contract combinatie. De Kostenplaats vanuit de Functie is leidend en niet die vanuit het project.

#### Toegepaste stap: 🚫 Removed Other Columns
Code:
```
// Verwijderen van oude losse kolommen
#"🚫 Removed Other Columns" = Table.SelectColumns(#"➕ Added Kostenplaats samen",{"Project", "Omschrijving", "Begindatum", "Einddatum", "Projectgroep", "Uren_inzet_pd", "Naam", "Dienstbetrekking", "Type_contract", "Werkgevernr", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Functie", "Kostendrager", "Einddatum_contract", "Huidig Contract?", "Uren_contract_pd", "Datum samen", "Medewerker samen", "Matching ID samen", "Kostenpl samen"}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🖍️ Renamed Columns
Code:
```
// Herbenoemen van kolommen
#"🖍️ Renamed Columns" = Table.RenameColumns(#"🚫 Removed Other Columns",{{"Datum samen", "Datum"}, {"Medewerker samen", "Medewerker"}, {"Kostenpl samen", "Kostenpl"}, {"Begindatum", "Begindatum project"}, {"Einddatum", "Einddatum project"}, {"Matching ID samen", "Matching ID"}}),
```
Beschrijving inhoudelijk ➡️ {invullen}

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block worden bepaalde medewerkers waar de focus niet op ligt eruit gefilterd.

#### Toegepaste stap: 🔍 Filtered Kostendrager
Code:
```
// Alleen de Waardes "D" en "Z" blijven over.
#"🔍 Filtered Kostendrager" = Table.SelectRows(#"-------------", each [Kostendrager] = "D" or [Kostendrager] = "Z"),
```
Beschrijving inhoudelijk ➡️ Hier worden alle waarden in de kolom Kostendrager die geen "D" of "Z" zijn weggefilterd.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ 

#### Toegepaste stap: ⨊ Grouped Rows
Code:
```
// Groperen van de projecten per dag 
#"⨊ Grouped Rows" = Table.Group(#"-----------", {"Matching ID", "Medewerker", "Naam", "Functie", "Kostendrager", "Kostenpl", "Werkgevernr", "Dienstbetrekking", "Type_contract", "Einddatum_contract", "Huidig Contract?", "Medw_Datum_in_dienst", "Medw_Datum_uit_dienst", "Datum"}, {{"Uren_inzet_pd", each List.Sum([Uren_inzet_pd]), type nullable number}, {"Uren_contract_pd", each List.Average([Uren_contract_pd]), type nullable number}}),
```
Beschrijving inhoudelijk ➡️ Hier worden de projecten op dagniveau gegroepeerd.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ 

#### Toegepaste stap: 🔄️ Replaced Value Uren_inzet_pd
Code:
```
// Indien er een null waarde is, dan was er op deze datum geen project. Dat betekend niet dat we neit weten of er een project was maar dat deze 0 uren zijn.
// Dit is een error preventie
#"🔄️ Replaced Value Uren_inzet_pd" = Table.ReplaceValue(#"------------",null,0,Replacer.ReplaceValue,{"Uren_inzet_pd"}),
```
Beschrijving inhoudelijk ➡️ Hier worden de null waarden vervangen door 0 in de kolom Uren_inzet_pd. Indien er een null waarde is, dan was er op deze datum geen project. Dat betekent niet dat het niet bekend is of er een project was, maar dat deze 0 uren zijn.

#### Toegepaste stap: ➕ Added Beschikbaarheid?
Code:
```
// Toevoegen van een kolom die aangeeft of iemand op die dag op de bank zat.
// Hierbij moet iemand een beschikbaarheid lager dan 70% hebben om op de bank te zitten.
#"➕ Added Beschikbaarheid?" = Table.AddColumn(#"🔄️ Replaced Value Uren_inzet_pd", "Beschikbaar?", each if ([Uren_inzet_pd]/[Uren_contract_pd]) <0.7 then "Ja" 
else if ([Uren_inzet_pd]/[Uren_contract_pd]) < 1 then "Frictie"
else "Nee", type text),
```
Beschrijving inhoudelijk ➡️ Hier wordt een kolom toegevoegd die aangeeft of iemand op een bepaalde dag op de bank zit. Als iemand voor minder dan 70% van de contracturen ingezet is, dan zit die medewerker op de bank.

#### Toegepaste stap: 🔍 Filtered Kostenpl
Code:
```
// Eruit filteren van Jupos, KP0085
#"🔍 Filtered Kostenpl" = Table.SelectRows(#"➕ Added Beschikbaarheid?", each [Kostenpl] <> "KP0085"),
```
Beschrijving inhoudelijk ➡️ De waarde "KP0085" in de kolom Kostenpl wordt hier weggefilterd.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt er toegevoegd sinds wanneer een medewerker op de bank zit. Dat is of de laatste dag van een project of wanneer de medewerker voor het eerst in dienst is gekomen.

#### Toegepaste stap: ⤵️ Sorted Rows
Code:
```
// Sorteren van de rijen o.b.v. Matching ID en Datum
#"⤵️ Sorted Rows" = Table.Sort(#"---------------",{{"Matching ID", Order.Ascending}, {"Datum", Order.Ascending}}),
```
Beschrijving inhoudelijk ➡️ Hier wordt de tabel gesorteerd op basis van Matching ID en Datum.

#### Toegepaste stap: ➕ Added Index
Code:
```
// Index toevoegen voor de huidige datum
#"➕ Added Index" = Table.AddIndexColumn(#"⤵️ Sorted Rows", "Index", 0, 1, Int64.Type),
```
Beschrijving inhoudelijk ➡️ Hier wordt een Indexkolom toegevoegd voor de huidige datum/dag.

#### Toegepaste stap: ➕ Added Index.Vorige
Code:
```
// Toevoegen van een index kolom om de vorige regel ernaast te kunnen zetten
#"➕ Added Index.Vorige" = Table.AddIndexColumn(#"➕ Added Index", "Index.Vorige", -1, 1, Int64.Type),
```
Beschrijving inhoudelijk ➡️ Hier wordt nog een Indexkolom toegevoegd die de waarde van de vorige Indexkolom -1 bevat. Dit wordt gedaan om de vorige regel naast de huidige regel te kunnen zetten.

#### Toegepaste stap: 🔀 Self Merged Queries Index
Code:
```
// Hiermee zet je de volgende datum naast de huidige
#"🔀 Self Merged Queries Index" = Table.NestedJoin(#"➕ Added Index.Vorige", {"Matching ID", "Index.Vorige"}, #"➕ Added Index.Vorige", {"Matching ID", "Index"}, "➕ Added Index.Volgende", JoinKind.LeftOuter),
```
Beschrijving inhoudelijk ➡️ Aan de hand van deze merge wordt de huidige datum naast de volgende datum gezet.

#### Toegepaste stap: ↪️ Expanded ➕ Added Index.Volgende
Code:
```
#"↪️ Expanded ➕ Added Index.Volgende" = Table.ExpandTableColumn(#"🔀 Self Merged Queries Index", "➕ Added Index.Volgende", {"Beschikbaar?"}, {"Beschikbaar?.1"}),
```

#### Toegepaste stap: ➕ Added Verandering beschikbaarheid?
Code:
```
// toevoegen van een kolom die aangeeft of de beschikbaarheid van vandaag anders is dan die van gisteren
#"➕ Added Verandering beschikbaarheid?" = Table.AddColumn(#"↪️ Expanded ➕ Added Index.Volgende", "Verandering beschikbaarheid?", each if [#"Beschikbaar?"] <> [#"Beschikbaar?.1"] then "Ja" else "Nee", type text),
```
Beschrijving inhoudelijk ➡️ Hier wordt een kolom toegevoegd die aangeeft of de beschikbaarheid van vandaag anders is dan die van gisteren.

#### Toegepaste stap: 🔍 Filtered Rows
Code:
```
// Filteren op Beschikbaarheid "Ja" 
// en op
// Beschikbaar.1 <> null 
// De allereerst datum, is altijd anders dan de vorige (aangezien de vorige niet bestaat). deze wil je niet meenemen, dus haal je eruit
#"🔍 Filtered Rows" = Table.SelectRows(#"➕ Added Verandering beschikbaarheid?", each ([#"Verandering beschikbaarheid?"] = "Ja") and ([#"Beschikbaar?.1"] <> null)),
```
Beschrijving inhoudelijk ➡️ Hier worden alleen de rijen behouden waar de kolom Verandering beschikbaarheid? de waarde "Ja" bevat en de waarde van kolom Beschikbaar?.1 ongelijk is aan null. De allereerst datum, is altijd anders dan de vorige (aangezien de vorige niet bestaat). Deze moet niet meegenomen worden en wordt hier dus eruit gehaald.

#### Toegepaste stap: ➕ Added Index Beschikbaarheid
Code:
```
// Toevoegen van een kolom Index beschikbaarheid
#"➕ Added Index Beschikbaarheid" = Table.AddIndexColumn(#"🔍 Filtered Rows", "Index Beschikbaarheid", 1, 1, Int64.Type),
```
Beschrijving inhoudelijk ➡️ Hier wordt een kolom Index Beschikbaarheid toegevoegd.

#### Toegepaste stap: 🔀 Self Merged Queries
Code:
```
#"🔀 Self Merged Queries" = Table.NestedJoin(#"➕ Added Verandering beschikbaarheid?", {"Matching ID", "Datum"}, #"➕ Added Index Beschikbaarheid", {"Matching ID", "Datum"}, "Added Index", JoinKind.LeftOuter),
```

#### Toegepaste stap: ↪️ Expanded Added Index
Code:
```
#"↪️ Expanded Added Index" = Table.ExpandTableColumn(#"🔀 Self Merged Queries", "Added Index", {"Datum"}, {"Datum.1"}),
```

#### Toegepaste stap: ⬇️ Filled Down Datum.1
Code:
```
// naar beneden aanvulen van de datums zodat bij alle regels binnen die groep beschikbaarheid dezelfde datum staat
#"⬇️ Filled Down Datum.1" = Table.FillDown(#"↪️ Expanded Added Index",{"Datum.1"}),
```
Beschrijving inhoudelijk ➡️ Hier worden de datums van de zojuist uitgevouwde kolom naar beneden aangevuld zodat bij alle regels binnen die groep beschikbaarheid dezelfde datum staat.

#### Toegepaste stap: 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥 Code Block 🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥🟥

- Beschrijving ➡️ In onderstaande code block wordt er toegevoegd sinds wanneer een medewerker op de bank zit. Dat is of de laatste dag van een project of wanneer de medewerker voor het eerst in dienst is gekomen.

#### Toegepaste stap: ➕ Added Beschikbaar per:
Code:
```
// Toevoegen van een kolom die aangeeft wanner iemand beschikbaar is (als ze beschikbaar zijn). Indien iemand net in dienst komt wordt de in dienst datum gebruikt
#"➕ Added Beschikbaar per:" = Table.AddColumn(#"---------------------", "Beschikbaar per:", each if [#"Beschikbaar?"] = "Nee" then null else if [Datum.1] = null then [Medw_Datum_in_dienst] else [Datum.1], type date),
```
Beschrijving inhoudelijk ➡️ Hier wordt een kolom toegevoegd die aangeeft per welke datum een medewerker beschikbaar is (als ze beschikbaar zijn). Indien iemand net in dienst komt wordt de in dienst datum gebruikt.

#### Toegepaste stap: 🚫 Removed overbodige Columns
Code:
```
#"🚫 Removed overbodige Columns" = Table.SelectColumns(#"➕ Added Beschikbaar per:",{"Matching ID", "Medewerker", "Naam", "Functie", "Kostendrager", "Kostenpl", "Werkgevernr", "Dienstbetrekking", "Type_contract", "Einddatum_contract", "Datum", "Uren_inzet_pd", "Uren_contract_pd", "Beschikbaar?", "Beschikbaar per:"})
```

Een query in Power Query heeft altijd ook een einde:
```
in
#"🚫 Removed overbodige Columns"
```

### Query: DIM - Structuur Team Eiffel

Een query in Power Query begint altijd met de code:
```
let
```

#### Toegepaste stap: Source
Code:
```
Source = Excel.Workbook(File.Contents("C:\Users\rbeni\OneDrive - DPA Group N.V\Documentatie PowerBI\Bezetting\Inputbestanden\General data.xlsx"), null, true),
```

#### Toegepaste stap: Navigation
Code:
```
#"DIM - Structuur Team Eiffel_Sheet" = Source{[Item="DIM - Structuur Team Eiffel",Kind="Sheet"]}[Data],
```

#### Toegepaste stap: Promoted Headers
Code:
```
#"Promoted Headers" = Table.PromoteHeaders(#"DIM - Structuur Team Eiffel_Sheet", [PromoteAllScalars=true]),
```

#### Toegepaste stap: Changed Type
Code:
```
#"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Kostenplaats code", type text}, {"Kostenplaats", type text}, {"Type kostenplaats code", Int64.Type}, {"Type kostenplaats", type text}, {"Vertical code", Int64.Type}, {"Vertical", type text}, {"Tekenbevoegde Projecten gbr", type text}, {"Tekenbevoegde Projecten", type text}, {"Tekenbevoegde HRM gbr", type number}, {"Tekenbevoegde HRM", type text}, {"Gbl.", Int64.Type}, {"Business Unit Director gbr", type text}, {"Business Unit Director", type text}, {"Vertical Director gbr", type text}, {"Vertical Director", type text}})
```

Een query in Power Query heeft altijd ook een einde:
```
in
#"Changed Type"
```

---

