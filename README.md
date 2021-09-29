# Software engineer technical test

The goal of this technical test is to check if the candidate
can build quality software to solve business problems.

## The business problem to solve

> Disclaimer: this is a toy problem not an actual project :-)

Artists who publish their music on our music streaming platform
want to know and reach their fans.
We need to provide tools to help them connect to their fanbase.

The tools to develop:
1. A **web API** that provide aggregated information
  about the fanbase of an artist.
2. A **script** that displays the artist top fans information,
  so the artist can send them VIP concert tickets and other goodies.

For legal reasons:
1. Users who do not consent to tracking are excluded from the web API
  & the script results. See the `consent` field in the database.
2. Minor (underage) users information is not displayed by the script.

### Technical specifications

* All the programs (web API & script) run inside
  [Docker containers](https://docs.docker.com/compose/)
* The containers are organised with a
  [Docker compose configuration](https://docs.docker.com/compose/)
* A database container is provided with the user data


#### Web API specifications

* The web API is set-up with:

  ```sh
  docker-compose build webapi
  ```

* Then, the web API is started with:

  ```sh
  docker-compose up webapi
  ```

* Requests are sent to the web API with the following command:

  ```sh
  curl -XPOST http://localhost:8080/fanbase -d '{"artist_id": 123}'
  ```

* The expected response is a JSON object with the following fields:

  |Field|Type|Description|
  |-----|----|-----------|
  |success|`bool`|the artist have at least one fan identified fan|
  |artist_id|`int`|the artist ID number provided by the request|
  |total_fans|`int`|size of the artist fanbase|
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

### Script specifications

* The script is partially written in the
  [script/top_fans.py](script/top_fans.py) file. But this version don't work
  and should be refactored.

* The refactored and working script should be written in Python.

* The script is set-up with:

  ```sh
  docker-compose build script
  ```

* Then it is executed with:

  ```sh
  docker-compose run script --artist_id=123 --nb_fans=3
  ```

* The expected output is a CSV content with the following fields:

  |Field|Type|Description|
  |-----|----|-----------|
  |artist_id|`int`|the artist ID number provided with the request|
  |user_id|`int`|the ID of the fan|
  |country|`string`|user country|
  |score|`float`|from `0` = not fan of the artist to `1` = die-hard fan|

  Example result:

  ```csv
  artist_id,user_id,country,score
  123,987,"TN",0.91
  123,654,"FR",0.90
  123,321,"TN",0.74
  ```

## Expected deliverables

1. A **working**, maintainable and **easy to read** source code.
  All the project files should be compressed into a single `zip` file.
2. Including a `NOTES.md` file where you explain your design and implementation
  choices.

## Bonus

Not required but nice to have:

* Python coding style compatible
  with the [PEP8](https://www.python.org/dev/peps/pep-0008/).
* Unit tests.
* Web API authentication.

## Troubleshooting

If you have any issue or any question, **please contact us**
and we'll help you.

---

**Happy coding.**
