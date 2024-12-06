# Rxjs

* Van pull és push megközelítése (protocol), hogy kommunikál az adat előállítója és fogadója
* pull megközelítés:
  * Az adat fogyasztója/használója(consumer) határozza meg/tudja mikor kér/kap
      adatot az adat szolgáltatójától(producer)
  * a producer nem tudja mikor fogja a consumer megkapni az adatot
  * a javascriptben a függvények így működnek és
  * a függvény a producer a függvényt hívó entitás a consumer
* push megközelítés
  * ilyenkor az adat szolgáltatója(producer) határozza meg mikor küld adatot a fogyasztónak(consumer)
  * a javascriptben a legalapvetőbb ilyenek a promise-ok
  * ide tartoznak az observable-ok is

  <br/>
  <table>
  <thead>
  <th></th>
  <th>Single</th>
  <th>Multiple</th>
  </thead>
  <tr>
  <td><b>Pull</b></td>
  <td>Function</td>
  <td>Iterator</td>
  </tr>
  <tr>
  <td><b>Push</b></td>
  <td>Promise</td>
  <td>Observable</td>
  </tr>
  </table>

  
  <table>
  <thead>
  <th></th>
  <th>Producer</th>
  <th>Consumer</th>
  </thead>
  <tr>
  <td><b>Pull</b></td>
  <td>Passzív: akkor állít elő adatot ha kérik rá</td>
  <td>Aktív: eldönti mikor álljon elő az adat, kéri a producer-től (pl meghív egy függvényt)</td>
  </tr>
  <tr>
  <td><b>Push</b></td>
  <td>Aktív: előállítja az adatot a saját tempójában, és amikor végzett elküldi a consumernek(a consumer nem tud róla hogy ez mikor fog megtörténni)</td>
  <td>Passzív: Várja és a reagál a megkapott adatra</td>
  </tr>
  </table>

## Observables

### Általános információk

* új observable-t a new Observable((subscriber) => {}) hívással tudunk példányosítani
* az átadott függvény törzsében a subscriber.next(value) hívással tudjuk a 'value' értékét kifelé küldeni az observable-ből
* fel lehet íratkozni az observable-re, lényegében figyeljük mikor ad ki adatot magaból, ezt a observable.subscribe() meghívásával tudjuk megtenni
* a subscribe függvény két fajta paraméterrel hívható meg, objektummal vagy függvénnyel
  * ha objectummal hívjuk meg akkor ilyen formája lehet az objectumnak:

    ```javascript
    observable.subscribe({
      next(x) {
        console.log('got value ' + x);
      },
      error(err) {
        console.error('something wrong occurred: ' + err);
      },
      complete() {
        console.log('done');
      },
    });
    ```

  * Ha pedig függvény paraméterrel hívjuk meg, akkor a függvény megkapja paraméterül az observable által átadott értéket
  * Ez lényegében egy rövidített formája az objektumos megadásnak és ilyenkor csak a next-re reagáló függvényt definiáljuk
  
    ```javascript
    import { Observable } from 'rxjs';

    const foo = new Observable((subscriber) => {
      console.log('Hello');
      subscriber.next(42);
    });

    foo.subscribe((x) => {
      console.log(x);
    });
    ```

### Observable életciklusa
  
* 4 állapota van egy observable életciklusának
  * creating (amikor létrehozzuk)
  * subscribing (amikor feliratkozunk rá)
  * executing (amikor végrehajtódik és valamilyen értéket előallít)
  * disposing (amikor leíratkozunk az observable-ről, nem figyeljük tovább, ez egy fontos lépés hogy elkerülhessük az esetleges rossz működést, vagy memória szivárgást)

## Observer
  
* lényegében az az objektum vagy függvény amit a observable.subscribe() függvénynek átadunk paraméterül
* egy consumer, az observable-ből küldött next, error, vagy complete értesítésekre figyel és reagál

  ```javascript
  const observer = {
    next: x => console.log('Observer got a next value: ' + x),
    error: err => console.error('Observer got an error: ' + err),
    complete: () => console.log('Observer got a complete notification'),
  };
  observable.subscribe(observer)
  //vagy hiányozhatnak is függvények az obejektumból
  const observer2 = {
    next: x => console.log('Observer got a next value: ' + x),
    error: err => console.error('Observer got an error: ' + err)
  };
  observable.subscribe(observer2)
  //vagy csak szimplán egy callback function-ként is átadhatjuk, objektum nélkül a subscribe-nak, ekkor a next-et kezeli
  observable.subscribe(x => console.log('Observer got a next value: ' + x));
  ```
