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

### 时间戳转为时间

```
  timestampToTime: function (timestamp) {
    var date = new Date(timestamp)
    var Y = date.getFullYear()
    var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1)
    var D = date.getDate() < 10 ? '0' + date.getDate() : date.getDate()
    // var h = date.getHours()
    // var m = date.getMinutes()
    // var s = date.getSeconds()
    return Y + '-' + M + '-' + D
  }
```
