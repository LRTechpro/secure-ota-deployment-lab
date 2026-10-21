# Docker Delivery Workflow

Place `docker/Dockerfile` in the root of the separately obtained Secure OTA source folder before running these commands.

## Build the image

```powershell
docker build -t secureota:v1 .
```

The tag identifies the application as `secureota` and records this delivery as version `v1`.

## Run the container

```powershell
docker run -d --name secureota-container -p 8081:80 secureota:v1
```

Open `http://localhost:8081`.

## Verify the delivery

```powershell
docker ps
docker inspect --format='{{json .State.Health}}' secureota-container
```

Test both application paths:

1. Restore an original update, calculate SHA-256, and confirm that matching hashes approve it.
2. Simulate a one-bit modification, calculate SHA-256 again, and confirm that the mismatch rejects it.

## Export for transfer

```powershell
docker save -o secureota-v1.tar secureota:v1
```

## Load a transferred image

```powershell
docker load -i .\secureota-v1.tar
docker image ls
```
