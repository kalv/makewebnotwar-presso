!SLIDE
# So how to the server stuff in Ruby?

We need to support long running requests on a single ruby process and support multiple connections...

!SLIDE smbullets
# EventMachine WebSocket

[https://github.com/igrigorik/em-websocket](https://github.com/igrigorik/em-websocket)

- Dedicated web server that will handle upgrading HTTP connection
- EventMachine creates a reactor loop allowing multiple connections on one process

!SLIDE
# EventMachine WebSocket

    @@@ Ruby SmallCode
    require 'em-websocket'

    EventMachine.run {

      EventMachine::WebSocket.start(:host => "0.0.0.0", :port => 8080) do |ws|
        ws.onopen {
          puts "WebSocket connection open"
          ws.send "Hello Client"
        }

        ws.onclose { puts "Connection closed" }

        ws.onmessage { |msg|
          puts "Recieved message: #{msg}"
          ws.send "Pong: #{msg}"
        }
      end
    }

!SLIDE smbullets
# Pusher.com

- Pusher is built on em-websocket
- Pusher.com is a hosted API for quickly adding the speed of WebSockets to your app

!SLIDE smbullets
# Cramp

[http://cramp.in/](http://cramp.in/)

- Asynchronous real-time web application framework
- Built on top of EventMachine
- Built-in support for **WebSockets** and **Server-Sent Events**
- Has to be deployed on specific web servers (Rainbows! or Thin)

!SLIDE
# Cramp

    @@@ Ruby SmallCode
    class ChatAction < Cramp::Websocket
      on_start :user_connected
      on_finish :user_left
      on_data :message_received

      def user_connected
        @@users << self
      end

      def user_left
        @@users.delete self
      end

      def message_received(data)
        puts "connected users: #{@@users.size.inspect}"
        @@users.each {|u| u.render data }
      end
    end

!SLIDE
# Sinatra & SSE

Sinatra is a popular lightweight DSL for knocking up quick web app

    @@@ Ruby SmallCode
    # myapp.rb
    require 'sinatra'

    get '/' do
      'Hello world!'
    end

Then

    @@@ Shell SmallCode
    $ gem install sinatra
    $ ruby -rubygems myapp.rb

!SLIDE
# Sinatra & SSE

    @@@ Ruby SmallCode
    get "/chat" do
      content_type 'text/event-stream'
      stream(:keep_open) { |out|
        connections << out
      }
    end

    post '/message' do
      # write to all open streams
      connections.each {|out|
        out << "event:message\ndata:#{params[:message]}\n\n"
      }
      "message sent"
    end

!SLIDE
# Heroku deployment

Quick building and deploying to the cloud

- Heroku's network stack does not allow WebSockets (yet)
- Does support SSE, let me show you