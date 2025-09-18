## Fruit harvester challenge

This is a project that collects data from robot harvesters and generates reports. The
objective is to consume data from robot harvesters, process it, persist it in a database
and generate reports based on the collected data.

### Background

Two farmers, denoted by index `1` and `2`, own robot harvesters that collect fruits from 5 different
trees. Each farmer owns 2-3 trees each which are denoted by index `1` to `5`. Due to an old family conflict however,
some of the trees are shared, and sometimes yields fruit for farmer `1`, and sometimes for farmer `2`. Your task is to
consume data from the harvesters which every 10 seconds sends a report over MQTT detailing how many fruits were
collected from each tree since the previous report. This data should be persisted in a database, and reports should be
generated detailing the cumulative yield for each farmer, tree and type of fruit.

### Setup

1. Clone this repo and open it in your favourite IDE.
2. Start the robot harvester simulator by running the following command in a terminal:
   ```bash
   docker compose up -d
   ```

Note: If you don't have docker/docker-compose installed, please refer to
the [official documentation](https://docs.docker.com/get-started/).

Note 2: If you want to restart the simulator from scratch, you can run

```bash
  docker compose down -v
```

to remove all data and then run the above command again.

Running the compose file will start the following tools:

#### MQTT broker

An MQTT broker is started on `localhost:1883`. You can use any MQTT client to subscribe to the topic
`/harvest/robot-data`
to see the data being published by the robot harvesters. No auth is required to connect. The data is published in JSON
format. All incoming messages follows the same format which you have to figure out by looking at the data being
published.

#### Database

A PostgreSQL database is started on `localhost:5432`. You can connect to it using any PostgreSQL client.
The database name is `database`, the username is `user` and the password is `password`.

#### pgadmin4

A pgadmin4 instance is started on `localhost:8080`. You can use it to manage the PostgreSQL database. Note that you have
to specify the database password `password` when opening pgadmin4 for the first time.

#### Node-RED

A Node-RED instance is started which generates the robot harvester data. It will push data to the MQTT broker every 10
seconds.

### Tasks

1. Create a database schema to store the data from the robot harvesters. You can add as many tables as you want.
2. Create a service in any programming language of your choice that connects to the MQTT broker, subscribes to the topic
   `/harvest/robot-data`, processes the incoming data and persists it in the database.
3. Create a report that shows the cumulative yield (sum) for each farmer, tree and type of fruit. Choose any of the
   following
   formats for the report:
    - MQTT message that is published to the topic `/harvest/report` every minute. Here you can also split the report
      into multiple messages on different topics if you want for example `/harvest/report/farmer/1`,
      `/harvest/report/farmer/2`
      etc.
    - A REST API endpoint that returns the report(s) in JSON format. Here you could do `GET` methods on
      `/harvest/report?farmer=1`, `/harvest/report?farmer=2`, `/harvest/report?fruit=APPLE` etc.
    - A simple web page/dashboard that displays the report.
    - Or multiple/all of the above.

### Guidelines

- You can take as much time as you want to complete the task, but it should not take more than a few hours.
- You can over-engineer the solution if you want, but prepare to be able to explain your design decisions.
- You can use any programming language and libraries of your choice.
- You can use any database of your choice, but PostgreSQL is recommended since it is already deployed in the project.
- You can use any MQTT client library of your choice.
- You can use any web framework of your choice if you decide to create a REST API or a web page/dashboard.
- You can use AI tools like ChatGPT to help you with the implementation, but please make sure to understand the code
  and not just copy-paste it.
- Try to build a solution which is responsive, even at larger time horizons and data volumes. For example, if the
  harvester reports every 10 seconds for a whole day, that would be 8640 messages per harvester, and with 2 harvesters
  that would be 17280 messages per day. The solution should be able to handle this amount of data without significant
  performance degradation.

### Practical information

- Please contact me if you have any questions regarding the task/if something
  isn't working as expected.
- When you are done, please create a pull request in this repo or zip the project and send it to me. Make sure to
  include any SQL schemas, for example by including the table definitions in `.sql` format. A meeting will be booked
  where we together will go through the solution and discuss your design decisions.

**Good luck and have fun!**

