# Předání projektu – Deník tankování pro Renault Clio 2004

## Kde projekt běží

- Aplikace: https://kemalius.github.io/clio-spotreba/
- Zdroj: https://github.com/kemalius/clio-spotreba, větev `main`, soubor `index.html`.
- GitHub Pages publikuje z kořene větve `main`. Aplikace je jeden soubor HTML s CSS a JavaScriptem, bez sestavení. Klient Supabase se načítá z CDN s připnutou verzí.
- Soukromá data synchronizuje Supabase Free, projekt `clio-spotreba`, reference `jgsuttbszovbrmhzhbbx`. Zdrojový kód je veřejný, data se čtou po přihlášení.

## Co aplikace umí

- Eviduje tankování podle data, tachometru, litrů, ceny a označení „do plna“. Tankování lze upravit nebo smazat.
- Počítá spotřebu mezi plnými nádržemi včetně mezilehlých částečných tankování. Ukazuje posledních osm úseků a cenu za litr u jednotlivých tankování.
- Sčítá palivo a další výdaje. Uvádí Kč/km za palivo a za všechny výdaje podle rozdílu prvního a posledního zaznamenaného stavu tachometru.
- Ukládá termíny STK a pojištění a zobrazuje odpočet v deníku.
- Bez přihlášení používá místní úložiště prohlížeče. Po přihlášení používá cloud. Tlačítko pro přenos místních záznamů importuje tankování a výdaje.

## Data a synchronizace

- Přihlášení v deníku používá Supabase Auth s e-mailem a heslem. GitHub přihlášení do správy Supabase je samostatné.
- Tabulka `public.clio_diary` má jeden řádek na uživatele. Primární klíč `user_id` odkazuje na `auth.users.id`. Pole JSONB `fuel` a `expenses` obsahují seznamy záznamů, `reminders` termíny a `imported_ids` identifikátory importovaných záznamů.
- RLS omezuje čtení, vytváření a změnu řádku na přihlášeného vlastníka. `revision` chrání před tichým přepsáním souběžné změny z jiného zařízení.
- Klíče místního úložiště jsou `clio-2004-refuels-v1`, `clio-2004-expenses-v1` a `clio-2004-reminders-v1`.
- Pokud potvrzovací e-maily míří na localhost, zkontroluj v Supabase **Authentication → URL Configuration** hodnotu **Site URL**. Produkční adresa je `https://kemalius.github.io/clio-spotreba/`.

## Jak pokračovat v Claude nebo jiném účtu ChatGPT

1. Přilož tento soubor a pošli odkaz na repozitář. Claude Code může repozitář naklonovat příkazem `git clone https://github.com/kemalius/clio-spotreba.git`.
2. Pro změny dej novému nástroji přístup k zápisu do repozitáře. Pokud potřebuje měnit databázi, připoj mu svůj projekt Supabase. Hesla, přístupové tokeny a klíče s oprávněním správce nech mimo konverzaci a mimo repozitář.
3. Před změnami načti aktuální stav větve `main`. Po nasazení ověř aplikaci na produkční adrese a při úpravách synchronizace vyzkoušej přihlášení a načtení záznamů na dvou zařízeních.

## Zálohy a případná migrace

- Změna účtu ChatGPT nebo přechod na Claude nevyžaduje přesun fungující aplikace. Kód zůstává na GitHubu a data v původním Supabase.
- Tento dokument předává kontext. Pro kopii kódu naklonuj repozitář nebo použij **Code → Download ZIP** na GitHubu.
- Soukromé záznamy zálohuj zvlášť. Ve správě Supabase lze otevřít tabulku `clio_diary` a exportovat její data. Export ulož soukromě. Záznamy zadané bez přihlášení nejdřív přenes do cloudu na zařízení, kde jsou uložené. Termíny uložené pouze místně si přepiš po přihlášení.
- Při přesunu do nového projektu Supabase zachovej schéma i RLS a při importu přiřaď záznamy k ID uživatele v cílovém projektu. Ověř, že se po přihlášení zobrazí tankování, výdaje a termíny. Samostatný export a obnova přímo z deníku zatím nejsou zavedené.
