# conrad — Conference Radar 📡

[![image](https://img.shields.io/pypi/v/conference-radar.svg)](https://pypi.org/project/conference-radar/) [![image](https://img.shields.io/pypi/pyversions/conference-radar.svg)](https://pypi.org/project/conference-radar/) [![image](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/ambv/black)

`conrad` helps you track conferences and meetups on your terminal!

---

Here's how it works:

<pre>
$ conrad show
</pre>

| id     | name               | url                               | city             | state  | country | start_date | end_date   |
|--------|--------------------|-----------------------------------|------------------|--------|---------|------------|------------|
| 3ae4f9 | PyCon Fr 2019      | https://www.pycon.fr/             | Bordeaux         |        | France  | 2019-10-31 | 2019-11-04 |
| 68bf57 | PyCon Sweden       | http://www.pycon.se/              | Stockholm        |        | Sweden  | 2019-10-31 | 2019-11-02 |

## Why conrad?

- 📅 Never miss CFP deadlines again. `conrad remind` can remind you every time you open a terminal!
- 📊 Query and explore events using tags, names, locations, and dates. `conrad show --cfp` will tell you about events where the cfp is open!
- 🤖 ([upcoming](https://github.com/vinayak-mehta/conrad/issues/17)) The event database updates automatically with events curated by the community.
- ⛏️ You can manually add events using `conrad import -f events.json` and raise a PR!
- 🔄 Run `conrad refresh` to get the latest events!

## Installation

You can simply use pip to install `conrad`:

<pre>
$ pip install conference-radar
</pre>

## Features

### Set reminders

You can set CFP reminders so that you never miss a deadline! The color changes based on event proximity; **> 30 days** ![#008000](https://placehold.it/15/008000/000000?text=+), **>10 and < 30 days** ![#ffff00](https://placehold.it/15/ffff00/000000?text=+) and **< 10 days** ![#ff0000](https://placehold.it/15/ff0000/000000?text=+).

<pre>
$ conrad remind -i 6bb714
$ conrad remind
</pre>

| name     | start_date | days_left                         |
|----------|------------|-----------------------------------|
| PyCon US | 2020-04-15 | **52 days left to cfp deadline!** |

**Protip**: Add `conrad remind` to your shell startup file so that you get a reminder every time you open a new terminal!

### Query and explore

You can query and explore the event database using various filters.

---

Look at events which have an open call for proposals (cfp):

<pre>
$ conrad show --cfp
</pre>

| id     | name     | url                  | city       | state        | country | start_date | end_date   |
|--------|----------|----------------------|------------|--------------|---------|------------|------------|
| 6bb714 | PyCon US | https://us.pycon.org | Pittsburgh | Pennsylvania | USA     | 2020-04-15 | 2020-04-23 |

---

Look at conferences using a tag:

<pre>
$ conrad show --tag python
</pre>

| id     | name               | url                               | city             | state  | country | start_date | end_date   |
|--------|--------------------|-----------------------------------|------------------|--------|---------|------------|------------|
| 3ae4f9 | PyCon Fr 2019      | https://www.pycon.fr/             | Bordeaux         |        | France  | 2019-10-31 | 2019-11-04 |
| 68bf57 | PyCon Sweden       | http://www.pycon.se/              | Stockholm        |        | Sweden  | 2019-10-31 | 2019-11-02 |

---

Look at conferences using a name:

<pre>
$ conrad show --name pycon
</pre>

| id     | name               | url                               | city             | state  | country | start_date | end_date   |
|--------|--------------------|-----------------------------------|------------------|--------|---------|------------|------------|
| 3ae4f9 | PyCon Fr 2019      | https://www.pycon.fr/             | Bordeaux         |        | France  | 2019-10-31 | 2019-11-04 |
| 68bf57 | PyCon Sweden       | http://www.pycon.se/              | Stockholm        |        | Sweden  | 2019-10-31 | 2019-11-02 |

---

Look at conferences in a city, state or country:

<pre>
$ conrad show --location usa
</pre>

| id     | name               | url                               | city             | state  | country | start_date | end_date   |
|--------|--------------------|-----------------------------------|------------------|--------|---------|------------|------------|
| 66867c | PyCascades 2020      | https://2020.pycascades.com             | Portland         | Oregon | USA  | 2020-02-08 | 2020-02-10 |
| 6bb714 | PyCon US | https://us.pycon.org | Pittsburgh | Pennsylvania | USA     | 2020-04-15 | 2020-04-23 |

---

Look at conferences based on when they're happening:

<pre>
$ conrad show --date ">= 2019-10-01" --date "<= 2020-01-01"
</pre>

| id     | name               | url                               | city             | state  | country | start_date | end_date   |
|--------|--------------------|-----------------------------------|------------------|--------|---------|------------|------------|
| 3ae4f9 | PyCon Fr 2019      | https://www.pycon.fr/             | Bordeaux         |        | France  | 2019-10-31 | 2019-11-04 |
| 68bf57 | PyCon Sweden       | http://www.pycon.se/              | Stockholm        |        | Sweden  | 2019-10-31 | 2019-11-02 |

### Continuous updates (upcoming)

The event list is maintained in `data/events.json`. This list is continuously updated using the available `scrapers`.

You can get the latest events using:

<pre>
$ conrad refresh
</pre>

---

You can add new events to the list! The [Contributor's Guide](CONTRIBUTING.md) has detailed information about guidelines around contributions.

To add new events, create a `new_events.json` file containing the list of events you want to add, with the following fields:

<pre>
[
    {
        "name": "PyCon US",
        "url": "https://us.pycon.org",
        "city": "Pittsburgh",
        "state": "Pennsylvania",
        "country": "USA",
        "cfp_open": true,
        "cfp_start_date": "2019-09-12",
        "cfp_end_date": "2019-12-20",
        "start_date": "2020-04-15",
        "end_date": "2020-04-23",
        "source": "https://www.python.org/events/",
        "tags": "['python']",
        "kind": "conference"
    }
]
"new_events.json" 17L, 436C
</pre>

After that, clone this repository and create a new branch:

<pre>
$ git clone https://www.github.com/vinayak-mehta/conrad
$ cd conrad
$ git checkout -b add-new-event
</pre>

Then import the new events:

<pre>
$ conrad import -f ../new_events.json
</pre>

And finally push your changes and raise a PR!
<pre>
$ git add .
$ git commit -m "Add new events"
$ git push origin add-new-event
</pre>

## Versioning

`conrad` uses [Semantic Versioning](https://semver.org/). For the available versions, see the tags on this repository.

## License

This project is licensed under the Apache License, see the [LICENSE](https://github.com/vinayak-mehta/conrad/blob/master/LICENSE) file for details.
