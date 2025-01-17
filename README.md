# Oppgave: Opprett en Web-App med Database for Klasseliste

#### Del 1: Last ned og installer WAMPserver på Windows
1. **Last ned WAMPserver**:
   - Gå til WAMPserver sin offisielle nettside.
   - Last ned den nyeste versjonen som passer til ditt Windows-operativsystem.

2. **Installer WAMPserver**:
   - Åpne den nedlastede filen og følg installasjonsveiviseren.
   - Velg standardinnstillinger med mindre du har spesifikke behov.
   - Start WAMPserver etter installasjonen.

#### Del 2: Opprett en SQL-database på localhost
1. **Åpne phpMyAdmin**:
   - Klikk på WAMPserver-ikonet i systemstatusfeltet og velg "phpMyAdmin".

2. **Opprett en ny database**:
   - I phpMyAdmin, gå til "Databaser"-fanen.
   - Skriv inn navnet på databasen, for eksempel `klasseliste`, og klikk "Opprett".

3. **Opprett tabeller**:
   - Gå til "SQL"-fanen og kjør følgende kommando for å opprette tabeller:
     ```sql
     CREATE TABLE elever (
         elev_id INT AUTO_INCREMENT PRIMARY KEY,
         navn VARCHAR(100),
         klasse VARCHAR(10)
     );

     CREATE TABLE karakterer (
         karakter_id INT AUTO_INCREMENT PRIMARY KEY,
         elev_id INT,
         fag VARCHAR(50),
         karakter CHAR(1),
         FOREIGN KEY (elev_id) REFERENCES elever(elev_id)
     );
     ```

#### Del 3: Opprett en express.js web-app
1. **Installer Node.js**:
   - Last ned og installer Node.js fra nodejs.org.

2. **Opprett et nytt prosjekt**:
   - Åpne en terminal og kjør følgende kommandoer:
     ```bash
     mkdir klasseliste-app
     cd klasseliste-app
     npm init -y
     npm install express mysql
     ```

3. **Opprett en enkel server**:
   - Lag en fil `app.js` med følgende innhold:
     ```javascript
     const express = require('express');
     const mysql = require('mysql');
     const app = express();

     const db = mysql.createConnection({
         host: 'localhost',
         user: 'root',
         password: '',
         database: 'klasseliste'
     });

     db.connect((err) => {
         if (err) throw err;
         console.log('Connected to database');
     });

     app.get('/', (req, res) => {
         res.send('Hello World!');
     });

     app.listen(3000, () => {
         console.log('Server started on port 3000');
     });
     ```

#### Del 4: Koble web-appen til databasen
1. **Hent data fra databasen**:
   - Oppdater `app.js` for å hente data fra databasen:
     ```javascript
     app.get('/elever', (req, res) => {
         let sql = 'SELECT * FROM elever';
         db.query(sql, (err, results) => {
             if (err) throw err;
             res.json(results);
         });
     });
     ```

#### Del 5: Hente data fra databasen for å vise det på websiden
1. **Vis data på websiden**:
   - Oppdater `app.js` for å vise data i HTML-format:
     ```javascript
     app.get('/elever', (req, res) => {
         let sql = 'SELECT * FROM elever';
         db.query(sql, (err, results) => {
             if (err) throw err;
             let html = '<h1>Klasseliste</h1><ul>';
             results.forEach(elev => {
                 html += `<li>${elev.navn} - ${elev.klasse}</li>`;
             });
             html += '</ul>';
             res.send(html);
         });
     });
     ```

### Valgfrihet underveis
- **Design**: Du kan velge å style websiden med CSS for å gjøre den mer attraktiv.
- **Funksjonalitet**: Du kan legge til funksjonalitet som å legge til nye elever eller oppdatere karakterer.

### Oppgave
**Oppgave**: Lag en funksjon i web-appen som lar brukeren legge til nye elever i databasen via et HTML-skjema. Skjemaet skal inneholde feltene `navn` og `klasse`. Når brukeren sender inn skjemaet, skal den nye eleven legges til i `elever`-tabellen i databasen.

Lykke til! 🚀

Kilde: Samtale med Copilot, 17.1.2025
(1) github.com. https://github.com/z-yong/studyNode/tree/2fe88abdb2239b8304b99118372e7d5b324cf3d1/express_mysql%2Fapp.js.
