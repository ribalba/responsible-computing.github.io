# Responsible Computing & Sustainable ICT deadlines countdown

Based on [sec-deadlines](https://sec-deadlines.github.io/) by Clement Fung and Bogdan Kulynych.

Based on [ai-deadlines](https://aideadlin.es) by @abshkdz

## Adding/updating a conference

* Read the data format description below. **Note that the timezone format sign is inverted** (e.g., UTC+7 is written as `Etc/GMT-7`). It's [not a bug][0]. I hate this format too. I'd be happy to move to a different timezone JavaScript library that uses a friendlier format, but I don't have time for that.
* Update `_data/conferences.yml`. You can do that on Github or locally after forking the repo.
* Send a pull request

### Conference entry record

Example record:

```
- name: LIMITS
  year: 2022
  date: "June 21 – 22"
  description: "Workshop on Computing within Limits"
  link: https://computingwithinlimits.org/
  deadline:
    - "2022-04-01 23:59"
  place: Online
  tags: [HARD, SOFT, ETPO, SHOP]
```

Descriptions of the fields:

| Field name    | Description                                                 |
|---------------|-------------------------------------------------------------|
| `name`\*      | Short conference name, without year                         |
| `year`\*      | Year the conference is happening                            |
| `description` | Description, or long name                                   |
| `link`\*      | URL to the conference home page                             |
| `deadline`\*  | A list of deadlines. (Gory details below)                   |
| `timezone`    | Timezone in [tz][1] format. By default is UTC-12 ([AoE][2]) |
| `date`        | When the conference is happening                            |
| `place`       | Where the conference is happening                           |
| `tags`        | One or multiple tags: See below.                            |

Fields marked with asterisk (\*) are required.

Tags:

- Hardware & Electronics: HARD
- Software: SOFT
- Human Factors: HUCO
- Ethics & Policy: ETPO
- Conference: CONF
- Workshop: SHOP
- Journal: JOUR


### Deadline format

The *deadline* field can contain:

1. The simplest option: a date and time in ISO format. Example: `["2017-08-19 23:59"]` (Note that you need to wrap even a single deadline in a list).
2. If a deadline is rolling, you can use a template date, just substitute the
   year with `%y` and the year before the conference with `%Y`. Example:
   `["%y-01-15 23:59"]` means there is a deadline on the 15th January in the
   same year as the conference.
2. A list of (1) or (2). Example of two rolling deadlines, with one in the end
   of October in the year prior to the conference year, and the second in the
   end of February in the same year as the conference:
  ```
  - "%Y-10-31 23:59"
  - "%y-02-28 23:59"
  ```

On the page, all deadlines are displayed in viewer's local time (that's a feature).

*Note:* If the deadline hour is `{h}:00`, it will be automatically translated into `{h-1}:59:59` to avoid pain and confusion when it happens to be midnight in local time.

### Timezones

The timezone is specified in [tz format][1]. Unlike abbreviations (e.g. EST), these are un-ambiguous. Here are tz codes for some common timezones:

| Common name                   | tz                                                                 |
|-------------------------------|--------------------------------------------------------------------|
| UTC                           | `Etc/UTC`                                                          |
| America Pacific Time          | `America/Los_Angeles`                                              |
| Pacific Standard Time (UTC-8) | `Etc/GMT+8` (Yes, the sign is inverted for some weird reason)      |
| America Eastern Time          | `America/New_York`                                                 |
| Eastern Standard Time (UTC-5) | `Etc/GMT+5`                                                        |
| American Samoa Time (UTC-11)  | `Pacific/Samoa` or `Etc/GMT+11`. This timezone does not use DST.   |
| Aleutian Islands              | `America/Adak`                                                     |

[0]: https://momentjs.com/timezone/docs/#/zone-object/offset/
[1]: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
[2]: https://www.timeanddate.com/time/zones/aoe

## Running locally

The site is hosted using [GitHub Pages](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll) and build with [jekyll](https://jekyllrb.com/)

If you want to see your changes locally before committing them you can install [jekyll](https://jekyllrb.com/docs/) locally or you can use docker to do everything for you:
```
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  --volume="jekyll_bundler_cache:/usr/local/bundle" \
  --publish 4000:4000 \
  jekyll/jekyll \
  sh -c "bundle config set path '/usr/local/bundle' && bundle install && jekyll serve"
```

This will host the page on http://0.0.0.0:4000/ for you to check. This will also allow live edits, so you don't need to restart the server after edit. It will also create a new volume to cache the bundler gems. You can either remove the line `--volume="jekyll_bundler_cache:/usr/local/bundle" \` or `docker volume rm jekyll_bundler_cache` will also clean everything up.