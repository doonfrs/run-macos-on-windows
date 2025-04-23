# Install & Run Macos on Ubuntu or Windows ( WSL )
## Install 
```bash
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e GENERATE_UNIQUE=true \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom-sonoma.plist' \
    -e SHORTNAME=sequoia \
    sickcodes/docker-osx:latest
```

### setup
  * format the desk with 200+ GB
  * install macos ( it will take time ) 

## avoid re-install every time
when you run the command in the Install section, this will reset your image everytime, so let's avoid that
* shutdown macos
* search for the image
```bash
sudo find /var/lib/docker -size +10G | grep mac_hdd_ng.img
```
* move the img file to a good location
```bash
mv /var/lib/.../mac_hdd_ng.img .
```

* Generate unique
```bash
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -p 5999:5999 \
    -e RAM=8 -e CPU_STRING=16 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e GENERATE_UNIQUE=true \
    -e GENERATE_SPECIFIC=true \
    -e DEVICE_MODEL="iMacPro1,1" \
    -v "${PWD}/output.env:/env" \
    -v "${PWD}/mac_hdd_ng.img:/image" \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e EXTRA="-display none -vnc 0.0.0.0:99" \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e SHORTNAME=sequoia \
    dickhub/docker-osx:naked
```
if you get the error:
qemu-system-x86_64: -device vmxnet3,netdev=net0,id=net0,mac=:49:1C:8E: Property 'vmxnet3.mac' doesn't take value ':49:1C:8E'
open the file output.env, and generate any mac online, then set the mac address in the file and save it

* run macos using this image, this will not reset the os
* I added 8 gb ram & cpu config and set the display to none, and ssh.
* for ssh, enable remote login from macos, then from the host ssh -p 50922 localhost

```bash
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -p 5999:5999 \
    -e RAM=8 -e CPU_STRING=16 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e DEVICE_MODEL="iMacPro1,1" \
    -v "${PWD}/output.env:/env" \
    -v "${PWD}/mac_hdd_ng.img:/image" \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e EXTRA="-display none -vnc 0.0.0.0:99" \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e SHORTNAME=sequoia \
    dickhub/docker-osx:naked
```

* Enable VNC and disable display, this will keep it running in the background
* use vnc and connect to localhost:5999, no password required
* Use it for local machine development, do not share this on an online machine unless you know what you are doing 
* for ssh, enable remote login from macos, then from the host ssh -p 50922 localhost

```bash
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -p 5999:5999 \
    -e RAM=8 -e CPU_STRING=16 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v "${PWD}/mac_hdd_ng.img:/image" \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e EXTRA="-display none -vnc 0.0.0.0:99" \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom-sonoma.plist' \
    -e SHORTNAME=sequoia \
    dickhub/docker-osx:naked
```

## Start
you do not need to call docker run everytime, only you need to start the container
```bash
docker start macos
```
