# Software engineer technical test

The goal of this technical test is to check if the candidate
can build quality software to solve business problems.

## The business problem to solve

> Disclaimer: this is a toy problem not an actual project :-)

Artists who publish their music on our music streaming platform
want to know and reach to their fans.
We need to provide tools to help them connect to their fan base.

The tools to develop:
1. **Fanbase**: a web API that provide aggregated information
  about the fanbase of an artist.
2. **TopFans**: a script that displays the artist top fans information,
  so the artist can send them VIP concert tickets and other goodies.

For legal reasons:
1. Users who do not consent to tracking are excluded from the result produced
  by Fanbase and TopFans tools. See the `consent` field in the database.
2. Minor (underage) users information is not displayed by TopFans.

### Technical specifications

* All the programs (Fanbase web API & TopFans script) should run inside
  [Docker containers](https://docs.docker.com/compose/).
* The containers should be organised with a
  [Docker compose configuration](https://docs.docker.com/compose/).
* A [MariaDB](https://mariadb.org/) database container is provided
  with the user data. See [database/](database/) directory.

#### Fanbase tool specifications

* Fanbase should be built as follows:

  ```sh
  docker-compose build fanbase
  ```

* Then, the web API should be started with the following command:

  ```sh
  docker-compose up fanbase
  ```

* HTTP requests should be sent the Fanbase web API like the example bellow:

  ```sh
  curl -XPOST http://localhost:8080/fanbase -d '{"artist_id": 123}'
  ```

* The expected response is a JSON object with the following fields:

  |Field|Type|Description|
  |-----|----|-----------|
  |success|`bool`|the artist have at least one fan identified fan|
  |artist_id|`int`|ID of the artist, provided by the HTTP request|
  |total_fans|`int`|size of the artist fan base|
  |fans_median_age|`int`|median age of the artist fans|
  |fans_top_countries|`array[string]`|top 3 countries with the most fans (ordered, top country first)|

  Example result:

  ```json
  {
    "success": true,
    "artist_id": 123,
    "total_fans": 456,
    "fans_median_age": 21,
    "fans_top_countries": ["TN", "FR", "NE"]
  }
  ```

### TopFans specifications

* TopFans script is partially written in the
  [topfans/run.py](topfans/run.py) file. But this version don't work
  and should be refactored.

* The refactored and working script should be written in Python.

* The script should be set-up with this command:

  ```sh
  docker-compose build topfans
  ```

* Then it should be executed as follows:

  ```sh
  docker-compose run topfans --artist_id 123 --nb_fans 3
  ```

* The expected output should be a CSV content with the following fields:

  |Field|Type|Description|
  |-----|----|-----------|
  |rank|`int`|rank of the fan, the top fan have the rank = `1`|
  |artist_id|`int`|ID of the artist, provided as input parameter|
  |user_id|`int`|ID of the user|
  |country|`string`|user's country|
  |score|`float`|from `0` = not fan of the artist to `1` = die-hard fan|

  Example result:

  ```csv
  rank,artist_id,user_id,country,score
  1,123,987,"TN",0.91
  2,123,654,"FR",0.90
  3,123,321,"TN",0.74
  ```

## Expected deliverables

1. A **working**, maintainable and **easy to read** source codes.
  All the project files should be compressed into a single `zip` file.
2. Including a `NOTES.md` file where you explain your design and implementation
  choices.

## Bonus

Not required but nice to have:

* Python coding style compatible
  with the [PEP8](https://www.python.org/dev/peps/pep-0008/).
* Unit tests can be added with notes about how to run them.
* Authentication support can be added to Fanbase web API.
* Improvements to the tools can be proposed in the `NOTES.md` file.

## Troubleshooting

If you have any issue or any question, **please contact us**
and we'll help you.

---

**Happy coding.**
