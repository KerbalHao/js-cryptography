对象/类的复杂计算的属性的缓存方式

```js
  const obj = {
    get data() {
      const actualData = someExpensiveComputation()

      Object.defineProperty(this, 'data', {
        value: actualData,
        configurable: false,
        writable: false,
        enumerable: false
      })

      return actualData
    }
  }

  // ----------------------------------
  class MyObj{
    constructor() {
      Object.defineProperty(this, data, {
        get() {
          const actualData = someExpensiveComputation()

          Object.defineProperty(this, 'data', {
            value: actualData,
            configurable: false,
            writable: false,
          })

          return actualData
        },
        configurable:true,
        enumerable:true
      })
    }
  }
```