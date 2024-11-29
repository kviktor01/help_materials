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

## Observables


