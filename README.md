<p align="center">
  <a href="https://directus.io" target="_blank" rel="noopener noreferrer">
    <img src="https://user-images.githubusercontent.com/522079/43096167-3a1b1118-8e86-11e8-9fb2-7b4e3b1368bc.png" width="140" alt="Directus Logo"/>
  </a>
</p>

<h1 align="center">
  Directus Docker
</h1>

<h3 align="center">
  <a href="https://directus.io">Website</a> •
  <a href="https://docs.directus.io">Docs</a> •
  <a href="https://docs.directus.io/api/reference.html">API Reference</a> •
  <a href="https://docs.directus.io/app/user-guide.html">User Guide</a> •
  <a href="https://directus.app">Demo</a> •
  <a href="https://docs.directus.io/supporting-directus.html">Contribute</a>
</h3>

<p>&nbsp;</p>

> **Warning**: docker support is experimental and we're still gathering some more feedback from the community. You can raise issues or ping us in #docker channel on [slack](https://slack.getdirectus.com/).

## Overview

Directus docker images can be found in [docker hub](https://hub.docker.com/r/directus/) under `directus` username. Images are pushed automatically each time a new release is created in [api](https://github.com/directus/api) or [app](https://github.com/directus/app) repositories.

## API Container

### Requirements

- MySQL compatible database container running

### Configuration

API can be configured via environment variables. These are some of the supported variables.

### General variables

<table>
  <thead>
    <tr>
      <th>Environment</th>
      <th>Required</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>APP_TIMEZONE</td>
      <td>&nbsp;</td>
      <td>America/New_York</td>
      <td>The API server timezone</td>
    </tr>
  </tbody>
<table>

### Authentication variables

<table>
  <thead>
    <tr>
      <th>Environment</th>
      <th>Required</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ADMIN_EMAIL</td>
      <td>Yes*</td>
      <td>&nbsp;</td>
      <td>The admin email</td>
    </tr>
    <tr>
      <td>ADMIN_PASSWORD</td>
      <td>&nbsp;</td>
      <td>generated**</td>
      <td>The admin password</td>
    </tr>
  </tbody>
<table>

> \* If there's no directus tables on the database, the auto-installation process will require you to provide the initial admin email, thus requiring the `ADMIN_EMAIL` variable in order to seed the initial user into the database.

> \*\* The installation process will not require the `ADMIN_PASSWORD` to be set, and if it doesn't detect it, it will generate a new password and output the credentials in the logs when it finishes seeding.

### Database variables

<table>
  <thead>
    <tr>
      <th>Environment</th>
      <th>Required</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DATABASE_HOST</td>
      <td>Yes</td>
      <td>&nbsp;</td>
      <td>The database hostname/address</td>
    </tr>
    <tr>
      <td>DATABASE_USERNAME</td>
      <td>Yes</td>
      <td>&nbsp;</td>
      <td>The database username</td>
    </tr>
    <tr>
      <td>DATABASE_PASSWORD</td>
      <td>Yes</td>
      <td>&nbsp;</td>
      <td>The database password</td>
    </tr>
    <tr>
      <td>DATABASE_PORT</td>
      <td>&nbsp;</td>
      <td>3306</td>
      <td>The database port</td>
    </tr>
    <tr>
      <td>DATABASE_NAME</td>
      <td>&nbsp;</td>
      <td>directus</td>
      <td>The database name</td>
    </tr>
    <tr>
      <td>DATABASE_TYPE</td>
      <td>&nbsp;</td>
      <td>mysql</td>
      <td>The database type</td>
    </tr>
    <tr>
      <td>DATABASE_ENGINE</td>
      <td>&nbsp;</td>
      <td>InnoDB</td>
      <td>The database engine</td>
    </tr>
    <tr>
      <td>DATABASE_CHARSET</td>
      <td>&nbsp;</td>
      <td>utfmb4</td>
      <td>The database charset</td>
    </tr>
  </tbody>
<table>

### Running

```
docker run \
  --link database:database \
  --env "DATABASE_HOST=database" \
  --env "DATABASE_PORT=3306" \
  --env "DATABASE_NAME=mydatabase" \
  --env "DATABASE_USERNAME=root" \
  --env "DATABASE_PASSWORD=root" \
  --env "ADMIN_EMAIL=email@email.com" \
  --publish 7000:80 \
  directus/api:latest
```

> The above example will run an `api` container and expose it on port 7000. To manage this instance you should also run an `app` container that points to this instance.


## API Container

### Configuration

API can be configured via environment variables. These are some of the supported variables.

### General variables

<table>
  <thead>
    <tr>
      <th>Environment</th>
      <th>Required</th>
      <th>Default</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>API_ENDPOINT<i>[_NAME]</i></td>
      <td>At least once</td>
      <td>&nbsp;</td>
      <td>
        The supported `app` endpoints.<br/>
        Should follow the format "Name; url"
      </td>
    </tr>
  </tbody>
<table>

You can manage multiple `api` endpoints using only one instance of `app` container. For example, if you want to manage two instances, you should set two variables that starts with `API_ENDPOINT_`

```
API_ENDPOINT_STAGING="Staging; http://staging.server.com/_/"
API_ENDPOINT_PRODUCTION="Production; http://production.server.com/_/"
```

### Running

```
docker run \
  --env API_ENDPOINT="Directus; http://localhost:7000/_/" \
  --publish 8000:80 \
  directus/app:latest
```

Assuming there's an `api` instance running on port 7000, you should now be able to manage it through `app` by accessing [localhost on port 8000](http://localhost:8000/).


## Examples

You can check examples under the [examples folder](https://github.com/directus/docker/tree/master/examples) on GitHub.

<p>&nbsp;</p>

----

<p align="center">
  Directus is released under the <a href="http://www.gnu.org/copyleft/gpl.html">GPLv3</a> license. <a href="http://rangerstudio.com">RANGER Studio LLC</a> owns all Directus trademarks and logos on behalf of our project's community. Copyright © 2006-2018, <a href="http://rangerstudio.com">RANGER Studio LLC</a>.
</p>
