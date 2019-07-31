# poefixer
A Path of Exile Stash API reader and database manager

## Context

Path of Exile is an action RPG video game with a massive in-game economy, mediated by external websites. To facilitate this interaction, the publisher maintains an HTTP/JSON API for access to the live feed of items and player pricing. This software is designed to harvest that API, store it to a local database and perform some basic post-processing to get a set of derivative data suitable for further analysis.

## Content

This module consists of two major components, both of which are
accessible from the top-level namespace or can be imported individually:

* `stashapi`

The *stashapi module* is the HTTP interface to public stash tab updates
published by GGG. This is how Path of Exile trading sites get their data,
though if you're not whitelisted, you will be rate restricted by default.

The *db module* is what takes the objects created by stashapi and writes them
to your database. This creates a _current_ (not time series) database of all
stashes and items.

## Usage

To load data:

* Begin by creating a database somewhere. MySQL is the most
  heavily tested by the author, but you can try something else
  if you want. Most of the code relies on sqlalchemy, so it should
  generally just work...
* Install the module. If you are running from source code, you
  can `pip install -e .` or you can just set the environment variable
  `PYTHONPATH=.` once or before each command.

These programs provide a basic database structure and will auto-instantiate
tables that they need. However, they are also too slow to keep up with the
entire output of the community! You would have to create a parallelized
version of the `sample_api_reader.py` script and the currency processor for
that, and that's beyond the scope of this project, mostly because doing
so without being whielisted by GGG would hit their auto-rate-limiting
thresholds, and I'm not yet a big enough fish to get on that list.

That being said, you can accomplish quite a bit, just by regularly updating
your pull to the most recent `next_id` and re-running. If you just wish
to analyze the market, this is more than sufficient, and will quickly give
you gigabytes of market data!

This isn't (yet) a full-featured end-user tool. It's really aimed at
Python developers who wish to begin working with the Path of Exile API,
and don't want to write the API and DB code themselves (I know I didn't!)

-A

Edit:
This is a fork of the original package by ajs, which included DB support and a whole lot of other stuff.
I've removed the DB bits and removed the dependency on rapidjson to make it fit my needs.
- S