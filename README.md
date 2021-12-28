# Kustomize-7DTD
Orchestration for running a 7DTD server

Target Image: [vinanrra/7dtd-server](https://hub.docker.com/r/vinanrra/7dtd-server)

## Purpose
Application Objectives:
- Orchestration instructions via Kustomize for configuring and running a 7 Days to Die server in kubernetes.
Personal Objectives:
- Learn how to orchestrate application deployment with a tool other than Helm

## Getting Started

This orchestration is built of two `overlays`
- dev
    - Use this overlay to deploy an initial prototype and designate the orchestration you would like to commit.
- prod
    - Use this overlay to deploy a "stable" deployment while leaving `dev` for testing configurations and modifications.

### Deploy dev
Change to the dev directory (Linux Assumed)
- `cd overlays/dev`
Modify the `application.properties` file to suit your assumed 7DTD server configurations.
Create your namespace:
- `kubectl create ns dev-7dtd`
Check kustomize build
- `kubectl kustomize`
Apply the kustomize configuration
- `kubectl apply -k .`

### Deploy prod
Change to the prod directory (Linux Assumed)
- `cd overlays/prod`
Modify the `application.properties` file to suit your assumed 7DTD server configurations.
Create your namespace:
- `kubectl create ns prod-7dtd`
Check kustomize build
- `kubectl kustomize`
Apply the kustomize configuration
- `kubectl apply -k .`

## Server Parameters
See [vinanrra/7dtd-server](https://hub.docker.com/r/vinanrra/7dtd-server) for more information

*Pulled from the repo above
## Parameters

| Parameter | Function |
| :----: | --- |
| `/path/to/7DaysToDie:/home/sdtdserver/.local/share/7DaysToDie/` | 7DaysToDie saves, where maps are store. |
| `/path/to/ServerFiles:/home/sdtdserver/serverfiles/` | 7DaysToDie server config files. |
| `/path/to/Logs:/home/sdtdserver/log/` | 7DaysToDie server log files. |
| `/path/to/BackupFolder:/home/sdtdserver/lgsm/backup/` | 7DaysToDie server backups files. |
| `/path/to/LGSM-Config:/home/sdtdserver/lgsm/config-lgsm/sdtdserver/` | LGSM config files. [More info](https://docs.linuxgsm.com/commands/monitor) |
| `26900:26900/tcp` | Default 7DaysToDie port **required** |
| `26900:26900/udp` | Default 7DaysToDie port **required** |
| `26901:26901/udp` | Default 7DaysToDie port **required** |
| `26902:26902/udp` | Default 7DaysToDie port **required** |
| `8080:8080/tcp` | Default 7DaysToDie webadmin port **optional**, if you use webadmin remember to change password in */path/to/ServerFiles/sdtdserver.xml* |
| `8081:8081/tcp` | Default 7DaysToDie telnet port **optional** |
| `8082:8082/tcp` | Default [Alloc Fixes Map GUI](https://7dtd.illy.bz/wiki/Server%20fixes) webserver port **optional** |
| `START_MODE=1` | Start mode of the container - see below for explanation **required** |
| `VERSION=stable` | Change between 7 days to die versions [more info](https://steamcommunity.com/app/251570/discussions/0/2570942124844173383/) **optional** |
| `TEST_ALERT=YES` | Test alerts at start of server **optional** |
| `ALLOC_FIXES=YES` | Install/Update [Alloc Fixes](https://7dtd.illy.bz/wiki/Server%20fixes), ONLY USE WITH LATEST STABLE BUILD **optional** |
| `BACKUP=YES` | Backup server at 5 AM (Only the latest 5 backups will be keep, maximum 30 days) [More info](https://docs.linuxgsm.com/commands/backup) **optional** |
| `MONITOR=NO` | Monitor server status, if server crash this will restart it [More info](https://docs.linuxgsm.com/commands/monitor), if map is not alread generate will give error read [#47](https://github.com/vinanrra/Docker-7DaysToDie/issues/47) **optional** |
| `PUID=1000` | for UserID - see below for explanation |
| `PGID=1000` | for GroupID - see below for explanation |
| `TimeZone=Europe/Madrid` | for TimeZone - see [TZ Database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) for time zones **recomendable**|
| `--restart unless-stopped` | Restart container always unlesss stopped manually **NEVER USE WITH START_MODE=4** |