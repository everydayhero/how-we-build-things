# panoramix
A reporting service with a panoramic view across our business. Named after a druid known for his magic elixirs.

![Panoramix](http://vignette3.wikia.nocookie.net/asterix/images/a/ad/Getafix_brewing.jpg/revision/latest?cb=20150710202238)

The role of Panoramix is to produce reports and provide API endpoints of data from across our business. Its primary source of data is the event data stored in Redshift by Segment SQL. The role of Panoramix in our wider ecosystem is illustrated in the following diagram.

![Panoramix Architecture](https://cloud.githubusercontent.com/assets/806356/10875495/ba8d6124-817e-11e5-9643-663906bd403d.png)

Other services in our ecosystem send events to Segment, which stores them in Redshift. Panoramix uses this data source to generate views or "projections", which it uses for faster generation of reports and API responses than it could by querying the event store directly.

An important aspect of this design is the decoupling of Panoramix as the reporting system from other services. It would be possible for much of the data required by Panoramix to be obtained by some kind of ETL process that accessed the databases of each service directly, rather than utilising an event based system as described above. The primary drawback to this approach would not only be a tight coupling of the reporting system to the database schema of every other service, but also a duplication of business logic applied to the data. A lot of logic concerned with how to interpret and derive information from the data would need to also exist in the reporting system. Using events, the events can contain any derived or "business meaningful" fields, removing the need for the reporting system to know how to derive these. Panoramix can then combine event data from any service, e.g. Supporter, NFP, Payments etc, to produce a wider view of our business.

## Application Architecture
Panoramix is generally based on a pattern known as the Lambda Architecture, which consists of 3 layers:

- The Batch Layer

Performs queries on the "master data set" or event store. Since the event store is append only and contains a complete history of events, these queries are on a large data set and may take some time. Batch layer queries are run at periodic intervals or within an infinite loop. The Amazon Redshift database provided by the Segment SQL service is the event store for Panoramix.

- The Serving Layer

The results of the batch layer queries are stored in another database for faster querying. These views allow web requests to be handled quickly, or reports to be generated without querying the entire history of events. Panoramix uses Postgres as a serving layer database.

- The Speed Layer

The batch layer and serving layer introduce a time lag between an event occurring and the data being available in the serving layer. If near real time data is required, the speed layer provides a mechanism to consume the stream of events and maintain a temporary aggregation of the data. The speed layer and serving layer views can then be combined at query time to produce near real time results of queries over very large data sets. Panoramix currently has no need for near real time data, but this could easily be introduced at a later time if required.

A key aspect of this design is that if new queries are introduced, or the logic in queries is modified, the batch layer can re-compute a correct result. Any inaccuracies introduced by the speed layer are temporary until the next batch query run.

## Development
Panoramix is written in Elixir using the Phoenix framework.
Docker-compose is used to define the development environment, so installing elixir locally is not required - although sometimes it can be more convenient than docker-compose.

To aid the convenience a little, wrapper scripts have been created in the `bin` directory that abstract the verbosity of calling docker-compose commands. These are described below.
### Setup
Ensure you have docker and docker-compose installed, and are logged into quay.io with `docker login quay.io`.

### Run tests
Run all tests:
`bin/mix test`

Run one file:
`bin/mix test path/to/test.ex`

Run a single test:
`bin/mix test path/to/test.ex:[line number]`

### Interective REPL
`bin/iex -S mix`
(leaving off `-S mix` opens a repl but does not load project dependencies)

### CI
`bin/ci` contains the script run by buildkite for CI.
