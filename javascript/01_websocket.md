!SLIDE
# Javascript Websocket API

    @@@ Javascript
    var websocket =
      new WebSocket("ws://ws.goodbits.co");

    webSocket.onopen = function(evt) {
      alert("Connection open ...");
    };

    webSocket.onmessage = function(evt) {
      alert( "Received Message: " + evt.data);
    };

    webSocket.onclose = function(evt) {
      alert("Connection closed.");
    };

!SLIDE
# Javascript WebSocket API

    @@@ Javascript
    webSocket.send("message");
    webSocket.close();