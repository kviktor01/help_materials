# Promises
* egy aszinkron művelet későbbi befejeződését jelzi
* lényegében egy ígéret(promise) hogy egy művelet végre fog hajtódni valamikor.
Ez a végrehajtódás lehet sikeres, vagy sikertelen
* három féle státusza lehet egy promise-nak: pending, fulfilled, rejected
  * pending: alapállapota a promise-nak
  * fulfilled: a promise-ban lévő utasítás helyesen végrehajtódott
  * rejected: a promise-ban lévő utásítás nem hajtódott végre sikeresen

## Promise.all()
* promise-ok tömbjét várja paraméterként és egy promise-al tér vissza
* akkor tér vissza ha az összes promise végetért
* ha bármelyik promise rejected, akkor a promise all is rejected
* példa:
```javascript
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
//Expected output: Array [3, 42, "foo"] 
```

## Promise.allSettled()
* promise-ok tömbjét várja paraméterként és egy promise-al tér vissza
* akkor lesz fulfilled ha mindegyik promise vagy rejected, vagy fulfilled, 
tehát az összes promise végrehajtása befejeződött
* példa:
```javascript
Promise.allSettled([
  Promise.resolve(33),
  new Promise((resolve) => setTimeout(() => resolve(66), 0)),
  99,
  Promise.reject(new Error("an error")),
]).then((values) => console.log(values));

// [
//   { status: 'fulfilled', value: 33 },
//   { status: 'fulfilled', value: 66 },
//   { status: 'fulfilled', value: 99 },
//   { status: 'rejected', reason: Error: an error }
// ]
```

## Promise.any()
* promise-ok tömbjét várja paraméterként és egy promise-al tér vissza
* a visszaadott promise fulfilled ha bármelyik promise fulfilled státusszal tér vissza
* akkor lesz rejected ha az összes promise rejected. 
Ekkor egy tömbbel tér vissza ami tartalmazza a hibaüzeneteket amiket a promise-ok dobtak (rejection reason tömb) 
* rejected ha üres inputot adunk át neki
* példa:
```javascript
const pErr = new Promise((resolve, reject) => {
  reject("Always fails");
});

const pSlow = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, "Done eventually");
});

const pFast = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "Done quick");
});

Promise.any([pErr, pSlow, pFast]).then((value) => {
  console.log(value);
  // pFast fulfills first
});
// Logs:
// Done quick
```

## Promise.race()
* promise-ok tömbjét várja paraméterként és egy promise-al tér vissza
* a promise státusza a legelőször befejeződő promise státusza lesz
* a promise futása befejeződik amikor a legelső promise befejeződik
* példa
```javascript
// Passing an array of promises that are already resolved,
// to trigger Promise.race as soon as possible
const resolvedPromisesArray = [Promise.resolve(33), Promise.resolve(44)];

const p = Promise.race(resolvedPromisesArray);
// Immediately logging the value of p
console.log(p);

// Using setTimeout, we can execute code after the stack is empty
setTimeout(() => {
  console.log("the stack is now empty");
  console.log(p);
});

// Logs, in order:
// Promise { <state>: "pending" }
// the stack is now empty
// Promise { <state>: "fulfilled", <value>: 33 }
```

## Promise.reject()
* egy rejected státusszal viszatérő promise-t ad vissza
* paraméterként a hiba okát lehet megadni (rejection reason)
* példa:
```javascript
Promise.reject(new Error("fail")).then(
  () => {
    // not called
  },
  (error) => {
    console.error(error); // Stacktrace
  },
);
```

## Promise.resolve()

## Promise.try()
* egy függvényt és annak parmétereit várja paraméterként (Promise.try(func, arg1,arg2 ...))
* lényegében egy promise-ba teszi a függvény végrehajtását
* példa:
```javascript
function doSomething(action) {
  return Promise.try(action)
    .then((result) => console.log(result))
    .catch((error) => console.error(error))
    .finally(() => console.log("Done"));
}

doSomething(() => "Sync result");

doSomething(() => {
  throw new Error("Sync error");
});

doSomething(async () => "Async result");

doSomething(async () => {
  throw new Error("Async error");
});
```

## Promise.withResolvers()

## Promise.prototype.then()
* két függvényt vár parméterként
* az első függvény akkor hívódik meg ha a promise fulfilled státusszal fejeződik be
* a második akkor ha rejected lesz a státusza a promise-nak
* a második egy opcionális függvény
* egy promise-t ad vissza ami tovább chainelhető
* példa:
```javascript
const p1 = new Promise((resolve, reject) => {
  resolve("Success!");
  // or
  // reject(new Error("Error!"));
});

p1.then(
  (value) => {
    console.log(value); // Success!
  },
  (reason) => {
    console.error(reason); // Error!
  },
);
```

## Promise.prototype.catch()
* akkor hívódik meg ha a promise rejected státusszal tér vissza
* ez egy rövidítés a then(undefined, onRejected) hívásra
* egy függvényt vár paraméterként, 
és ez a függvény megkapja paraméterként a rejected státusz okát
* egy promise.al tér vissza így ez tovább chainelhető
* példa:
```javascript
const p1 = new Promise((resolve, reject) => {
  throw new Error("Uh-oh!");
});

p1.catch((e) => {
  console.error(e); // "Uh-oh!"
});
```

## Promise.prototype.finally()
* egy függvényt vár paraméterül
* ez a promise státuszától függetlenül végrehajtódó kódrészlet
* egy promise-al tér vissza így tovább chainelhető
* példa:
```javascript
let isLoading = true;

fetch(myRequest)
  .then((response) => {
    const contentType = response.headers.get("content-type");
    if (contentType && contentType.includes("application/json")) {
      return response.json();
    }
    throw new TypeError("Oops, we haven't got JSON!");
  })
  .then((json) => {
    /* process your JSON further */
  })
  .catch((error) => {
    console.error(error); // this line can also throw, e.g. when console = {}
  })
  .finally(() => {
    isLoading = false;
  });
```
*Forrás: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise*
