!SLIDE
# HTML5 WebSockets & Server-Sent Sockets coated in Ruby #

Hello I'm @kalv from @goodbitsco

!SLIDE smbullets
# What you're gonna get

- Overview of Websockets / Server-Sent Events (SSE)
- The Javascript API
- How in Ruby?
- Example app in Ruby deployed to Heroku (yep the cloud)

!SLIDE smbullets
# What's the problem?

- Need fast updates from server to browser
- For things like Messaging/Games/Live Dashboards
- Yeah sure what about AJAX polling?
- Polling of HTTP requests creates un-needed overhead
- Lots of duplicated work

!SLIDE smbullets
# Hello WebSockets

- Persistent connections over HTTP. Goodbye "connecting" overhead.
- Send and receive data. Dubbed "TCP for web"
- Extension of HTTP, upgrades connection to allow full-duplex
- Javascript Websocket API implemented in browsers.
- Being standardized as RFC 6455

!SLIDE smbullets
# Websocket Compatibility

<table class="versions">
  <tr>
    <th>Protocol</th>
    <th>IE</th>
    <th>Firefox</th>
    <th>Chrome</th>
    <th>Safari</th>
    <th>Opera</th>
    <th>iOS</th>
  </tr>
  <tr>
    <td>hixie-76 / hybi-00 (insecure)</td>
    <td>&nbsp;</td>
    <td>4.0 (disabled)</td>
    <td>6</td>
    <td>5.0.1</td>
    <td>11.00 (disabled)</td>
    <td>4.2</td>
  </tr>
  <tr>
    <td>hybi-10</td>
    <td>&nbsp;</td>
    <td>7</td>
    <td>14</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>RFC 6455</td>
    <td>10 PP5</td>
    <td>11</td>
    <td>16</td>
    <td>Nightly</td>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
</table>

[https://en.wikipedia.org/wiki/WebSockets](https://en.wikipedia.org/wiki/WebSockets)

!SLIDE smbullets
# Server-Sent Events (SSE)

- First introduced in Opera back in 2006!
- One way only, send data from Server to browser as **DOM Events**
- Simply a long polling HTTP Request with "text/event-stream" content type
- No need to upgrade HTTP connection
- If you know [Comet]("https://en.wikipedia.org/wiki/Comet_(programming)"), this is an alternative

!SLIDE smbullets
# SSE Compatibility

<table class="versions">
  <tr>
    <th>IE</th>
    <th>Firefox</th>
    <th>Chrome</th>
    <th>Safari</th>
    <th>Opera</th>
    <th>iOS</th>
  </tr>
  <tr>
    <td>No, 10?</td>
    <td>YES, >6</td>
    <td>YES, >6</td>
    <td>YES, >5</td>
    <td>YES, >11</td>
    <td>YES, >4</td>
  </tr>
</table>

[http://caniuse.com/#feat=eventsource](http://caniuse.com/#feat=eventsource)

!SLIDE smbullets
# WebSockets vs SSE

- WebSockets require web servers to understand upgrading of HTTP
- What do you need? Do you need full-duplex communication?
- Dashboards/Notifications = SSE
- Messaging Apps/Games = WebSocket
- imho: Seems easier right now to use SSE