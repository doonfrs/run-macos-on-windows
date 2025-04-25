# Install & Run Macos on Ubuntu or Windows ( WSL )
## Install 
```bash
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -p 5999:5999 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e RAM=10 -e CPU_STRING=16 \
    -e GENERATE_UNIQUE=true \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom-sonoma.plist' \
    -e SHORTNAME=sonoma \
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

* Now let's use the image and vnc instead of the default display  
```bash
# Remove the current macos !! carefull to move the image as mentioned in the above step
# if you do not move it, you will lose your installation
# docker rm macos

# Run the docker command
docker run -it \
    --device /dev/kvm \
    --name macos \
    -p 50922:10022 \
    -p 5999:5999 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e RAM=10 -e CPU_STRING=16 \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -v "${PWD}/mac_hdd_ng.img:/image" \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e EXTRA="-display none -vnc 0.0.0.0:99" \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/refs/heads/sequoia/config-custom-sequoia.plist' \
    -e SHORTNAME=sonoma \
    dickhub/docker-osx:naked
```

* Connection:
- vnc: localhost:5999
- ssh: localhost -p 50922



## Start
you do not need to call docker run everytime, only you need to start the container
```bash
docker start macos
```
