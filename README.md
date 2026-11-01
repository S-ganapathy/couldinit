# Automated Cloud-Init Server Provisioning

This project provisions an automated Ubuntu 24.04 server locally using `cloud-init` and `QEMU` with zero manual intervention.

## Prerequisites
- QEMU (`qemu-system-x86_64`)
- Cloud Image Utilities (`cloud-localds`)
- KVM / Hardware Virtualization enabled

## How to Run

1. Generate the cloud-init seed ISO:
   `ash
   cloud-localds seed.iso cloud-init.yaml
Download and resize the base Ubuntu 24.04 Cloud Image:

Bash
wget -O noble-server.img https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
qemu-img resize noble-server.img 10G
Launch the virtual machine:

Bash
qemu-system-x86_64 -enable-kvm -m 2048 -smp 2 \
  -drive file=noble-server.img,format=qcow2 \
  -drive file=seed.iso,media=cdrom \
  -net nic -net user,hostfwd=tcp::8080-:8080,hostfwd=tcp::2222-:22 \
  -nographic
Verification
Wait approximately 60–90 seconds for initial provisioning to complete. Test the HTTP service from the host machine:

Bash
curl http://127.0.0.1:8080
Expected output:

Plaintext
"Hello from Server!"
