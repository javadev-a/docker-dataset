# Docker Dataset

Have you ever wanted to access pre-populated databases with dummy but valid data? It can be for something as simple as practicing writing SQL queries to running tests on databases. Under such circumstances, you have to either have to create dummy data or utilize some internet-searching skills to find data to populate your database. I think this is a common enough problem/requirement that solution can be Dockerized for reuse. So here is a Docker image for [PostgreSQL](https://www.postgresql.org/) with databases populated with sample data.

## Datasets

So far we have the following datasets which are being used in the images.
* [Postgres Sample Databases](https://wiki.postgresql.org/wiki/Sample_Databases): The datasets being used from here are `dellstore2` (tagged `dellstore`), `iso3166`,  `pagila`, `sportsdb`, `usda` and `world`. `pagila` is not being used from the pgFoundry source because it doesn't directly work with the newer version of PostgreSQL we're using. So we're using [this modified version](https://github.com/devrimgunduz/pagila) which seems okay other than the fact that the maintainer changes the dates in the data every year to reflect the new year. I don't care enough about it as of now, but that might change in the future. pgFoundry has been down for a few days now. Therefore we have switched the URLs to their FTP sources [here](https://www.postgresql.org/ftp/projects/pgFoundry/dbsamples/).

## Databases

The only database supported so far is [PostgreSQL](https://www.postgresql.org/). We use the `alpine` version of the official image as the base image to keep our image slim.

## Tags

Available tags are `dellstore`, `iso3166`,  `pagila`, `sportsdb`, `usda`, `world`, `all` and `latest`. `all` and `latest` are the same image with all the datasets in one image. Each of them has been loaded into their own database in the image. The rest of the tags belong to images single datasets.

## Usage

You can start the container by running:
```
docker run -d --name pg-ds-<tag> aa8y/postgres-dataset:<tag>
```
and access it by:
```
docker exec -it pg-ds-<tag> psql -d <db_name>
```
where `<tag>` is one of the tags mentioned [here](#tags) and `<db_name>` is the database name which is one of the dataset names mentioned [here](#datasets). You can also use them with `docker-compose`. See [this example](https://github.com/aa8y/data-dude/blob/master/docker-compose.yml) for information on how to use them.

## Custom images

If you want to build a custom image with not one or all the datasets, but some, then you can do that using:
```
docker build -t aa8y/postgres-dataset:some --build-arg DATASETS=dellstore,world .
```
and then following the same [aforementioned](#usage) steps for using your custom image.

## Future Work

* Images for other popular databases like [MySQL](https://www.mysql.com/).
* Add `french-towns-communes-francais` datasets from [Postgres Sample Databases](https://wiki.postgresql.org/wiki/Sample_Databases).
* Find and add more free data sources.
