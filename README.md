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
2. [Listener](#listener)  

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
  providedIn: 'root',
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
    this.dataService.addDataListener({
      push: (r) => this.pushData(r),
      remove: null,
      clear: null
    });
```  
## Listener
