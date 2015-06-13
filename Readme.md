[![Build Status](https://travis-ci.org/palkan/influx_udp.svg?branch=master)](https://travis-ci.org/palkan/influx_udp)

Erlang InfluxDB UDP Writer
==========================

Write data to Influxdb (~>0.8) using JSON UDP interface [(influxdb docs)](http://influxdb.com/docs/v0.8/api/reading_and_writing_data.html#writing-data-through-json-+-udp).


**Erlang version:** >=17.1

## Setup

rebar.config
```erlang
...
{deps, [
  ...
  {influx_udp, ".*", {git, "https://github.com/palkan/influx_udp.git", "master"}}
]}.
...
```

app.config
```erlang
[
  {influx_udp,
    [
        {influx_host, '8.8.8.8'}, %% defaults to '127.0.0.1'
        {influx_port, 4444}, %% defaults to 4444
        {pool_size, 5}, 
        {max_overflow, 10} %% poolboy settings (defaults are 1 and 0)
    ]
  }
].
```

## Usage

First, you need to start application: 

```erlang
influx_udp:start().
``` 

And then you can create pools and write data to InfluxDB.

## Pools

The default pool is started on application start unless you specified `{default_pool, false}` in configuration file.

Default pool uses parameters specified in application configuration.

You can run pools manually:

```erlang
influx_udp:start_pool(my_pool, #{ host => 'yet.another.influx.host' }).
```

Options not specified in `influx_udp:start_pool/2` would be taken from default configuration.

## Writing data

```erlang

%% Writing to named pool
influx_udp:write(my_pool, Series::string()|atom()|binary(), Points::list(map())|list(proplist())|map()|proplist()). 

%% Writing to default pool
influx_udp:write(Series, Points).

%% Writing raw valid InfluxDB input data
influx_udp:write(Data::binary()).

%% or
influx_udp:write(my_pool, Data::binary()).
```
