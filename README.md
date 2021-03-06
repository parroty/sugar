# Sugar

[![Build Status](https://travis-ci.org/sugar-framework/sugar.png?branch=master)](https://travis-ci.org/sugar-framework/sugar)
[![Coverage Status](https://coveralls.io/repos/sugar-framework/sugar/badge.png)](https://coveralls.io/r/sugar-framework/sugar)

Modular web framework for Elixir

> #### Note - 2014-07-18
> 
> Sugar may transition to more of an educational project. Either way, it will remain updated to a certain degree, especially once Elixir v1 hits.
> 
> If you're looking for a project with a more steady backing of support, please use [Phoenix](https://github.com/phoenixframework/phoenix).

## Goals

- Speed. Sugar shouldn't be slow and neither should your project.
- Ease. Sugar should be simple because simple is easy to learn and use.
- Effective. Sugar should aid development. You have better things to which to devote your time.

## Reason for Existence

Why build this when [Dynamo](https://github.com/dynamo/dynamo), [Weber](http://0xax.github.io/weber/), and [Phoenix](https://github.com/phoenixframework/phoenix) exist with growing communities? While both projects are great in their own right, Sugar aims to be another contender, sparking more development on all three projects (and/or others that may follow) and giving developers another option when deciding what framework fits their needs best. Sugar may even strive to shake things up just as [ChicagoBoss](http://www.chicagoboss.org/) has done.

## Getting Started

```
# Clone this repo
git clone https://github.com/sugar-framework/simple.git
cd simple

# Get project dependencies
mix deps.get

# Start the web server
mix server # or `iex -S mix server` if you want access to iex
```

### Configurations

Currently, Sugars expects a `Config` module to be defined in your project that has a `config/0` function defined for configuring your application. A simple `Keyword` list is returned with the configuration values.

Eventually, this will be replaced with a more elegant solution.

Valid options:

- `log` - `true|false` - turns logging on or off
- `server` - contains options to be sent to the underlying web server (currently, only [Cowboy](https://github.com/extend/cowboy))
    - `ip` - the ip to bind the server to.
              Must be a tuple in the format `{ x, y, z, w }`.
    - `port` - the port to run the server.
                Defaults to 4000 (http) and 4040 (https).
    - `acceptors` - the number of acceptors for the listener.
                     Defaults to 100.
    - `max_connections` - max number of connections supported.
                           Defaults to :infinity.
    - `dispatch` - manually configure Cowboy's dispatch.
    - `ref` - the reference name to be used.
               Defaults to `plug.HTTP` (http) and `plug.HTTPS` (https).
               This is the value that needs to be given on shutdown.

#### Example

```elixir
defmodule Config do
  def config do
    [
      log: true,
      server: [
        port: 4000
      ]
    ]
  end
end
```

### Routers

Because Sugar builds upon [Plug](https://github.com/elixir-lang/plug), it leverages `Plug.Router` to do the heavy lifting in routing your application, adding an alternate DSL.

Routes are defined with the form:

    method route [guard], controller, action

`method` is `get`, `post`, `put`, `patch`, `delete`, or `options`, each responsible for a single HTTP method. `method` can also be `any`, which will match on all HTTP methods. `controller` is any valid Elixir module name, and `action` is any valid function defined in the `controller` module.

#### Example

```elixir
defmodule Router do
  use Sugar.Router

  get "/", Hello, :index
  get "/pages/:id", Hello, :show
  post "/pages", Hello, :create
  put "/pages/:id" when id == 1, Hello, :show
end
```

### Controllers

All controller actions should have an arrity of 2, with the first argument being a `Plug.Conn` representing the current connection and the second argument being a `Keyword` list of any parameters captured in the route path.

Sugar bundles these response helpers to assist in sending a response:

- `render/2` - sends a normal response
- `not_found/1` - sends a 404 (Not found) response

#### Example

```elixir
defmodule Hello do
  use Sugar.Controller

  def index(conn, []) do
    render conn, "index.html.eex", []
  end

  def show(conn, args) do
    render conn, "show.html.eex", args
  end

  def create(conn, []) do
    render conn, "create.html.eex", []
  end
end
```

## Todo Items

- basic authentication (?)
- Development error page
- Caching
    - Compiled HTML
    - Application Data
    - Adapters
        - ETS

## Who's behind this?

Why, the [contributors](https://github.com/sugar-framework/sugar/graphs/contributors), of course! Would you consider being one? Please send a pull request :)

## How to start contributing

The main product of this repository is the example terms in the file [CONTRIBUTING.md](https://github.com/sugar-framework/sugar/blob/master/CONTRIBUTING.md). This project uses those guidelines as the basis for its own development process. Please refer to that file.

## License

Sugar is released under the MIT License.

See [LICENSE](https://github.com/sugar-framework/sugar/blob/master/LICENSE) for details.
