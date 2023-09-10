# Share Data between Docker Containers

## Step 1 — Creating an Independent Volume

```bash
docker volume create --name DataVolume1
```

```bash
docker run -it --rm -v DataVolume1:/datavolume1 ubuntu

# --rm deletes the container when we exit
# -v DataVolume1:/datavolume1 mounts the volume
```

Verify that the volume is present on our system with docker volume inspect:

```bash
$ docker volume inspect DataVolume1

[
    {
        "CreatedAt": "2018-07-11T16:57:54Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/DataVolume1/_data",
        "Name": "DataVolume1",
        "Options": {},
        "Scope": "local"
    }
]
```

## Step 2 — Creating a Volume that Persists when the Container is Removed

```bash
docker run -ti --name=Container2 -v DataVolume2:/datavolume2 ubuntu
```

> Note: The `-v` flag is very flexible. It can bindmount or name a volume with just a slight adjustment in syntax. If the first argument begins with a `/` or `~/` you’re creating a bindmount. Remove that, and you’re naming the volume.
>
>`-v /path:/path/in/container` mounts the host directory, `/path` at the `/path/in/container`
>
>`-v path:/path/in/container` creates a volume named `path` with no relationship to the host

When we restart the container, the volume will mount automatically:

```bash
docker start -ai Container2
```

Step 3 — Sharing Data Between Multiple Docker Containers


