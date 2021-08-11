# HowTo – Wie schreibe ich ein LaTeX-Dokument in zehn Schritten?

## Motivation
Die folgenden zehn Schritte sollen dabei helfen, möglichst schnell ein LaTeX-Dokument für den Konferenzband "Recht und Religion" zu erstellen.
Es ist explizit *keine* Einführung in LaTeX allgemein.

Ferner hat dieses HowTo nicht den Anspruch vollständig zu sein. Bitte schauen Sie auch immer in das Beispieldokument. Ergänzungswünsche und Fragen können gerne an martin.sievers@schoenerpublizieren.de geschickt werden.

## Die Regeln
1. Grundstruktur beachten. Das Dokument befindet sich in einer `.tex`-Datei. Diese ist wie folgt aufgebaut:
```latex
\documentclass[inproceedings,ngerman]{kiste}
... % Das ist der Vorspann (Layoutdefinition)
\addbibresource{LitVZ.bib}% lädt Datensammlung für Literaturverzeichnis
\addbibresource{Gerichtentscheidungen.bib}% lädt Datensammlung für Literaturverzeichnis
\begin{document}
...% Das ist das eigentliche Dokument (Text und Struktur)
\end{document}
```

Alle Quellen sollten systematisch erfasst werden, z.B. in `Citavi` und als `.bib`-Datei im biblatex-Format vorliegen. Jeder Eintrag erhält dadurch einen eindeutigen Identifier (ID), der für die konkreten Verweise im Text genutzt wird. Im folgenden Beispiel sind dies `KarstenHoof.2013` bzw. `Maurer.2010`:
```latex
@incollection{KarstenHoof.2013,
   abstract  = {Zusammenfassung des Eilrechtsschutzes beim BVerfG, dieses betrachtet nicht die Erfolgsaussichten in der Hauptsache, Autor argumentiert dagegen},
   author    = {Hoof, Karsten},
   title     = {Eilrechtsschutz durch das Bundesverfassungsgericht},
   pages     = {81-91},
   editor    = {Fadeev, Vladimir Ivanovi{\v{c}} and Schulze, Carola},
   booktitle = {Verfassungsgerichtsbarkeit in der Russischen Föderation und in der Bundesrepublik Deutschland},
   year      = {2013},
   address   = {Potsdam},
   file      = {https://publishup.uni-potsdam.de/opus4-ubp/frontdoor/deliver/index/docId/6596/file/proceedings_schulze_S81_91.pdf}
}

@book{Maurer.2010,
   author  = {Maurer, Hartmut},
   year    = {2010},
   title   = {Staatsrecht I: Grundlagen Verfassungsorgane Staatsfunktionen},
   address = {München},
   edition = {6},
}
```

Ein Literaturverzeichnis kann mit `\printbibliography[nottype=jurisdiction]` erstellt werden. Dazu wird das Hilfsprogramm `biber` genutzt, das aus Overleaf heraus automatisch gestartet werden sollte.

Das Prinzip der systemmatischen Erfassung gilt auch für Gerichtsentscheidungen. SIe können am einfachsten in einer zweiten `.bib`-Datei erfasst werden, z.B. `Gerichtsentscheidungen.bib` (siehe oben). Die Einträge nutzen spezielle Felder definiert:
```latex
@jurisdiction{BVerwG_1989,
   gericht = {BVerwG},
   dokumententyp = {Beschluss},
   entscheidungsdatum = {1989-12-14},
   aktenzeichen = {2 ER 301/89},
   datenbank = {juris},
}

@jurisdiction{BVerfG_1986,
   gericht = {BVerfG},
   dokumententyp = {Einstweilige Anordnung},
   entscheidungsdatum = {1986-01-03},
   aktenzeichen = {1 BvQ 12/85},
   fundstelle = {BVerfGE 71, 350},
   options = {citedbypage},
}
```
Die Liste der zitierten Gerichtsentscheidungen kann mit `\printjurisdiction` ausgegeben werden.

2. Text immer "ganz normal" eingeben, d.h. Umlaute z.B. als ä, ö, ü, ß. Faustregel: Alle Zeichen der Tastatur können direkt genutzt werden.
3. LaTeX-Sonderzeichen wie %, $, & oder \ maskieren mit Backslash, also z.B. `\%` für % oder `\&` für &
4. Besondere Zeichen beachten:
   - Anführungszeichen über `\enquote{Hier steht etwas, das in Anführungszeichen erscheinen soll}` ==> korrekte, sprachabhängige Anführungszeichen
   - Geschützte Leerzeichen `~` nutzen, wo nötig, z.B. `S.~17`
   - Strecken und Gedankenstriche als `--` (Verdopplung des Bindestrichs), z.B. `1957--1959`
   - Paragraphenzeichen als `§` oder `\S`, doppeltes §§ als `\SSS`
   - Auslassungszeichen ... als `\textellipsis{}`, in Kombination mit eckigen Klammern [...] als `\textelp{}`
   - 
5. Absätze stets mit Leerzeile im Quelltext voneinder trennen; *keinen* Zeilenumbruch `\\` dafür verwenden!
6. Gliederungsebenen `\section`, `\subsection`, `\subsubsection` zur Strukturierung verwenden: `\section{Diskussion}`
7. Querverweise verwenden. Dazu mit `\label{Initialen:Typ:Name}` eine eindeutige Marke setzen und auf diese mit `\ref{}` verweisen, z.B.:
   ``` latex
   \section{Wichtiger Abschnitt}\label{ms:sec:wichtigerAbschnitt}
   ...
   \section{Diskussion}\label{ms:sec:diskussion}
   Der Abschnitt~\ref{ms:sec:wichtigerAbschnitt} beschreibt ...
   ```
   Anmerkung: Es kann auch eine andere Systematik zur Benennung gewählt, sie sollte aber eindeutig sein (innerhalb des Dokuments, aber auch artikelübergreifend)

8. Besondere Strukturen:
   - Titelseite:
   ```latex
   \title{Titel des Beitrags}
   \author{Autor/in des Betrags}
   \maketitle
   ```
   - Abstract mit 
   ```latex
   \begin{abstract}
   TEXT
   \end{abstract}
   ```
   - Nummerierte Listen mit `\begin{enumerate}...\end{enumerate}`
   - Unnummierte Liste als `\begin{itemize}...\end{itemize}`
   ```latex
   \begin{itemize}
   \item Punkt 1
   \item Punkt 2
   ...
   \end{itemize}
   ```
9.  Fußnoten
    - ohne Literaturverweis: Fußnotentext als Argument zu `\footnote{...}`
    - mit Literaturverweis: Nachweis mit Hilfe der eindeutigen BibTeX-ID aus der eigenen Datenbank als Argument zu `\footcite{...}`; Angabe der Belegstelle als optionales Argument.
    - Mehrere Quellen können mit dem `\footcites`-Makro in einem Rutsch angeben werden. Seitenzahlen können *ohne* `S.~` angegeben werden; für Seitenbereiche reicht der Bindestrich; Folgeseiten nicht als `f.`, sondern mit `\psq` angeben. So stimmt auch die Zeichensetzung immer:
      ```latex
      Ein Text\footnote{Wirklich nicht viel.} und noch mehr Text
      ...
      \enquote{Das hier ist ein tolles Zitat.}\footcite[17]{ABC.2020}
      ...
      Ein paar Ausführungen, die ich aus existierender Literatur übernommen habe.\footcites[Vgl. dazu][17-20]{ABC.2020}[65\psq]{DEF.2019}
      ```
    - Alternaiv kann man auch innerhalb von `\footnote{...}` das Makro `\cite` bzw. `\cites` verwenden (analoge Argumente wie `\footcite` bzw. `\footcites`)  
10. Möglichst wenig (am besten gar nicht) manuell in das Layout eingreifen:
   - keine manuellen Abstände
   - keine manuellen Schriftarten oder -größenänderungen
   - eigene Trennungen nur für falsche oder ungünstige Trennungen; entweder lokal im Text: `Ur\-instinkt` oder im Vorspann:
  
      ```latex
      ...
      \hyphenation{
         Ur-instinkt Donau-schiff-fahrts-ka-pi-tän
      }
      \begin{document}
      ...
      ```  
      Ist i.d.R. nur sehr selten nötig.