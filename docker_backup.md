# Backup

```shell
#!/bin/bash
if [ "x$1" = 'x' ]; then
  echo 'Usage:' >&2
  echo '  dvol_backup.sh BACKED_VOLUME_NAME' >&2
  echo '  dvol_backup.sh BACKED_DIR BACKUP_NAME' >&2
  echo '  dvol_backup.sh $(pwd)/DIR' >&2
  exit 1
fi
if [ "$(id -u)" -ne 0 ]; then
  echo 'This script must be run with sudo' >&2
  exit 1
fi
BUP_FNAME=$(basename $1)
[ "x$2" != 'x' ] && BUP_FNAME=$2
BUP_FNAME=$BUP_FNAME.$(date +"%Y-%m-%d_%H%M%S").tar
docker run --rm \
  -v $1:/source:ro \
  -v $(pwd):/backup:rw \
  ubuntu \
  tar cvf /backup/$BUP_FNAME /source
chown shimajda:shimajda $BUP_FNAME
echo Backup ready: $BUP_FNAME
```

# Restore

```shell
#!/bin/bash
if [ "x$1" = 'x' ] || [ "x$2" = 'x' ]; then
  echo 'Usage:' >&2
  echo '  dvol_restore.sh RESTORED_VOLUME_NAME PWD_BACKUP.tar' >&2
  echo '  dvol_restore.sh /RESTORED/FULL/PATH PWD_BACKUP.tar' >&2
  echo '  dvol_restore.sh $(pwd)/IN/CURRENT/DIR PWD_BACKUP.tar' >&2
  exit 1
fi
if [ "$(id -u)" -ne 0 ]; then
  echo 'This script must be run with sudo' >&2
  exit 1
fi
docker run --rm \
  -v $1:/target:rw \
  -v $(pwd):/backup:ro \
  ubuntu \
  bash -c "cd /target && rm -rf * && tar xvf /backup/$2 --strip 1"
```
