# Duct

Duct is a minimal framework for building web applications in Clojure,
with a strong emphasis on [simplicity][].

[simplicity]: http://www.infoq.com/presentations/Simple-Made-Easy


## Usage

Create a new Duct project with Leiningen.

```sh
lein new duct <<your project name>>
```

This will create a minimal Duct project. You can extend this by
appending profile hints to add extra functionality.

* `+cljs`     adds in ClojureScript compilation and hot-loading
* `+example`  adds an example endpoint
* `+heroku`   adds configuration for deploying to Heroku
* `+postgres` adds a PostgreSQL dependency and database component
* `+ragtime`  adds a Ragtime component to handle database migrations
* `+site`     adds site middleware, a favicon, webjars and more
* `+sqlite`   adds a SQLite dependency and database component

For example:

```sh
lein new duct foobar +site +example
```

As with all Leiningen templates, Duct will create a new directory with
the same name as your project. For information on how to run and build
your project, refer to the project's `README.md` file.


## Concepts

Duct consists of a [Leiningen][] template and a small support library.

Duct depends on existing libraries for the majority of its functionality.

Externally, Duct follows the [Twelve-Factor App][12-factor] methodology.

Internally, Duct uses Stuart Sierra's [Reloaded Workflow][reloaded].

Duct prefers local bindings over global state.

Duct separates configuration and environment.

Duct applications are divided by purpose, rather than layer.

[leiningen]: https://github.com/technomancy/leiningen
[12-factor]: http://12factor.net/
[reloaded]:  http://thinkrelevance.com/blog/2013/06/04/clojure-workflow-reloaded


## Overview

Duct is designed to produce a standalone web application, configured
with environment variables, and logging to STDOUT. Typically it will
sit behind a proxy or load-balancer, and works well in environments
like [Heroku][] and [Docker][].

Internally, Duct projects are structured with the [Component][]
library. **Components** handle the lifecycle of the web server, and
connections to other services and databases. It's highly recommended
you avoid any global state, and even dynamic bindings are discouraged.

Components are grouped into a **system**. In Duct, the system is
created from one or more system definition files, written in [edn][].
These files define the components in the system, the dependencies
between components, and how they are configured.

The routes of the application are divided into **endpoints**. These
are functions that take a component map, and return a [Ring][] handler
function. Duct therefore relies on closures and lexical scoping to
pass database connections and other configuration data to the routes.

Endpoints should resemble microservices, grouping routes by purpose.
An endpoint might handle user authentication, or handle comments on a
post. Strive to keep your endpoints small and focused.

Endpoints should communicate with components via **boundary**
protocols. This draws a line between external services modeled by
components, and the internal functionality modeled by endpoints.
Protocols can be mocked, allowing efficient internal testing.

[heroku]:    https://www.heroku.com/
[docker]:    https://www.docker.com/
[component]: https://github.com/stuartsierra/component
[edn]:       https://github.com/edn-format/edn
[ring]:      https://github.com/ring-clojure/ring


## Documentation

* [Getting Started](https://github.com/weavejester/duct/wiki/Getting-Started)
* [Configuration](https://github.com/weavejester/duct/wiki/Configuration)
* [Compatible Libraries](https://github.com/weavejester/duct/wiki/Compatible-Libraries)


## Community

* [Google Group](https://groups.google.com/forum/#!forum/duct-clojure)


## File structure

Duct projects are structured as below. Files marked with a * are kept
out of version control.

```handlebars
{{project}}
├── README.md
├── dev
│   ├── resources
│   │   ├── dev.edn
│   │   └── local.edn *
│   └── src
│       ├── dev.clj
│       ├── local.clj *
│       └── user.clj
├── profiles.clj *
├── project.clj
├── resources
│   └── {{project}}
│       ├── endpoint
│       │   └── {{endpoint}}
│       ├── errors
│       ├── migrations
│       ├── public
│       └── system.edn
├── src
│   └── {{project}}
│       ├── boundary
│       │   └── {{boundary}}.clj
│       ├── component
│       │   └── {{component}}.clj
│       ├── endpoint
│       │   └── {{endpoint}}.clj
│       └── main.clj
└── test
    └── {{project}}
        ├── boundary
        │   └── {{boundary}}_test.clj
        ├── component
        │   └── {{component}}_test.clj
        └── endpoint
            └── {{endpoint}}_test.clj
```


## License

Copyright © 2016 James Reeves

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
