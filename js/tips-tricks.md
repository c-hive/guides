# JavaScript / Tips & Tricks

#### Practical Example of Writing Singleton Classes

```js
import ElectronStore from "electron-store";

export default class SingletonStore {
   static instance = null;

   static getInstance() {
      if(!this.instance) {
         this.instance = new ElectronStore();
      }

      return this.instance;
   }
}
```