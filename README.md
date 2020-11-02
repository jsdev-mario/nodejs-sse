# nodejs-sse

## Install

```
$ npm install nodejs-sse
```

## How to use

Use with express:

```javascript
var SSE = require('sse-nodejs');

var express = require('express');

var app = express();

app.get('/', (req,res) => {
   res.sendFile(__dirname+ '/index.html');
});

app.get('/time', async(req,res) => {
    var app = SSE(res);

    app.sendEvent('time', function () {
    
        const data = await getData()        
        return data
    },1000);

    app.disconnect( () => {
        console.log("disconnected");
    });

    app.removeEvent('time',3100);

});

app.listen(3000,  () => {
    console.log('Simple SSE server start at port: 3000')
});

```

Use in frontend


```html
<!DOCTYPE html>
<html>
<head></head>
<body>
HELLO WORLD
<div id="clock"></div>
</body>
<script>
    var ev = new EventSource('/time');
    ev.addEventListener('time', function (result) {
        document.getElementById("clock").innerHTML += '<h4>' + result.data + '</h4>'
    })
</script>
</html>
```

## Option

```javascript
    var app = sse(res, options);
```

| Option  | Desciption |
| ------------- | ------------- |
| padding | add padding 2Kb, default true |
| heartbeat | send a "heartbeat" every 10 seconds. https://bugzilla.mozilla.org/show_bug.cgi?id=444328 |
| retry | time to retry, default time 3000 |
| headers | add extra header, default header {"Content-Type": "text/event-stream","Cache-Control": "no-cache", "Connection": "keep-alive"} |


## Function

| Function  | Desciption |
| ------------- | ------------- |
| send(data) | send data only (message on server sent events) |
| sendEvent(name,data,timeInterval) | send data after timeInterval if no timeInterval send once (data can be a function)|
| removeEvent(nameEvent, timeout) | remove an event after timeout, if no timeout it will remove immediately |
| disconnect(function) | listen event 'close' |


## How to use

You can use this module with HTTP response object


### The MIT License (MIT)

Copyright (c) <2020> Jaon Micle
