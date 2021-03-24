---
title: Start Label Studio
type: guide
order: 202
---

After you install Label Studio, start the server to start using it. 

```bash
label-studio start
```

By default, Label Studio starts with a SQLite database to store labeling tasks and annotations. You can specify different source and target storage for labeling tasks and annotations using Label Studio UI or the API. See [Database storage](install.html#Database-storage) for more.

## Labeling performance 
<!--maybe put this with the database storage stuff? hmm-->

The SQLite database works well for projects with tens of thousands of labeling tasks. If you want to annotate millions of tasks or anticipate a lot of concurrent users, use a PostgreSQL database. See [Install and upgrade Label Studio](install.html#PostgreSQL-database) for more.  

For example, if you import data while labeling is being performed, labeling tasks can take more than 10 seconds to load and annotations can take more than 10 seconds to perform. If you want to label more than 100,000 tasks with 5 or more concurrent users, consider using PostgreSQL or another database with Label Studio. 

## Command line arguments for starting Label Studio
<!--make this a table of options with links-->

You can specify a machine learning backend and other options using the command line interface. Run `label-studio --help` to see all available options, or refer to the following tables.

Some available arguments for Label Studio provide information or start the Label Studio server:

| Command line argument |  Description |
| --- | ---- |
| `-h` `--help` | Display available command line arguments. |
| `--version` | Show the Label Studio version. |
| `--init` | Initialize Label Studio. |
| `start` | Start the Label Studio server. |
| `reset_password` | Reset the password for a specific Label Studio username. See [Create user accounts for Label Studio](signup.html). |
| `shell` | Run a Django shell. |


The following command line arguments are optional and must be specified with `label-studio start <argument> <value>` or as an environment variable when you set up the environment to host Label Studio:

| Command line argument | Environment variable | Description |
| --- | ---- | ---- |
| `-b`, `--no-browser` | N/A | Do not automatically open a web browser when starting Label Studio. |
| `-db` `--database` | `DATABASE` | Specify the database file path for storing labeling tasks and annotations. See [Database storage](install.html#Database_storage). |
| `--data-dir` | `DATA_DIR` | Directory to use to store all application-related data. |
| `-d` `--debug` | N/A | Enable debug mode for troubleshooting Label Studio. |
| `-c` `--config` | `CONFIG_PATH` | Path to the server configuration for Label Studio. |
| `-l` `--label-config` | `LABEL_CONFIG` | Path to the label configuration file for a specific Label Studio project. See [Set up your labeling project](setup.html). |
| `--ml-backends` | `ML_BACKENDS` | Specify the URLs for one or more machine learning backends. See [Set up machine learning with your labeling process](ml.html). |
| `--sampling` | N/A | Specify one of sequential or uniform to define the order for labeling tasks. See [Set up task sampling for your project](start.html#Set_up_task_sampling_for_your_project) on this page. |
| `--log-level` | N/A | One of DEBUG, INFO, WARNING, or ERROR. Use to specify the logging level for the Label Studio server. |
| `--internal-host` | `INTERNAL_HOST` | Web server address to make Label Studio available at. See [Run Label Studio on localhost with default settings](start.html#Run-Label-Studio-on-localhost-with-default-settings) on this page. |
| `-p` `--port` | `PORT` | Specify the web server port for Label Studio. Defaults to 8080. See [Run Label Studio on localhost with a different port](start.html#Run-Label-Studio-on-localhost-with-a-different-port) on this page. |
| `--host` | `HOST` | Specify the hostname to use to generate links for imported labeling tasks or static loading requirements. Leave empty to make all paths relative to the root domain. For example, specify `"https://77.42.77.42:1234"` or `"http://ls.example.com/subdomain/"`. See [Run Label Studio with an external domain name](start.html#Run-Label-Studio-with-an-external-domain-name) on this page. |
| `--cert` | `CERT_FILE` | Certificate file to use to access Label Studio over HTTPS. Must be in PEM format. See [Run Label Studio with HTTPS](start.html#Run-Label-Studio-with-HTTPS) on this page. | 
| `--key` | `KEY_FILE` | Private key file for HTTPS connection. Must be in PEM format. See [Run Label Studio with HTTPS](start.html#Run-Label-Studio-with-HTTPS) on this page. |
| `--initial-project-description` | `PROJECT_DESC` | Specify a project description for a Label Studio project. See [Set up your labeling project](setup.html). |
| `--password` | `PASSWORD` | Password to use for the default user. |
| `--username` | `USERNAME` | Username to use for the default user. |
| `--agree-fix-sqlite` | N/A | Automatically agree to let Label Studio fix SQLite issues when using Python 3.6–3.8 on Windows operating systems. | 


## Run Label Studio on localhost with default settings
By default, Label Studio uses the `0.0.0.0` address to start. If you want to use `localhost` instead, use the following command:
```bash
label-studio start --internal-host localhost
```

Or, set the following environment variable:
```
INTERNAL_HOST = localhost
```

## Run Label Studio on localhost with a different port
By default, Label Studio runs on port 8080. If that port is already in use or if you want to specify a different port, start Label Studio with the following command:
```bash
label-studio start --port <port>
```

For example, start Label Studio on port 9001:
```bash
label-studio start --port 9001
```

Or, set the following environment variable:
```
PORT = 9001
```

## Run Label Studio on Docker with a different port

To run Label Studio on Docker with a port other than the default of 8080, use the port argument when starting Label Studio on Docker. For example, to start Label Studio in a Docker container accessible with port 9001, run the following: 
```bash
docker run -it -p 8080:8080 -v `pwd`/mydata:/root/.local/share/label-studio/ heartexlabs/label-studio:latest label-studio --port 9001
```

Or, if you're using Docker Compose, update the `docker-compose.yml` file that you're using to expose a different port for the app. For example, this portion of the `docker-compose.yml` file exposes port 9001 instead of port 8080 for running Label Studio:
```
...
app:
  build: ./
  expose:
    - "9001"
...
```


## Run Label Studio on a remote server

<!--UPDATE internal host PARAMS? -->


## Run Label Studio on a remote server with an external domain name

<!--UPDATE HOST, PORT, AND INTERNAL_HOST PARAMS? -->


## Run Label Studio with HTTPS
To run Label Studio with HTTPS and access the web server using HTTPS in the browser, specify a certificate and private key when starting Label Studio. 

You can start Label Studio with the following command:
```bash
label-studio start --cert <certificate.pem> --key <keyfile.pem>
```

Or, set the following environment variables:
```
CERT_FILE = <certificate.pem>
KEY_FILE =  <keyfile.pem>
```

The certificate and private key files must both be provided as PEM files. 

## Run Label Studio on the cloud using Heroku
To run Label Studio on the cloud using Heroku, specify an environment variable so that Label Studio loads. 

```
LABEL_STUDIO_HOSTNAME
```

If you want, you can specify a different hostname for Label Studio, but you don't need to.

To run Label Studio with Heroku and use PostgreSQL as the [database storage](install.html#Database_storage), specify the PostgreSQL environment variables required as part of the Heroku environment variable `DATABASE_URL`. For example, to specify a PostgreSQL database hosted on Amazon:
```
DATABASE_URL = postgres://username:password@hostname.compute.amazonaws.com:5432/dbname
```
Then you can specify the required environment variables for a PostgreSQL connection as config variables. See [Database storage](install.html#Database_storage).


## Run Label Studio on the cloud using a different cloud provider
To run Label Studio on the cloud using a cloud provider such as Google Cloud Services (GCS), Amazon Web Services (AWS), or Microsoft Azure, 


## Run Label Studio with an external domain name

If you want multiple people to collaborate on a project, you might want to run Label Studio with an external domain name. 

To do that, specify the `host` parameter when you start Label Studio to ensure that the correct URLs are created when importing resource files (images, audio, etc) and generating labeling tasks.   

You can specify the external domain name when you start Label Studio from the command line. 
```bash
label-studio start --host http://subdomain.example.com/ls-root
```

Or, you can use environment variables:
```
HOST = https://subdomain.example.com:7777
```

You must specify the protocol for the domain name: `http://` or `https://`

If your external host has a port, specify the port as part of the host name. 


## Set up task sampling for your project 

When you start Label Studio, one way that you can order the way your imported tasks are exposed to annotators is to specify task sampling when you start Label Studio. 

For example, to specify sequential task ordering for annotators:
```bash
label-studio start --sampling sequential
```

The following table lists the available sampling options from the command line: 

| Option | Description |
| --- | --- | 
| sequential | Default. Tasks are shown to annotators in ascending order by the `id` field. |
| uniform | Tasks are sampled with equal probabilities. |

For more flexible task sampling and ordering options, see [Set up your labeling project](setup.html).