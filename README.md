# bench some web microframeworks (Ruby)

Because we don't have enough drama, let's bench some *hello world* apps!

## Why?

As part of some research, collected some benchmark around Ruby web
microframeworks.

The idea is bench the performance from [Rack](https://github.com/rack/rack),
throught the stack of the framework to the minimum representation of an
application (*Hello World* apps).

It is not the goal to bechmark hits to the database or external services,
nor to do a silly *fibonacci-as-a-service*, ok?

## What?

The following microframeworks were considered when doing this research:

- [Brooklyn](https://github.com/luislavena/brooklyn) - [brooklyn.ru](apps/brooklyn.ru)
- [Cuba](https://github.com/soveran/cuba) - [cuba.ru](apps/cuba.ru)
- [Gin](https://github.com/jcasts/gin) - [gin.ru](apps/gin.ru)
- [Hobbit](https://github.com/patriciomacadden/hobbit) - [hobbit.ru](apps/hobbit.ru)
- [Lotus (Router)](https://github.com/lotus/router) - [lotus-router.ru](apps/lotus-router.ru)
- [Nancy](https://github.com/heapsource/nancy) - [nancy.ru](apps/nancy.ru)
- [NYNY](https://github.com/alisnic/nyny) - [nyny.ru](apps/nyny.ru)
- [Mustermann](https://github.com/rkh/mustermann) - [mustermann.ru](apps/mustermann.ru)
- [Rack](https://github.com/rack/rack) - [rack.ru](apps/rack.ru) + [rack-response.ru](apps/rack-response.ru)
- ~~[Rails](https://github.com/rails/rails) - [rails.ru](apps/rails.ru)~~
- [Ramaze](https://github.com/Ramaze/ramaze) - [ramaze.ru](apps/ramaze.ru)
- [Rambutan](https://github.com/NewRosies/rambutan) - [rambutan.ru](apps/rambutan.ru)
- [Roda](https://github.com/jeremyevans/roda) - [roda.ru](apps/roda.ru)
- [Scorched](https://github.com/Wardrop/Scorched) - [scorched.ru](apps/scorched.ru)
- [Sinatra](https://github.com/sinatra/sinatra) - [sinatra.ru](apps/sinatra.ru)

Please note that while Rails has been added to the list, it is just a
minimalistic representation (using Metal, no full middleware stack, etc). You
shouldn't take the performance numbers mentioned here about Rails (or any
other) as scientific and decision-taking references.

## How?

Used [wrk](https://github.com/wg/wrk) to benchmark, locally, a burst of
requests (in 2 threads) over 10 seconds. The command line used was:

```console
$ wrk -t 2 http://localhost:9292/
```

All the frameworks were run using [Puma](https://github.com/puma/puma) on
Ruby 2.1, in production mode and using 16 threads:

```console
$ puma -e production -t 16:16 apps/<framework.ru>
```

## Run benchmark for all frameworks

```console
# use `bundle exec`, if needed
$ bench/summary-memory
$ bench/summary-speed
```

### Have some numbers around?

Yup, I do:

#### Requests/sec
<!-- speed_table -->
```
Framework            Requests/sec  % from best
----------------------------------------------
rack                      8607.68       100.0%
mustermann                7706.42       89.53%
hobbit                    7313.19       84.96%
lotus-router              7181.29       83.43%
roda                      7002.94       81.36%
rack-response             6689.92       77.72%
brooklyn                  6437.88       74.79%
rambutan                  6055.88       70.35%
cuba                      6037.42       70.14%
nancy                     5622.57       65.32%
nyny                      4074.75       47.34%
gin                       3806.78       44.23%
sinatra                   2853.92       33.16%
rails                     2517.19       29.24%
scorched                  1636.35       19.01%
ramaze                    1357.23       15.77%
```
<!-- speed_table_end -->

#### Memory Allocation/Request
<!-- mem_table -->
```
Framework       Allocs/Req Memsize/Req
--------------------------------------
rack                    60        1704
hobbit                  70        1976
roda                    73        2144
mustermann              73        2040
rack-response           83        3072
lotus-router            92        2272
brooklyn                98        2432
cuba                   100        3457
nancy                  107        3440
rambutan               116        3448
nyny                   169        5248
rails                  212        9109
gin                    214        9419
sinatra                255        6083
ramaze                 579       21837
scorched              1711      115524
```
<!-- mem_table_end -->


These numbers were collected on:

- OSX, 10.8.5, MacBook Pro i5 (2.5GHz), 8GB 1600 MHz DDR3.
- ruby 2.1.3p242 (clang 5.1 - clang-503.0.40)
