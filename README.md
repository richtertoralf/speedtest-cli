# speedtest-cli
>speedtest-cli ist ein Befehlszeilentool zur Durchführung von Geschwindigkeitstests für Ihre Internetverbindung.
>Mit speedtest-cli können Sie die Download- und Upload-Geschwindigkeiten sowie die Ping-Zeiten zu verschiedenen Servern messen.
>Es handelt sich um eine Befehlszeilenversion des beliebten Speedtest-Tools von Ookla.


```
sudo apt install speedtest-cli
sudo apt install jq
```

`Infos mit: speedtest --h`

# json
```
json
# probier mal das aus:
speedtest --json | jq
```
```
# Die Ausgabe von speedtest in der Variable result speichern
result=$(speedtest --json)

# Die Werte direkt in ein assoziatives Array parsen
declare -A values
values[ping]=$(echo "$result" | jq -r '.ping')
values[upload]=$(echo "$result" | jq -r '.upload' | awk '{printf "%.2f", $0}')
values[download]=$(echo "$result" | jq -r '.download' | awk '{printf "%.2f", $0}')
# In diesem Beispiel verwende ich awk, um die Dezimalstellen auf zwei Stellen nach dem Komma zu runden,
# bevor sie in das assoziative Array values gespeichert werden. 

# Alle Werte des assoziativen Arrays anzeigen
declare -p values

# Die Werte aus dem assoziativen Array einzeln abrufen und anzeigen
echo "Ping: ${values[ping]} ms"
echo "Upload: ${values[upload]} Mbps"
echo "Download: ${values[download]} Mbps"
```
# csv
```
speedtest --csv-header
# Ausgabe:
Server ID,Sponsor,Server Name,Timestamp,Distance,Ping,Download,Upload,Share,IP Address

speedtest --csv
# Ausgabe:
55374,Wifiweb s.r.l.,Bassano del Grappa,2023-09-03T21:15:29.280359Z,636.8788953521096,58.922,44309018.861549765,6960602.171301725,,49.12.204.192

```
```
# CSV-Header von speedtest in einer Variable speichern
csv_header=$(speedtest --csv-header)

# CSV-Ausgabe von speedtest in einer Variable speichern
csv_output=$(speedtest --csv)

# Ein assoziatives Array erstellen
declare -A values

# Die Namen der Wertepaare aus der Header-Zeile extrahieren und in ein Array setzen
IFS=',' read -ra headers <<< "$csv_header"

# Die Werte aus der CSV-Ausgabe extrahieren und in das assoziative Array setzen
IFS=',' read -ra data <<< "$csv_output"

# Die Werte mit den zugehörigen Header-Namen in das assoziative Array setzen
for ((i = 0; i < ${#headers[@]}; i++)); do
  values["${headers[$i]}"]="${data[$i]}"
done

# Die Werte im assoziativen Array anzeigen, z.B.:
echo "Ping: ${values[Ping]} ms"
echo "Upload: ${values[Upload]} Mbps"
echo "Download: ${values[Download]} Mbps"
```
