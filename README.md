# RabbitMQ Websockets Plugin #

This plugin exposes Websockets for RabbitMQ.

The user connects to the host where RabbitMQ is running, using default port `8080`.

The user can use a form to submit to which `exchange` wants to receive messages and can provide an optional `routing_key`.

The plugin will attach a consumer to the provided exchange and will start sending messages to the connected websocket processes.

Depending on the `type` of the exchange will be the behavior seen browser side.

## Installation ##

Get the `rabbitmq-public-umbrella`

    $ hg clone http://hg.rabbitmq.com/rabbitmq-public-umbrella
    $ cd rabbitmq-public-umbrella
    $ make co

Get the [misultin_wrapper](https://github.com/videlalvaro/misultin_wrapper) to support Websockets:

Inside the `rabbitmq-public-umbrella` directory do:

    $ git clone git://github.com/videlalvaro/misultin_wrapper.git

Then clone this repository:

    $ git clone git://github.com/videlalvaro/rabbitmq-websockets.git

Once you have the code you can move into the `rabbitmq-websockets` directory and test the plugin with the broker:

    $ make run-in-broker

If you want to install the plugin in the broker then copy the `*.ez` files inside the `dist` folder to your broker `plugins` folder. Don't copy the `rabbit_common-*.ez` file.

## Usage ##

Point your browser to [http://localhost:8080](http://localhost:8080) and then submit the name of an __existing exchange__. The routing key is optional and depends on how you publish messages to that exchange.

You can publish test messages by calling the following helper function inside the Erlang REPL:

    rabbit_websockets_util:publish_msg(Exchange, Msg, RKey).

All three parameters are binaries.

## Configuration ##

This plugin has two parameters that affect it's behavior:

`misultin_port`: The port Misultin should listen to. Default value is `8080`.

`message_handler`: an Erlang tuple with two atoms like {module, function}. Default value is `{rabbit_websockets_util, basic_handler}`.

You can modify such settings on your `rabbitmq.config` file like this:

    [
      {rabbit, [
        ...
        %% list of RabbitMQ options
        ]},
      {rabbitmq_websockets, [
              {misultin_port, 8081},
              {message_handler, {my_module, my_function}} ]}
    ].

## License ##

See LICENSE.md