die Commandline ist wie ein Werkzeugkeller, in dem hinter dem Schraubenschlüssel plötzlich eine CNC-Fräse steht.
Hier sind ein paar Tools, die ähnlich „tiefsinnig“ sind wie vim, awk, nano, tmux/Termux — also klein wirkend, aber mit enormer Tiefe.
1. tmux — Terminal-Sessions wie ein Cockpit
Falls du tmux noch nicht nutzt: unbedingt anschauen.
Damit kannst du Terminal-Sitzungen offen halten, Fenster teilen, dich per SSH trennen und später wieder verbinden.
sudo apt install tmuxtmux
Wichtige Tasten:
Strg + B, dann %     vertikal teilenStrg + B, dann "     horizontal teilenStrg + B, dann C     neues FensterStrg + B, dann D     Session verlassentmux attach          Session wieder öffnen
Das ist Gold wert auf Servern.

2. mc — Midnight Commander
Der Name passt schon fast verdächtig gut.
sudo apt install mcmc
Zwei-Spalten-Dateimanager im Terminal. Kopieren, Verschieben, SSH/SFTP, Archive öffnen, Dateien bearbeiten. Für viele Serverarbeiten angenehmer als reine cd/ls-Akrobatik.
Tasten:
F3 ansehenF4 bearbeitenF5 kopierenF6 verschiebenF8 löschenF10 beenden

3. fzf — fuzzy finder, der Such-Zauberstab
sudo apt install fzf
Damit kannst du Dateien, History, Prozesse und Ordner interaktiv durchsuchen.
Beispiel:
history | fzf
Oder Dateien auswählen:
nano "$(find . -type f | fzf)"
Richtig mächtig wird es mit Shell-Integration: Strg + R wird dann zu einer komfortablen History-Suche.

4. ripgrep / rg — Suchen wie grep, nur auf Steroiden
sudo apt install ripgrep
Beispiel:
rg "mysql_connect"
Sucht rekursiv im aktuellen Projekt. Schnell, übersichtlich, respektiert .gitignore.
Sehr praktisch für Code:
rg "TODO"rg "function login"rg "class UserController"

5. jq — JSON zerlegen wie ein Skalpell
Wenn du mit APIs, Configs oder Node.js arbeitest: jq ist Pflichtwerkzeug.
sudo apt install jq
Beispiel:
echo '{"name":"Midnight","status":"wach"}' | jq
Ein Feld auslesen:
echo '{"name":"Midnight","status":"wach"}' | jq '.name'
Array filtern:
cat users.json | jq '.users[] | select(.active == true)'
jq ist quasi awk für JSON.

6. yq — wie jq, aber für YAML
Sehr nützlich für Config-Dateien.
sudo apt install yq
Beispiel:
yq '.services' docker-compose.yml
Auch wenn du Docker meidest: YAML taucht überall auf — GitHub Actions, Ansible, Configs, CI/CD.

7. sed — Text umbauen ohne Editor
sed wirkt erst kryptisch, ist aber brutal nützlich.
sed 's/alt/neu/g' datei.txt
Direkt in Datei ersetzen:
sed -i 's/localhost/127.0.0.1/g' config.php
Zeilen anzeigen:
sed -n '10,20p' datei.txt
sed ist wie „Suchen/Ersetzen“ als Maschine.

8. awk — kleine Datenauswertung direkt im Terminal
Kennst du schon, aber vermutlich noch nicht in seiner ganzen Gemeinheit.
awk '{print $1}' datei.txt
Spalten summieren:
awk '{sum += $1} END {print sum}' zahlen.txt
CSV-artig:
awk -F ';' '{print $2}' daten.csv
awk ist eine kleine Programmiersprache, die sich als Textfilter tarnt. Klassischer Undercover-Agent.

9. xargs — Befehle aus Listen bauen
Sehr mächtig zusammen mit find, rg, ls.
find . -name "*.log" | xargs rm
Sicherer mit Leerzeichen:
find . -name "*.log" -print0 | xargs -0 rm
Oder alle gefundenen Dateien in nano öffnen:
rg -l "TODO" | xargs nano

10. watch — Befehl dauerhaft beobachten
watch -n 1 free -h
Oder:
watch -n 2 "df -h"
Oder beim Server:
watch -n 1 "systemctl status apache2 --no-pager"
Das ist simpel, aber unglaublich praktisch.

11. htop / btop — Systemmonitor hübsch und nützlich
sudo apt install htophtop
Noch schöner:
sudo apt install btopbtop
btop sieht aus, als hätte jemand top endlich einen Kaffee gegeben.

12. ncdu — Speicherfresser finden
sudo apt install ncduncdu /
Oder im aktuellen Ordner:
ncdu .
Damit findest du schnell, welcher Ordner die Platte vollmüllt. Sehr nützlich auf Servern.

13. rsync — Kopieren, aber ernst gemeint
rsync -avh quelle/ ziel/
Über SSH:
rsync -avh ./projekt/ user@server:/var/www/projekt/
Mit Fortschritt:
rsync -avh --progress quelle/ ziel/
Trockenlauf:
rsync -avhn quelle/ ziel/
Das n steht für „nur zeigen, nichts machen“. Sehr empfehlenswert, bevor man produktive Daten mit Schwung in die Botanik kopiert.

14. nc / netcat — Netzwerk-Schweizer-Taschenmesser
Port testen:
nc -vz example.com 80
Einfacher Listener:
nc -l -p 1234
Von anderem Terminal senden:
echo "Hallo" | nc localhost 1234
Zum Lernen von Sockets, kleinen Tests und Debugging sehr schön.

15. socat — netcat auf Steroiden
socat kann Verbindungen zwischen fast allem bauen: TCP, UDP, Dateien, serielle Schnittstellen, UNIX-Sockets.
sudo apt install socat
Beispiel TCP-Server:
socat TCP-LISTEN:1234,fork STDOUT
Oder serielle Schnittstelle anschauen:
socat - /dev/ttyUSB0,b115200,raw,echo=0
Für ESP32, Pipes, Blender-Fernsteuerung oder lokale IPC-Spielereien ist das ein richtig spannendes Werkzeug.

16. screen — alter Bruder von tmux
sudo apt install screenscreen
Session verlassen:
Strg + A, dann D
Zurück:
screen -r
Ich würde heute eher tmux nehmen, aber screen ist auf vielen Servern schon vorhanden.

17. entr — Befehl ausführen, wenn Datei sich ändert
Sehr cool beim Entwickeln.
sudo apt install entr
Beispiel: Script neu ausführen, wenn es geändert wird:
ls *.py | entr python3 main.py
Oder PHP-Dateien beobachten:
find . -name "*.php" | entr -c php index.php
Das ist wie ein kleiner, ehrlicher Watcher ohne Framework-Zirkus.

18. taskwarrior — Aufgabenverwaltung im Terminal
sudo apt install taskwarrior
Aufgabe hinzufügen:
task add "Whiptail Menü bauen"
Anzeigen:
task
Erledigen:
task 1 done
Kann Projekte, Tags, Prioritäten, Filter. Sehr tief, wenn man will.

19. ledger / hledger — Buchhaltung als Textdatei
Wenn du irgendwann Einnahmen, Ausgaben, Projekte oder Vereinskram sauber als Klartext verwalten willst:
sudo apt install hledger
Beispiel-Datei:
2026-04-28 Kaffee    Ausgaben:Kaffee        2.50 EUR    Vermögen:Bargeld
Auswerten:
hledger bal
Das Prinzip ist faszinierend: Finanzen als versionierbare Textdatei.

20. make — nicht nur für C-Projekte
make kann dein persönlicher Projektknopf sein.
Makefile:
run:	python3 main.pybackup:	rsync -avh ./ user@server:/backup/projekt/serve:	php -S localhost:8000
Dann:
make runmake backupmake serve
Für Projekte mit vielen Befehlen ist das wunderbar lesbar.

21. just — moderneres make für Befehle
Falls verfügbar:
sudo apt install just
justfile:
run:    node app.jsdev:    npm run devbackup:    rsync -avh ./ server:/backup/
Dann:
just runjust dev
Sehr angenehm für Projekt-Kommandos. Weniger historischer Ballast als make.

22. bat — schöneres cat
Unter Debian/Ubuntu heißt es oft:
sudo apt install bat
Auf manchen Systemen startet es als:
batcat datei.php
Es zeigt Syntax-Highlighting, Zeilennummern und Git-Änderungen.
batcat app.js

23. eza — moderneres ls
sudo apt install eza
Beispiele:
eza -laeza --treeeza -lah --git
Falls nicht verfügbar, ist tree auch schön:
sudo apt install treetree

24. zoxide — intelligenteres cd
sudo apt install zoxide
Danach Shell-Integration aktivieren. Dann kannst du oft sowas machen:
z projekt
und es springt in einen oft genutzten Projektordner, auch wenn du nicht den ganzen Pfad eintippst.

25. lf oder ranger — Terminal-Dateimanager für Tastaturmenschen
sudo apt install rangerranger
Oder:
sudo apt install lflf
ranger ist sehr angenehm zum Navigieren, Vorschauen, Öffnen, Kopieren. mc ist klassischer, ranger fühlt sich mehr wie Vim-Dateinavigation an.

26. gitui / tig — Git im Terminal schöner benutzen
tig:
sudo apt install tigtig
Damit kannst du Git-History durchsuchen, Commits ansehen, Diffs lesen.
gitui ist moderner, aber nicht überall direkt per apt verfügbar.

27. lazygit — Git-TUI mit Komfort
Falls installiert oder aus Release verfügbar:
lazygit
Sehr angenehme Oberfläche für Staging, Commits, Branches, Pull, Push. Gerade wenn Git nervt, aber man trotzdem sauber arbeiten will.

28. journalctl — Logs richtig lesen
Nicht unbekannt, aber oft unterschätzt.
Aktuelle Logs:
journalctl -xe
Für Dienst:
journalctl -u apache2
Live mitlaufen:
journalctl -u apache2 -f
Seit heute:
journalctl --since today
Für Serverdiagnose extrem wichtig.

29. systemctl — Dienste steuern
Auch bekannt, aber tiefer als man denkt:
systemctl status apache2systemctl restart apache2systemctl enable apache2systemctl disable apache2
Alle laufenden Dienste:
systemctl --type=service --state=running
Fehlgeschlagene Dienste:
systemctl --failed

30. dialog — wie whiptail, aber größer
Wenn dir whiptail gefällt:
sudo apt install dialog
Beispiel:
dialog --msgbox "Hallo aus dialog" 10 40
dialog hat sehr viele Widgets: Menüs, Formulare, Kalender, Fortschrittsbalken, Dateiauswahl. Für Bash-Installer und kleine Admin-Tools richtig schön.

Meine persönliche Top-Liste für dich
Für deinen Stil und deine Projekte würde ich dir diese Reihenfolge empfehlen:
1. tmux       Server-Sessions, SSH, dauerhaftes Arbeiten2. mc         komfortables Dateimanagement3. fzf        Such-Zauberstab4. rg         Code/Text schnell durchsuchen5. jq         JSON/API-Daten bearbeiten6. ncdu       Speicherfresser finden7. rsync      Backups und Serverumzüge8. dialog     Terminal-GUIs bauen9. socat      Sockets, ESP32, Pipes, Experimente10. make/just Projektbefehle sauber bündeln
Und wenn du nur eins heute ausprobierst:
sudo apt install tmux mc fzf ripgrep jq ncdu
Danach:
tmuxmcrg "TODO"ncdu .
Das ist ein schöner Einstieg in die „Terminal kann doch mehr als mich anschreien“-Abteilung.
