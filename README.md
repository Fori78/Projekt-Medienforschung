Medienanalyse-Projekt (SQL)

Analyse von TV-Reichweite, Zuschauerverhalten und Sendungsperformance
Datenbasis: Fiktive Daten von 4 TV-Sendern (2 öffentlich, 2 privat)
Technologien: MySQL, Prozeduren, Transaktionen, Unterabfragen, Trigger

Projektziel
Analyse des Mediennutzungsverhaltens und der Reichweite verschiedener TV-Formate.
Besonderer Fokus auf:

Wirkung von Wiederholungen
  Zuschauerwechsel zwischen Sendern
  Einfluss von Konkurrenzprogrammen
  Kombination aus TV & Streaming
  Wochentagseffekte

Fragestellungen
  Welche Formate haben die höchste durchschnittliche Reichweite je Sender?
  Wie stark wirkt sich die Wiederholung einer Sendung auf Zuschauerzahlen aus?
  Haben Formate an bestimmten Wochentagen höhere Reichweiten?
  Wie viele Zuschauer wechseln zwischen Sendern (z. B. bei Sendeüberschneidung)?
  Wie wirkt sich Streaming zusätzlich zur TV-Ausstrahlung auf Gesamtreichweite aus?

Datenmodell (Tabellenübersicht)
Tabelle			            Inhalt
broadcasters		        Senderdaten (Name, Typ, Sitz)
formats			            TV-Formate (Name, Genre, Dauer, Senderzuordnung)
tv_airings		          TV-Ausstrahlungen (Datum, Zeit, Zuschauer, Wiederholung?)
streaming_views		      Views in Mediatheken/Online (Datum, Watchtime)
competing_programs	    Konkurrenzprogramme anderer Sender
audience_profiles	      Zielgruppeninformationen (Altersgruppe, Region, Interessen)
low_reach_log		        Logt Reichweitenwarnungen automatisch (durch Trigger)

Wichtige SQL-Techniken im Projekt
Technik			          Umsetzung im Projekt
Prozedur		          add_airing_with_streaming: Neue TV-Ausstrahlung + Streaming automatisch einfügen
Transaktion		        Gleichzeitige Speicherung von TV + Streaming-Eintrag
Unterabfragen		      Zuschauerwechsel bei zeitgleichen Sendungen je Senderpaar
Trigger			          Warnung bei Reichweite < 500.000 (automatischer Log-Eintrag)
Views			            Gespeicherte Sicht: Reichweitevergleiche je Senderpaar
Aggregation		        Durchschnittliche Reichweite je Format / Wochentag

Beispiele
Zuschauerwechsel zwischen Sendern (WDR vs. Pro7)

SELECT 
    a1.air_date, a1.start_time,
    b1.broadcaster_name AS sender1, f1.format_name AS format1, a1.viewers AS viewers1,
    b2.broadcaster_name AS sender2, f2.format_name AS format2, a2.viewers AS viewers2
FROM tv_airings a1
JOIN formats f1 ON a1.format_id = f1.format_id
JOIN broadcasters b1 ON f1.broadcaster_id = b1.broadcaster_id
JOIN tv_airings a2 ON a1.air_date = a2.air_date 
                  AND a1.start_time = a2.start_time 
                  AND a1.airing_id < a2.airing_id
JOIN formats f2 ON a2.format_id = f2.format_id
JOIN broadcasters b2 ON f2.broadcaster_id = b2.broadcaster_id
WHERE (b1.broadcaster_name = 'WDR' AND b2.broadcaster_name = 'Pro7')
   OR (b1.broadcaster_name = 'Pro7' AND b2.broadcaster_name = 'WDR');

Setup & Daten
  SQL-Datei importieren
  Beispiel-Prozeduren & Trigger aktivieren
  Abfragen & Views ausführen

Lizenz
Dieses Projekt steht unter der MIT-Lizenz.

Optional: Für Visualisierung können Tools wie Power BI, Tableau oder Python genutzt werden. 
Hier habe ich zwei Python-Scripte zur Visualisierung der Top-Formate nach Reichweite und der Wochentagseffekte auf Zuschauerzahlen hinzugefügt.
