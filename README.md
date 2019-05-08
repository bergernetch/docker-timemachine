mbentley/timemachine
====================

docker image to run netatalk (compatible Time Machine for OS X)
based off of debian:jessie

To pull this image:
`docker pull grizmin/timemachine`

Example docker-compose usage:
```
docker-compose -f timemachine-compose.yml up -d
```

Example usage:
```
docker run -d --restart=always \
  --net=host \
  --name timemachine \
  -e CUSTOM_AFP_CONF="false" \
  -e LOG_LEVEL="info" \
  -e MIMIC_MODEL="TimeCapsule6,106" \
  -e PASSWORD="timemachine" \
  -e SET_PERMISSIONS="false" \
  -e SHARE_NAME="TimeMachine" \
  -e VOLUME_SIZE_LIMIT="0" \
  -v /path/on/host/to/backup/to/for/timemachine:/opt/timemachine \
  -v timemachine-netatalk:/var/netatalk \
  -v timemachine-logs:/var/log/supervisor \
  mbentley/timemachine:latest
```

This works best with `--net=host` so that discovery can be broadcast.  Otherwise, just expose port 548 (`-p 548:548`) and then you must manually map the drive for it to show up.

If you're using an external volume like in the example above, you will need to set the filesystem permissions on disk.  By default, the `timemachine` user is `1000:1000`.

Default credentials:
  * Username: `timemachine`
  * Password: `timemachine`

Optional variables:
  * `CUSTOM_AFP_CONF` - (default - `false`) indicates that you are going to bind mount a custom config to `/etc/netatalk/afp.conf` if set to `true`
  * `LOG_LEVEL` - (default - `info`) sets the netatalk log level
  * `MIMIC_MODEL` - (default `TimeCapsule6,106`) sets the value of time machine to mimic
  * `PASSWORD` - (default - `timemachine`) sets the password for the `timemachine` user
  * `SET_PERMISSIONS` - (default `false`) set to `true` to have the entrypoint set ownership and permission on `/opt/timemachine`
  * `SHARE_NAME` - (default `TimeMachine`) sets the name of the timemachine share to TimeMachine by default
  * `VOLUME_SIZE_LIMIT` - (default - `0`) sets the maximum size of the time machine backup in MiB ([mebibyte](https://en.wikipedia.org/wiki/Mebibyte))

Thanks for [odarriba](https://github.com/odarriba) and [arve0](https://github.com/arve0) for their examples to start from.
