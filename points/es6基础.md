## 1. 手写 Promise
[https://juejin.im/post/5b88e06451882542d733767a](https://juejin.im/post/5b88e06451882542d733767a)
[https://segmentfault.com/a/1190000012664201](https://segmentfault.com/a/1190000012664201)

### 1. 同步写法
```
function MyPromise(fn) {
  let self = this
  self.status = 'pending'  // pending, fulfilled, rejected
  self.value = ''
  self.reason = ''

  function _resolve(value) {
    if (self.status === 'pending') {
      self.status = 'fulfilled'
      self.value = value
    }
  }

  function _reject(reason) {
    if(self.status === 'pending') {
      self.status = 'rejected'
      self.reason = reason
    }
  }

  fn(_resolve, _reject)
}

MyPromise.prototype.then = function(onfulfilled, onrejected) {
  let self = this
  if (self.status === 'fulfilled') {
    onfulfilled(self.value)
  } else if (self.status === 'rejected') {
    onrejected(self.reason)
  } else {
  }

}


var p = new MyPromise(function(resolve, reject) {
  resolve('123')
}).then(res => {
  console.log(res)
})
```

### 2. 添加异步
```
function MyPromise(fn) {
  let self = this
  self.status = 'pending'  // pending, fulfilled, rejected
  self.value = ''
  self.reason = ''
  self.fulfilledList = []
  self.rejectedList = []

  function _resolve(value) {
    if (self.status === 'pending') {
      self.status = 'fulfilled'
      self.value = value
      let cb
      while(cb = self.fulfilledList.shift()) {
        cb()
      }
    }
  }

  function _reject(reason) {
    if(self.status === 'pending') {
      self.status = 'rejected'
      self.reason = reason
      let cb
      while(cb = self.rejectedList.shift()) {
        cb()
      }
    }
  }

  fn(_resolve, _reject)
}

MyPromise.prototype.then = function(onfulfilled, onrejected) {
  let self = this
  if (self.status === 'fulfilled') {
    onfulfilled(self.value)
  } else if (self.status === 'rejected') {
    onrejected(self.reason)
  } else {
    self.fulfilledList.push(function(){
      onfulfilled(self.value)
    })
    self.rejectedList.push(function() {
      onrejected(self.reason)
    })
  }
}


var p = new MyPromise(function(resolve, reject) {
  setTimeout(function(){
    resolve('123')
  }, 1000)
  // setTimeout(function(){
  //   reject('123')
  // }, 1000)
}).then(res => {
  console.log(res)
}, err => {
  console.log(err)
})
```

### 3. 添加静态resolve,reject和catch
```
function MyPromise(fn) {
  let self = this
  self.status = 'pending'  // pending, fulfilled, rejected
  self.value = ''
  self.reason = ''
  self.fulfilledList = []
  self.rejectedList = []

  function _resolve(value) {
    if (self.status === 'pending') {
      self.status = 'fulfilled'
      self.value = value
      let cb
      while(cb = self.fulfilledList.shift()) {
        cb()
      }
    }
  }

  function _reject(reason) {
    if(self.status === 'pending') {
      self.status = 'rejected'
      self.reason = reason
      let cb
      while(cb = self.rejectedList.shift()) {
        cb()
      }
    }
  }

  fn(_resolve, _reject)
}

MyPromise.prototype.then = function(onfulfilled, onrejected) {
  let self = this
  if (self.status === 'fulfilled') {
    onfulfilled(self.value)
  } else if (self.status === 'rejected') {
    onrejected(self.reason)
  } else {
    self.fulfilledList.push(function(){
      onfulfilled(self.value)
    })
    self.rejectedList.push(function() {
      onrejected(self.reason)
    })
  }
}

MyPromise.prototype.catch = function(fn) {
  return this.then(undefined, fn)
}

MyPromise.resolve = function(value) {
  return new MyPromise(function(resolve, rejected) {
    resolve(value)
  })
}

MyPromise.reject = function(reason) {
  return new MyPromise(function(resolve, reject) {
    reject(reason)
  })
}

// var p = new MyPromise(function(resolve, reject) {
//   setTimeout(function(){
//     resolve('123')
//   }, 1000)
//   // setTimeout(function(){
//   //   reject('123')
//   // }, 1000)
// }).then(res => {
//   console.log(res)
// }, err => {
//   console.log(err)
// })

MyPromise.resolve('123-then').then(res => {
  console.log(res)
})

MyPromise.reject('123-catch').catch(res => {
  console.log(res)
})
```
### 4. 链式调用
```
function MyPromise(fn) {
  let self = this
  self.status = 'pending'  // pending, fulfilled, rejected
  self.value = ''
  self.reason = ''
  self.fulfilledList = []
  self.rejectedList = []

  function _resolve(value) {
    if (self.status === 'pending') {
      self.status = 'fulfilled'
      self.value = value
      let cb
      while(cb = self.fulfilledList.shift()) {
        cb()
      }
    }
  }

  function _reject(reason) {
    if(self.status === 'pending') {
      self.status = 'rejected'
      self.reason = reason
      let cb
      while(cb = self.rejectedList.shift()) {
        cb()
      }
    }
  }

  fn(_resolve, _reject)
}

MyPromise.prototype.then = function(onfulfilled, onrejected) {
  let self = this

  return new MyPromise(function(resolveNext, rejectNext) {

    // 封装一个成功时执行的函数
    function dofulfilled(value) {
      try {
        if (typeof onfulfilled !== 'function') {
          resolveNext(reason)
          return
        }
        let res = onfulfilled(value)
        // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
        if (res instanceof MyPromise) {
          res.then(resolveNext, rejectNext)
        }else {
          resolveNext(res)
        }
      } catch(err) {
        rejectNext(err)
      }
    }

    // 封装一个失败时执行的函数
    function dorejected(reason) {
      try {
        if (typeof onrejected !== 'function') {
          rejectNext(reason)
          return
        }
        let res = onrejected(reason)
        // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
        if (res instanceof MyPromise) {
          res.then(resolveNext, rejectNext)
        }else {
          resolveNext(res)
        }
      } catch(err) {
        rejectNext(err)
      }

    }

    if (self.status === 'fulfilled') {
      dofulfilled(self.value)
    } else if (self.status === 'rejected') {
      dorejected(self.reason)
    } else {
      self.fulfilledList.push(function(){
        dofulfilled(self.value)
      })
      self.rejectedList.push(function() {
        dorejected(self.reason)
      })
    }

  }) 
  
}

MyPromise.prototype.catch = function(fn) {
  return this.then(undefined, fn)
}

MyPromise.resolve = function(value) {
  return new MyPromise(function(resolve, rejected) {
    resolve(value)
  })
}

MyPromise.reject = function(reason) {
  return new MyPromise(function(resolve, reject) {
    reject(reason)
  })
}


new MyPromise(function(resolve, reject) {
  setTimeout(function(){
    resolve('res')
  }, 1000)
}).then(res => {
  console.log('res', res)
  return 'res1'
}).then(res1 => {
  console.log('res1', res1)
}).catch(err => {
  console.log('err', err)
})


// new MyPromise(function(resolve, reject) {
//   setTimeout(function(){
//     reject('res')
//   }, 1000)
// }).then(res => {
//   console.log('res', res)
//   return 'res1'
// }).then(res1 => {
//   console.log('res1', res1)
// }).catch(err => {
//   console.log('err', err)
// })
```
## 2. async await
一个函数如果加上 async ，那么该函数就会返回一个 Promise。 async 就是将函数返回值使用 Promise.resolve() 包裹了下，和 then 中处理返回值一样，并且 await 只能配套 async 使用。