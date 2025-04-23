# Install & Run Macos on Ubuntu or Windows ( WSL )
## Install 
```bash
touch mac.env
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -p 5999:5999 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e RAM=8 -e CPU_STRING=16 \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -v "${PWD}/mac.env:/env" \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e GENERATE_UNIQUE=true \
    -e GENERATE_SPECIFIC=true \
    -e DEVICE_MODEL="iMacPro1,1" \
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
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e RAM=8 -e CPU_STRING=16 \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -v "${PWD}/mac.env:/env" \
    -v "${PWD}/mac_hdd_ng.img:/image" \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e DEVICE_MODEL="iMacPro1,1" \
    -e SHORTNAME=sequoia \
    dickhub/docker-osx:naked
```
## use vnc only 
if you want to disable the emulator screen and use vnc only add the line:
```bash
    -e EXTRA="-display none -vnc 0.0.0.0:99" \
```
to the prev line
( you may need to the container )


## Start
you do not need to call docker run everytime, only you need to start the container
```bash
docker start macos
```
