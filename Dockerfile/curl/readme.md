# [sshpass](https://linux.die.net/man/1/sshpass)

## Docker image

+ Build

```
docker build -t curl .
```

+ Execute

```
docker run -ti --rm curl [operation] [URLs]
```

+ Mount download directory

```
docker run -ti --rm -v %cd%:/root curl [operation] [URLs] -o /download/<filename>       // Windows
docker run -ti --rm -v ${PWD}:/root curl [operation] [URLs] -o /download/<filename>     // Linux
```
