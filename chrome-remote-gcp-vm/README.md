# Development/Testing VM on GCP

Based on this solution: https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine.

### Create VM (setup script takes ± 4min to install)

Set the config

```sh
INSTANCE_NAME=my-remote-vm
VM_TYPE=e2-medium
ZONE=us-east1-b
DISK_SIZE=30GB
NETWORK=default
SUBNET=default
```

With a public IP

```sh
gcloud compute instances create $INSTANCE_NAME \
  --zone=$ZONE \
  --boot-disk-size=$DISK_SIZE \
  --machine-type=$VM_TYPE \
  --zone=$ZONE \
  --network=$NETWORK --subnet=$SUBNET \
  --metadata-from-file startup-script=./startup.sh
```

Without Public IP (requires Cloud NAT for egress internet traffic)

```sh
gcloud compute instances create $INSTANCE_NAME \
  --zone=$ZONE \
  --boot-disk-size=$DISK_SIZE \
  --machine-type=$VM_TYPE \
  --zone=$ZONE \
  --no-address --network=$NETWORK --subnet=$SUBNET \
  --metadata-from-file startup-script=./startup.sh
```

Set ownership:

```sh
gcloud compute ssh $INSTANCE_NAME --zone $ZONE --command 'sudo usermod -a -G chrome-remote-desktop $USER'
```

## Setup CRD (Chrome Remote Desktop)

1. Open https://remotedesktop.google.com/headless
2. SSH into VM and paste connection command
   ```sh
   gcloud compute ssh $INSTANCE_NAME --zone $ZONE
   ```
3. Set pin
4. Open https://remotedesktop.google.com and connect to VM

## Delete afterwards

```sh
gcloud compute instances create $INSTANCE_NAME \
  --zone=$ZONE 
```
