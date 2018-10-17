# 算法

### 倒计时


```
  function timer (times) {
    function counter () {
      var day = Math.floor(times / (24 * 60 * 60))
      var hour = Math.floor(times % (24 * 60 * 60) / (60 * 60))
      var minute = Math.floor(times % (24 * 60 * 60) % (60 * 60) / 60)
      var second = Math.floor(times % (24 * 60 * 60) % (60 * 60) % 60)
      console.log({
        d: day,
        h: hour,
        m: minute,
        s: second
      })
      return {
        d: day,
        h: hour,
        m: minute,
        s: second
      }
    }
    return function ()  {
      setInterval(() => {
        console.log(times)
        times -= 1
        counter(times)
      }, 1000)
    }
  }
  var daojishi = timer(600000)
  daojishi()
```
