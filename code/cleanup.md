Cleanup:
```bash
docker container rm -f $(docker container ls -aq)
```

Reclaim disk space:
```bash
docker image rm -f $(docker image ls -f reference='diamol/*' -q)
```

