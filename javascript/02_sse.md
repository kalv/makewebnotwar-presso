!SLIDE
# Server-Sent Events API

    @@@ Javascript
    var source = new EventSource('/stream');
    source.onmessage = function (event) {
      alert(event.data);
    };


!SLIDE
# Response from /stream
On server side respond with "text/event-stream" MIME Type

    data: This is a first message

    data: This is a second message
    data: which has two lines in it

!SLIDE
# Handling specific events

Send 'event' name before data

    event: newEntry
    data: entry_id_12345

    event: userFollowed
    data: user_id_1234

!SLIDE
# Handling specific events

Set up event listeners to handle types of events

    @@@ Javascript
    var source = new EventSource('/stream');
    source.addEventListener(
      'newEntry'
      , newEntryHandler
      , false
    );
    source.addEventListener(
      'userFollowed'
      , addFollowHandler
      , false
    );