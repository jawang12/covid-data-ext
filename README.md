![](https://github.com/jawang12/covid-data-ext/workflows/Update%20Data/badge.svg)

# COVID-19-worldwide-json-data-script

A script to access Coronavirus COVID-19 worldwide data in JSON format with extended daily insights

This script stores data in JSON format from the John Hopkins University CSSE via [mathdroid API](https://github.com/mathdroid/covid-19-api).

It will get updated every 8 hours by github actions.

## Changelogs

### [ 2, Apr 2020 ]

Add growth and growth rate data to dailyReports.

There are new key-values (You also can check the Data Format below for further details):

```json
    "confirmedGrowth": 0,
    "recoveredGrowth": 0,
    "deathsGrowth": 0,
    "confirmedGrowthRate": 0,
    "recoveredGrowthRate": 0,
    "deathsGrowthRate": 0
```

## Data Format

The JSON contains global "confirmed", "recovered" and "deaths" cases summary updated by "lastUpdate" at the beginning.

Following "dailyReports" is an array which contains global daily summary and details of every involved country ordered by date since 2020-01-22.

By using data, you can validate "errorStatus !== true" at first to get meaningful data.

```json
{
    "errorStatus": false,
    "lastSync": "2020-04-01T15:15:46.146Z",
    "lastUpdate": "2020-04-01T13:42:16.000Z",
    "confirmed": 883225,
    "recovered": 185377,
    "deaths": 44156,
    "dailyReports": [
        {
            "errorStatus": false,
            "updatedDate": "2020-01-22",
            "confirmed": 555,
            "recovered": 28,
            "deaths": 17,
            "countries": [
                {
                    "country": "China",
                    "countryCode": "CN",
                    "confirmed": 547,
                    "recovered": 28,
                    "deaths": 17,
                    "confirmedGrowth": 0,
                    "recoveredGrowth": 0,
                    "deathsGrowth": 0,
                    "confirmedGrowthRate": 0,
                    "recoveredGrowthRate": 0,
                    "deathsGrowthRate": 0,
                },
                ...
            ],
            "confirmedGrowth": 0,
            "recoveredGrowth": 0,
            "deathsGrowth": 0,
            "confirmedGrowthRate": 0,
            "recoveredGrowthRate": 0,
            "deathsGrowthRate": 0,
        },
        {
            "errorStatus": false,
            "updatedDate": "2020-01-23",
            "confirmed": 653,
            "recovered": 30,
            "deaths": 18,
            "countries": [
                {
                    "country": "China",
                    "countryCode": "CN",
                    "confirmed": 639,
                    "recovered": 30,
                    "deaths": 18,
                    "confirmedGrowth": 92,
                    "recoveredGrowth": 2,
                    "deathsGrowth": 1,
                    "confirmedGrowthRate": 0.16819012797074953,
                    "recoveredGrowthRate": 0.07142857142857142,
                    "deathsGrowthRate": 0.058823529411764705,
                },
                ...
            ],
            "confirmedGrowth": 98,
            "recoveredGrowth": 2,
            "deathsGrowth": 1,
            "confirmedGrowthRate": 0.17657657657657658,
            "recoveredGrowthRate": 0.07142857142857142,
            "deathsGrowthRate": 0.058823529411764705
        },
        ...
    ]
}
```

_Note: The "dailyReports" array is ordered by date since 2020-01-22. It means you can get spcific date by index.
For example, if you want to log a daily report by 2020-03-14:_

```js
//This example uses "moment" library to calculate the index
const moment = require('moment');
const recordIndex = (date) => {
  return moment(date).diff('2020-01-22', 'day');
};

fetch(
  'https://github.com/maxMaxineChen/COVID-19-worldwide-json-data-script/blob/master/data/data.json'
)
  .then((response) => response.json())
  .then((data) => {
    if (data.dailyReports[recordIndex('2020-03-14')].errorStatus === false)
      console.log(
        `2020-03-14 daily reports: ${
          data.dailyReports[recordIndex('2020-03-14')]
        }`
      );
  });
```

## Script Usage

1.  **Clone.**

    ```shell
    git clone https://github.com/maxMaxineChen/COVID-19-worldwide-json-data-script.git
    ```

2.  **Install deps.**

    ```shell
    cd COVID-19-worldwide-json-data-script/
    npm install
    ```

3.  **Running the script**

    ```shell
    # Option 1. Gets the latest data to store at /data/data.json and saves log at updated.log (recommended!)
    npm start
    #Option 2. Updates data based on the exisited /data/data.json to the latest date
    npm update
    #Option 3. Builds the latest data to override the existed /data/data.json
    npm build
    ```

    _Note:_

    If you see "Failed to fetch 2020-XX-XX Error code: 404" in log, it means the data hasn't updated from API server (Since the data updating always has gap, you may see the error often for the latest one or two days data fetching. Don't worry and keep using the existing data ^-^ ).

    If you see that the error code is 443, it means the API server failed to server some commands shortly.

    Running "npm start" OR "npm update" later if you like to fix some error data or get updated to the latest date.
