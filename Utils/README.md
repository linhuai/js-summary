# utils

### 一、env
```
const inBrowser = typeof window !== 'undefined'
const ua = inBrowser && navigator.toLowserCase()
const inWeChartDevTools = ua && /wechartdevtools/.test(ua)
const inAndroid = ua && ua.indexOf('android')

```
### 二、 debug
```
function log (msg) {
  console.log(msg)
}

function warn (msg) {
  console.error(msg)
}

```

### 三、DOM
```
function addEvent (el, type, fn, capture) {
  el.addEventListener(type, fn, !!capture)
}

function removeEvent (el, type, fn, capture) {
  el.removeEventListener(type, fn, !!capture)
 }
 
function insertBefore(el, target) {
  el.parentNode.insertBefore(el, target)
}

function appendChild(el, target) {
  el.appendChild(target)
}

function prepend(el, target) {
  if (target.firstChild) {
    el.insertBefore(target.firstChild)
  } else {
    el.appendChild(target)
  }
}

function removeChild(el, child) {
  el.removeChild(child)
}
 
```

### 四、 Event
```
// Better-scroll
function BScroll () {
  this._events = {}
}
plugin_name.prototype.on = function (type, fn, context = this) {
  if (!this._events[type]) {
    this._events[type] = []
  }
  this._events[type].push([fn, context])
}
BScroll.prototype.once = function (type, fn, context = this) {
  function magic () {
    this.off(type, magic)
    fn.apply(context, arguments)
  }
  // To expose the corresponding function method in order to execute off method
  magic.fn = fn
  this.on(type, magic)
}
BScroll.prototype.off = function (type, fn) {
  let _events = this._events[type]
  if (!_events) return
  let count = _events.length
  while(count--) {
    if (_events[count][0] === fn || (_events[count][0] && _events[count][0].fn === fn)) {
    _events.splice(count, 1)
  }
}
BScroll.prototype.trigger = function (type) {
  let events = this._evnets[type]
  if (!events) return
  
  let len = events.length
  let eventsCopy = [...events]
  for (let i = o; i < len; i++) {
    let event = eventsCopy[i]
    let [fn, context] = event
    if (fn) {
      fn.apply(context, [].slice.call(arguments, 1)))
    }
  }
}

```
```
// swiperClass
class SwiperClass {
  constructor (params = {}) {
    const self = this
    self.params = params
    
    // Events
    self.eventsListeners = {}
  }
  
  on (events, handler, priority) {
    const self = this
    if (typeof handler !=='function') return self
    const method = priority ? 'unshift' : 'push'
    events.split(' ').forEach(event => {
      if (!self.eventsListeners[event]) self.eventsListeners[event] = []
      self.eventsListeners[event][method](handler)
    }
    return self
  }
  
  once (events, handler, priority) {
    const self = this
    if (typeof handler !== 'function') return self
    function onceHandler (...args) {
      handler.apply(self, args)
      self.off(events, onceHandler)
    }
    return self.on(events, onceHandler, priority)
  }
  
  off (events, handler) {
    const self = this
    if (!self.events[event]) return self
    events.split(' ').forEach(event => {
      if (typeof handler === 'undefined') {
        self.eventsListeners[event] = []
      } else if (self.eventsListeners[event] && self.eventsListeners[event].length) {
        self.eventsListeners[event].forEach((eventHandler, index) => {
          if (eventHandler === handler) {
            self.eventsListeners[event].splice(index, 1)
          }
        }
      }
    }
   
  }
}
```

### 五、Utils
```
function deleteProps(obj) {
  const object = obj
  Object.keys(object).forEach(key => {
    try {
      object[key] = null
    } catch (e) {
      // no getter for object
    }
    try {
      delete object[key]
    } catch (e) {
      // something got wrong
    }
  }
}

function nextTick(callback, delay = 0) {
  return setTimeout(callback, delay)
}

function parseUrlQuery (url) {
  const query = {}
  let urlToParse = url || window.location.href
  let i
  let params
  let param
  let length
  if (typeof urlToParse === 'string' && urlToParse.length) {
    urlToParse = urlToParse.indexOf('?') > -1 ? urlToParse.replace(/\S*/, '') : ''
    params = urlToParse.split('&').filter(paramsPart => paramsPart !== '')
    length = params.length
    
    for(i = 0; i < length; i++) {
      param = params[i].replace(/#\S+/g, '').split('=')
      
      // here need some code
    }
  }

}

function isObject (obj) {
  return typeof o === 'object' && o !== null && o.constructor && o.constructor === Object
}
```
