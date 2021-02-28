# Development/Testing VM on GCP

Adpted from this solution: https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine.

### Create VM

```sh
INSTANCE_NAME=my-remote-vm
VM_TYPE=f1-micro
ZONE=us-east1-b
DISK_SIZE=30GB

gcloud compute instances create $INSTANCE_NAME \
  --zone=$ZONE \
  --boot-disk-size=$DISK_SIZE \
  --machine-type=$VM_TYPE \
  --zone=$ZONE \
  --metadata-from-file startup-script=./startup.sh

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