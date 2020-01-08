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
4. [Programm](#programm)

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
## Programm
#### data.service.ts  
```typescript
import { Injectable } from '@angular/core';

export interface IDataRecord {
  time: Date;
  temp: number;
  humidity: number;
}

interface IDataListener {
  push: (record: IDataRecord) => void;
  remove: (index: number) => void;
  clear: () => void;
}

@Injectable({
  providedIn: 'root'
})
export class DataService {

  private records: IDataRecord [] = [];
  private listeners: IDataListener [] = [];

  constructor() {

      setTimeout(() => {
        this.add({time: new Date(), temp: 20.12, humidity: 33.2});
        this.add({time: new Date(), temp: 24.21, humidity: 66.6});
        this.add({time: new Date(), temp: 22.42, humidity: 53.7});
      }, 2000);
    }

  public addDataListener(l: IDataListener) {
    this.listeners.push(l);
  }

  public removeDataListener(l: IDataListener) {
    const i = this.listeners.findIndex( (item) => item === l ); // sucht das zu entfernende item, in der Liste
    this.listeners.splice(i, 1); // entfernt das Item
  }

  public add(d: IDataRecord) {
    this.records.push(d);
    for (const l of this.listeners) {
      try {
        l.push(d);
      } catch (error) {
        console.log('err');
      }
    }
  }
  public getCount(): number {
    return this.records.length;
  }

  public getValue(index: number): IDataRecord {
    return this.records[index];
  }
}

```
#### temp-table.component.ts  
```typescript
import { Component, OnInit } from '@angular/core';
import { DataService, IDataRecord } from 'src/app/services/data.service';

interface ITableRecord {
    row: number;
    date: string;
    time: string;
    temp: string;
    humidity: string;
}

@Component({
  selector: 'app-temp-table',
  templateUrl: './temp-table.component.html',
  styleUrls: ['./temp-table.component.css']
})
export class TempTableComponent implements OnInit {

  public COLNAMES = [
    '#',
    'Datum',
    'Zeit',
    'Temperatur / °C',
    'Feuchtigkeit / %'
  ];

  public records: ITableRecord [] = [];

  constructor(private dataService: DataService) {
    this.dataService.addDataListener({
      push: (r) => this.pushData(r),
      remove: null,
      clear: null
    });

    this.records.push({row: 1, date: '2019 12 05', time: '10:05:02', temp: '23.5', humidity: '45'});
    this.records.push({row: 2, date: '2019 12 05', time: '11:05:02', temp: '24.2', humidity: '35'});
  }

  private pushData(r: IDataRecord) {
    const rt: ITableRecord = {
      row: this.records.length + 1,
      data: r.time.toLocaleDateString,
      time: r.time.toLocaleTimeString,
      temp: '' + r.temp,
      humidity: '' + r.humidity
    };
    this.records.push(rt);
  }

  ngOnInit() {
  }

}

```







