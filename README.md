# Protokoll FIVU/Winter Thomas 5AHME (2019/20)
* **Thema:** Installation Services, Listener
* **Datum:** 5.12.2019
* **Gefehlt:** Sarah Vezonik
* **Erstellt von:** Winter Thomas
* **Protokoll letzte Einheit:** Sarah Vezonik
* **Protokoll nächste Einheit:** Lorenz Breitenthaler  

----------------------------------------------------------------------------------------------

## Inhaltsverzeichnis
1. [Services](#services)  
    1. [Warum Services](#warum-services)  
    2. [Erzeugung](#erzeugung)  
    3. [Verwendung](#verwendung)  
2. [Interface](#interface)  
3. [Listener Pattern](#listener-pattern)  

## Services  
### Warum Sevices 
Das Holen und Speichern soll nicht dierekt von einer Komponente übernommen werden, da dieße zum Anzeigen der Daten vorgesehen sind und einem Service anordnen was mit den Daten geschieht. Somit werden services Verwendet um die Aufgabe des Holens und des Speicherns von Daten zu übernehmen.  

### Erzeugung  
Services können sehr einfach mit einem Angular CLI Befhl erzeugt werden.  
```ng generate service <name-vom-Service>  ```
Mit diesem Befehl wird eine Skelettklasse erzeugt die wie folgt aussieht:  
```javascript  
import { Injectable } from '@angular/core';  

@Injectable({
  providedIn: 'root',   //Service ist in allen Komponenten verwendbar
})
export class DataService {

  constructor() { }

}
```
### Verwendung
Um diesen Service dan in der gewünschten Komponente zu verwenden, wird diese wie folgt eingebunden:  
Zuerst der import Befehl:  
```javascript 
    import { DataService } from 'src/app/services/data.service';
```
Danach wird der Constructor erstellt:
```javascript  
   constructor(private dataService: DataService) {}
```  
## Interface  
Interfaces beinhalten zusammengehörige Datenelemente und Methoden um sie dann in einer Klasse zu implementieren. Sie können auch als Datentyp verwendung finden. Interfaces werden nur zur Programmentwicklung in TypeScript verwendet. Im fertigen JavaScript Programm wird nicht mit Interfaces gearbeitet.  
Im folgenden beispiel wird ein Interface angelegt um einen Datensatz zusammen zu fassen:  

```typescript
export interface IDataRecord {
  time: Date;
  temp: number;
  humidity: number;
}
```
Das Schlüsselwort ```export``` wird verwendet um das Interface in anderen Klassen zu verwenden falls es benötigt wird.  

## Listener Pattern  
Das Listener pattern wird mit hilfe eines Interfaces realisiert indem 3 Methoden zusammengefasst sind:  
``` typescript  
interface IDataListener {
  push: (record: IDataRecord) => void;
  remove: (index: number) => void;
  clear: () => void;
}
```  
Die Methode ```push``` wird später neue werte an die Tabelle übergeben.  
Die Methode ```remove``` wird einen einzelnen Datensatz aus der Tabelle entfernen.  
Die Methode ```clear``` wird alle Daten aus der Tabelle entfernen.  







