# JavaScript / Tips & Tricks

#### Singleton classes

```js
class Foo {
    // ...
}

class Singleton {
   static instance = null;

   static getInstance() {
      if(!this.instance) {
         this.instance = new Foo();
      }

      return this.instance;
   }
}

const s1 = Singleton.getInstance();
const s2 = Singleton.getInstance();

console.log(s1 === s2); // => true
```