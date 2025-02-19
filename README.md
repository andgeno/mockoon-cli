<div align="center">
  <a href="https://mockoon.com" alt="mockoon logo">
    <img width="200" height="200" src="https://mockoon.com/images/logo-square-cli.png">
  </a>
  <br>
  <a href="https://mockoon.com/#download"><img src="https://img.shields.io/badge/Download%20app-Go-green.svg?style=flat-square&colorB=1997c6"/></a>
  <a href="https://mockoon.com/"><img src="https://img.shields.io/badge/Website-Go-green.svg?style=flat-square&colorB=1997c6"/></a>
  <a href="http://eepurl.com/dskB2X"><img src="https://img.shields.io/badge/Newsletter-Subscribe-green.svg?style=flat-square"/></a>
  <a href="https://twitter.com/GetMockoon"><img src="https://img.shields.io/badge/Twitter_@GetMockoon-follow-blue.svg?style=flat-square&colorB=1da1f2"/></a>
  <a href="https://discord.gg/MutRpsY5gE"><img src="https://img.shields.io/badge/Discord-go-blue.svg?style=flat-square&colorA=6c84d9&colorB=1da1f2"/></a>
  <br>
  <a href="https://www.npmjs.com/package/@mockoon/cli"><img src="https://img.shields.io/npm/v/@mockoon/cli.svg?style=flat-square&colorB=cb3837"/></a>
  <br>
  <br>
  <h1>@Mockoon/cli</h1>
</div>

Welcome to Mockoon's official CLI, a lightweight and fast NPM package to deploy your mock APIs anywhere.
Feed it with a Mockoon's [export file](https://mockoon.com/docs/latest/import-export-data/), and you are good to go.

The CLI supports the same features as the main application: [templating system](https://mockoon.com/docs/latest/templating/overview/), [proxy mode](https://mockoon.com/docs/latest/proxy-mode/), [route response rules](https://mockoon.com/docs/latest/route-responses/dynamic-rules/), etc.

![Mockoon CLI screenshot](./docs/screenshot.png)

- [Installation](#installation)
- [Export your mock to use in the CLI](#export-your-mock-to-use-in-the-cli)
- [Compatibility](#compatibility)
- [Commands](#commands)
  - [`mockoon-cli start`](#mockoon-cli-start)
  - [`mockoon-cli list [ID]`](#mockoon-cli-list-id)
  - [`mockoon-cli stop [ID]`](#mockoon-cli-stop-id)
  - [`mockoon-cli dockerize`](#mockoon-cli-dockerize)
  - [`mockoon-cli help [COMMAND]`](#mockoon-cli-help-command)
- [Docker](#docker)
  - [Using the generic Docker image](#using-the-generic-docker-image)
  - [Using the `dockerize` command](#using-the-dockerize-command)
- [Logs](#logs)
- [PM2](#pm2)
- [Mockoon's documentation](#mockoons-documentation)
- [Sponsors](#sponsors)
- [Support/feedback](#supportfeedback)
- [Contributing](#contributing)
- [Roadmap](#roadmap)

## Installation

```sh-session
$ npm install -g @mockoon/cli
```

Usage:

```sh-session
$ mockoon-cli COMMAND
```

## Export your mock to use in the CLI

The CLI is compatible with Mockoon export files starting from version 1.7.0.

To export your environment(s) to use them in the CLI, follow these steps:

1. Open the `Import/export` application menu and choose `Mockoon's format -> Export all environments to a file (JSON)` or `Mockoon's format -> Export current environment to a file (JSON)`.

   ![Export to a file](/docs/export-to-file.png)

   > You can also right-click on one of the environments and select `Copy to clipboard (JSON)`. You will then have to manually create a file and paste the environment's JSON data.

   ![Export to a file](/docs/export-to-clipboard.png)

2. Choose a folder to save the JSON file.
3. Use the [start command](#mockoon-cli-start) below to launch your mock APIs with the CLI:

   ```sh-sessions
   $ mockoon-cli start --data ~/path/to/your-file.json -i 0
   ```

You will find more details in the [official documentation](https://mockoon.com/docs/latest/import-export-data/).

## Compatibility

### Node.js

Mockoon's CLI has been tested on Node.js versions 10, 12, 14, and 15.

### Mockoon data files

The CLI currently supports only data files exported using the [main application](https://mockoon.com) in Mockoon's own format.
The CLI can import and migrate data from older versions of Mockoon. If you exported the data file with a more recent version of the application, you may need to update your CLI with the following command: `npm install -g @mockoon/cli`.

## Commands

- [`mockoon-cli start`](#mockoon-cli-start)
- [`mockoon-cli list [ID]`](#mockoon-cli-list-id)
- [`mockoon-cli stop [ID]`](#mockoon-cli-stop-id)
- [`mockoon-cli dockerize`](#mockoon-cli-dockerize)
- [`mockoon-cli help [COMMAND]`](#mockoon-cli-help-command)

### `mockoon-cli start`

Starts a mock API from a Mockoon's export file environment. As an export file can contain multiple environments, you can indicate the one you want to run by specifying its index in the file or its name.
The process will be created by default with the name and port of the Mockoon's environment. You can override these values by using the `--port` and `--pname` flags.

```
USAGE
  $ mockoon-cli start

OPTIONS
  -a, --all          Run all environments
  -d, --data=data    (required) Path or URL to your Mockoon data export file
  -i, --index=index  Environment's index in the data file
  -n, --name=name    Environment name in the data file
  -N, --pname=pname    Override process name
  -p, --port=port    Override environment's port
  -l, --hostname=0.0.0.0    Override default listening hostname (0.0.0.0)
  -h, --help         show CLI help

EXAMPLES
  $ mockoon-cli start --data ~/export-data.json
  $ mockoon-cli start --data ~/export-data.json --index 0
  $ mockoon-cli start --data https://file-server/export-data.json --index 0
  $ mockoon-cli start --data ~/export-data.json --name "Mock environment"
  $ mockoon-cli start --data ~/export-data.json --name "Mock environment" --pname "proc1"
  $ mockoon-cli start --data ~/export-data.json --all
```

### `mockoon-cli list [ID]`

_Command alias: `info`_

Lists all the running mock APIs and display some information: process name, pid, status, cpu, memory, port.
You can also get the same information for a specific mock API by providing its pid or name.

```
USAGE
  $ mockoon-cli list

ARGUMENTS
  ID  Running API pid or name

OPTIONS
  -h, --help  show CLI help

EXAMPLE
  $ mockoon-cli list
  $ mockoon-cli info
  $ mockoon-cli list 0
  $ mockoon-cli list "Mock_environment"
```

### `mockoon-cli stop [ID]`

Stops one or more running processes. When 'all' is provided, all processes will be stopped.

```
USAGE
  $ mockoon-cli stop [ID]

ARGUMENTS
  ID  Running API pid or name

OPTIONS
  -h, --help  show CLI help

EXAMPLE
  $ mockoon-cli stop
  $ mockoon-cli stop 0
  $ mockoon-cli stop "name"
  $ mockoon-cli stop "all"
```

### `mockoon-cli dockerize`

Generates a Dockerfile used to build a self-contained image of a mock API. After building the image, no additional parameters will be needed when running the container.
This command takes similar flags as the [`start` command](#mockoon-start).

Please note that this command will extract your Mockoon environment from the export file you provide and put it side by side with the generated Dockerfile. Both files are required in order to build the image.

For more information on how to build the image: [Using the dockerize command](#using-the-dockerize-command)

```
USAGE
  $ mockoon-cli dockerize

OPTIONS
  -d, --data=data    (required) Path or URL to your Mockoon data export file
  -i, --index=index  Environment's index in the data file
  -n, --name=name    Environment name in the data file
  -p, --port=port    Override environment's port
  -o, --output       Generated Dockerfile path and name (e.g. `./Dockerfile`)
  -h, --help         show CLI help

EXAMPLES
  $ mockoon-cli dockerize --data ~/export-data.json --output ./Dockerfile
  $ mockoon-cli dockerize --data ~/export-data.json --index 0 --output ./Dockerfile
  $ mockoon-cli dockerize --data https://file-server/export-data.json --index 0 --output ./Dockerfile
  $ mockoon-cli dockerize --data ~/export-data.json --name "Mock environment" --output ./Dockerfile
```

### `mockoon-cli help [COMMAND]`

Returns information about a command.

```
USAGE
  $ mockoon-cli help [COMMAND]

ARGUMENTS
  COMMAND  command to show help for

OPTIONS
  --all  see all commands in CLI
```

## Docker

### Using the generic Docker image

A generic Docker image is published on the [Docker Hub Mockoon CLI repository](https://hub.docker.com/r/mockoon/cli). It uses `node:14-alpine` and installs the latest version of Mockoon CLI.

All of `mockoon-cli start` flags (`--port`, `--index`, etc.) must be provided when running the container.

To load the Mockoon data, you can either mount a local data file and pass `mockoon-cli start` flags at the end of the command:

`docker run -d --mount type=bind,source=/home/your-data-file.json,target=/data,readonly -p 3000:3000 mockoon/cli:latest -d data -i 0 -p 3000`

Or directly pass a URL to the `mockoon-cli start` command, without mounting a local data file:

`docker run -d -p 3000:3000 mockoon/cli:latest -d https://raw.githubusercontent.com/mockoon/mock-samples/main/samples/generate-mock-data.json -i 0 -p 3000`

### Using the `dockerize` command

You can use the [`dockerize` command](#mockoon-cli-dockerize) to generate a new Dockerfile that will allow you to build a self-contained image. Thus, no Mockoon CLI specific parameters will be needed when running the container.

- Run the `dockerize` command:

  `mockoon-cli dockerize --data ./sample-data.json --port 3000 --index 0 --output ./tmp/Dockerfile`

- navigate to the `tmp` folder, where the Dockerfile has been generated:

  `cd tmp`

- Build the image:

  `docker build -t mockoon-mock1 .`

- Run the container:

  `docker run -d -p <host_port>:3000 mockoon-mock1`

## Logs

Logs are located in `~/.mockoon-cli/logs/{mock-name}-[error|out].log`.

The `error.log` file contains mostly server errors that occur at startup time and prevent the mock API to run (port already in use, etc.). They shouldn't occur that often.

The `out.log` file contains all other log entries (all levels) produced by the running mock server. Most of the errors occurring in Mockoon CLI (or the main application) are not critical and therefore considered as normal output. As an example, if the JSON body from an entering request is erroneous, Mockoon will log a JSON parsing error, but it won't block the normal execution of the application.

## PM2

Mockoon CLI uses [PM2](https://pm2.keymetrics.io/) to start, stop or list the running mock APIs. Therefore, you can directly use PM2 commands to manage the processes.

## Mockoon's documentation

You will find Mockoon's [documentation](https://mockoon.com/docs/latest) on the official website. It covers the most complex features.

## Sponsors

If you like Mockoon, you can support the project with a one-time donation:
[![Paypal](https://www.paypalobjects.com/webstatic/mktg/Logo/pp-logo-100px.png)](https://paypal.me/255kb) <a href="https://www.buymeacoffee.com/255kb" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/white_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>

You can also [sponsor the maintainer (255kb) on GitHub](https://github.com/sponsors/255kb) and join all the [Sponsors and Backers](https://github.com/mockoon/mockoon/blob/main/backers.md) who helped this project over time!

## Support/feedback

You can discuss all things related to Mockoon's CLI, and ask for help, on the [official community](https://github.com/mockoon/cli/discussions). It's also a good place to discuss bugs and feature requests before opening an issue on this repository. For more chat-like discussions, you can also join our [Discord server](https://discord.gg/MutRpsY5gE).

## Contributing

If you are interested in contributing to Mockoon, please take a look at the [contributing guidelines](https://github.com/mockoon/cli/blob/main/CONTRIBUTING.md).

Please also take a look at our [Code of Conduct](https://github.com/mockoon/cli/blob/main/CODE_OF_CONDUCT.md).

## Roadmap

If you want to know what will be coming in the next release you can check the global [Roadmap](https://github.com/orgs/mockoon/projects/2).

New releases will be announced on Mockoon's [Twitter account @GetMockoon](https://twitter.com/GetMockoon) and through the newsletter to which you can subscribe [here](http://eepurl.com/dskB2X).
