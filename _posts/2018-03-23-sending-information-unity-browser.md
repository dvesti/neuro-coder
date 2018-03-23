---
layout: single
title: "How to send information from Unity to a web browser - part I."
date: 2018-03-22 16:16:01 +0100
tags: unity node science
categories: gamedev
comments: true
---

When you'are administering expeirments, it is often challenging to keep track of what the participants are doing. You can peak from behind their shoulders, which makes them uneasy, or you can ask them, which ruins your experiment. I often wished, if only there was a way to monitor participants performance on a different screen ;)

Our expeirments are usually FPS games, either in VR or a poor man's desktop versions. So I decided to use some internet magic and push information from the game engine to a web server which displays exactly what is going on :)

## Environment setup

Most of our experiemnts are run in unity, so that is what I am gonna provide my code for. As for the web server part, I went with node and Express, not necessarily because they are the best, but I quite like javascript, I hate php and most importantly, I don't know Express :D So this is gonna be a self initiation road for me as well :)

I setup express as per mozzila instructions [here](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/development_environment) and booted it up. Cool :) We have a listening server.

Next I dabbled in what shoudl I use as an listening protocol - TCP packets? UDP? in all honesty, as much as I want to learn Express, I have no wish in dealing with these sort of traffic protocols. Given I expect only a single stream to be communicating at the same time and Express shoudl be able to handle several hundreds requests per second, I can just do pure RESTful API.

### Express

First we need to start routing all that unity traffic. I am building all in index so far. App.js so far untouched. So let's listen to post requests:

```javascript
//index.js
router.post('/', function(req, res, next){
  console.log(req.body);
  res.send({});
});
```

### Unity

And then we need a controller in Unity that is gonna send that request. I went with simple dictionary of strings that is conveniently convented to javascript object on reception. Because web serving is async, we need enumerator for that unless you want your game to freeze until response is received. Be mindful that this is Unity specific implementation and your classes and ways of instantiating enumerators might differ.

This will basically send a dummy POST with *{"dict-key", "dict-value"}* object to the server. Test it out and see that Express is indeed receiving and returning POST answers.

```c#
    public string url = "http://localhost:8080";

    void Update()
    {
        if(Input.GetKeyDown(KeyCode.F3)) SendInfo(new Dictionary<string, string>{\{"dict-key", "dict-value"\}});
    }

    IEnumerator SendPOST(Dictionary<string, string> dict)
    {
        var form = new WWWForm();
        foreach (var di in dict)
        {
            form.AddField(di.Key, di.Value);
        }
        // Upload to a cgi script
        var w = UnityWebRequest.Post(url, form);
        yield return w.Send();

        if (w.isNetworkError || w.isHttpError) print(w.error);
        else print("Finished POST");
    }

    public void SendInfo(Dictionary<string, string> info)
    {
        StartCorouting(SendPOST(info));
    }
```

## Listening and sockets

We are already listening in node. But there is a problem. Server is listening, but client (your browser), knows nothing about the events that are happening in node. We need a way to push notification and these changes to anybody interested. And we will do that with [socket.io](https://github.com/socketio/socket.io-client). If you don't have it already, you can install sockets using `npm install socket.io`.

Sockets allow us to "subscribe" to events and incomming data and run functions that change DOM. It works on everything but Edge (really, wth Microsoft). We need to slightly change the server settings in app.js and in our view as well. The node server received POST in *index.js* is emited to the client *incomming.js*. We lastly load sockets and incomming.js in the view.

```javascript
//app.js
var app = express();
var server = require('http').Server(app);
io = require('socket.io')(server); //no var initialisation makes sure io is available in controllers as well
server.listen(8080);

//index.js
router.post('/', function(req, res, next){
  console.log(req.body);
  io.emit('unityEvent', req.body);
  res.send({});
});
//incomming.js
var socket = io();
socket.on('unityEvent', function (data) {
  console.log('event received');
  console.log(data);
  document.getElementById('position').innerHTML = data.key;
});

//index.pug
script(src="/socket.io/socket.io.js")
script(src='javascripts/incomming.js')
block content
  h1= title
  p Welcome to #{title}

  div#position
```

With these changes, our work is simply done :) After pressing F3 in Unity you will see the value in DOM div #position change. It is time for you to change what you want to send over - new trials times, position etc. Just beware of overposting! NEVER send these requests in Update. 5 requests in 1 second is fine, 200 might be a bit challenging :)

I'll write a follow-up about displaying player position and other features using javascript *p5*, which is gonna be fun as well. If you have any questions, don't hesitate on leaving them down in the comments.