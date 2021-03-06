# Promtimer

[Mortimer](https://github.com/couchbaselabs/mortimer) is a convenient 
tool that can be used to display statistics grabbed from Couchbase collected 
log bundles (cbcollects). With the move to use Prometheus for stats
storage and management, Mortimer will no longer work. It would be possible
to extract stats from cbcollects and adapt them to work with Mortimer, but
this project attempts somthing different: use Prometheus itself and Grafana
to allow easy, powerful browsing of Prometheus metrics (stats) available in 
cbcollects.

If you've already installed, here are a some quick links you might be
interested in:
* [Dashboards README](dashboards/README.md)
* [Todos](TODO.md)

## Idea
Promtimer:

1. starts a Prometheus server for each of the cbcollects you are interesteed 
   in exploring
1. generates a convenient Grafana configuration including supporting
   anonymous login, a custom home dashboard, data sources and dashboards
1. starts Grafana allowing you to login and browse dashboards that collect
   metrics from across each node in the cluster

## Dependencies

You will need:

* A Prometheus binary (version 2.20 or later)
* Grafana (version 7.1 or later)
* Promtimer
* Some cbcollects

If you happen to be building Couchbase Server 7.0 or later, you will already 
have a Prometheus binary: it's in the `install/bin` directory of one of your 
local builds. If you don't, the [Getting Started](https://prometheus.io/docs/introduction/first_steps/) 
instructions on the Prometheus website are comprehensive. You don't actually
need to install Prometheus, you just need the binary. [Downloading](https://prometheus.io/download/)
and unzipping the pre-compiled binaries for your platform is sufficient.

If you're on Mac,`brew` is convenient:

    brew install prometheus

It's also possible to build Prometheus from source yourself. This is 
straightforward:

```
git clone git@github.com:prometheus/prometheus.git
cd prometheus
make prometheus
```

You'll need a full Grafana install. The `grafana-server` binary alone isn't 
sufficient as Grafana ships with many configuration files. 
[Installation instructions](https://grafana.com/docs/grafana/latest/installation/) 
on the Grafana website look comprehensive. Follow the instructions for your
platform to get a recent version of Grafana. On Mac, it's easy:

    brew install grafana

To get Promtimer, clone this repo locally:

    git clone https://github.com/couchbaselabs/promtimer.git

As to cbcollects, you probably wouldn't be reading this if you didn't already
have them. 

## How to Use Promtimer

Assemble the cbcollects in a directory and unzip them. 

```
ls collectinfo*.zip | xargs -n 1 unzip
```

Start Promtimer:

```
bin/promtimer --grafana-home <path-to-grafana-shared-config-home>
     [--prometheus <path-to-prometheus-binary>]
```

The `path-to-grafana-shared-config-home` is what is known in Grafana terminology as the
"homepath". This is the out-of-the-box Grafana shared config path. On brew-installed
Grafana on Macs this is something like:

    /usr/local/Cellar/grafana/x.y.z/share/grafana

On linux systems the homepath should usually be:

    /usr/share/grafana

The `path-to-prometheus-binary` specifies the path to the prometheus binary
and is needed if the binary isn't locatable via your default search path.

The Grafana dashboards page should open for you automatically. If not, navigate
to `localhost:13000/dashboards` in your browser and begin exploring the
available dashboards.

## Clean Up

Promtimer creates a `.promtimer` sub-directory in the directory where it's
started and places the Grafana configuration. To clean up, remove this
directory:

```
rm -rf .promtimer
```

## Want to Help?

Awesome! See the [list of todos](TODO.md).
