# speedtest-cli
>speedtest-cli ist ein Befehlszeilentool zur Durchführung von Geschwindigkeitstests für Ihre Internetverbindung.
>Mit speedtest-cli können Sie die Download- und Upload-Geschwindigkeiten sowie die Ping-Zeiten zu verschiedenen Servern messen.
>Es handelt sich um eine Befehlszeilenversion des beliebten Speedtest-Tools von Ookla.


```
sudo apt install speedtest-cli
sudo apt install jq
```

`Infos mit: speedtest --h`

```
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
