# homelab-docs

Goal: Kubernetes cluster, running 24/7 using cellular network based on (tailscale/wireguard/cloudflare tunnels) with self-hosted popular services like navidrome, jellyfin or moonlight.


Initial thoughts with tools and devices



Devices: 
1. Raspberry Pi 4 (1 ethernet, 2xUSB 2.0 2xUSB 3.0, WiFi, Bluetooth),
2. 4G ZTE USB Modem with built-in router
3. MacBook M1 (Asahi Linux, USB-C including support for NTFS, ext4 disks since it runs Linux)
4. Ubuntu Desktop PC x64/86 on AM4 with AMD CPU & GPU
5. NTFS HDD (USB)
6. ext4 SSD (USB)
7. a VPS as a gateway because CGNAT doesn't give public static IP (in Poland there is one company that does this for a customer: OTVARTA very limited plan but you don't have to run a business to get it)
BTW. From what I got e-mailing them, they also setup reverse dns for self hosted e-mail purposes.


Thinking proces:

First problem with solution: 
Set up Macbook M1 with Asahi Linux. Made backup using wi-fi transfer since MacOS doesn't support external disk drives as ext4 and NTFS ones I have. Used tool copyparty.
Asahi Linux installed and completed basic configuration along with connection thru ssh from the ubuntu desktop.

Basic commands to manage pods, services
kubectl get pods
kubectl get services or kubectl get svc
while learning kubernetes the complete basic is service api and i found it myself using kubectl get services under running name
"kubernates" which has port of 443 and going to that page shows unathorized access message for the api.
kubectl scale nginx --replicas=2
kubectl create deployment NAME --image=IMAGE
kubectl expose deployment NAME --port=PORT
kubectl scale deployment nginx --replicas=REPLICAS_INTIGER

Additionally learned:
1. avk use with grep (or commands that take output of previous one with this symbol: |)
avk 'print $1'
and 
grep for exact string
grep "match"
or grep -i "Match" to match no matter lowercase
example command to get pods endpoints from service nginx if running the nginx:
kubectl describe service nginx | grep -i "endpoints" | awk '{print $2}'
why not double quotes for awk?
because awk takes single quotes so what you type you give to awk
and if double quotes the bash will try to make sense of $2 which will give awk just $2
example result: 10.42.0.103:80


Curiousity while learning:

Why are commands named like this and that, how to memorize those better?

1. command avk: it is named avk, because of its creators name, nothing to think about deeply

2. command svc used in kubectl is shortcut to service

3. k3s is wrapper, kubectl command alone works but it will also work while typing k3s first like: k3s kubectl get services or kubectl get services is same

4. Grep command - it is used 99% on Linux to use it after previous command output, but what if we want to use it alone? let's say we have file.txt in current directory. grep "target" ./file.txt . So the first parameter is target search and second file location which is not that intuitive because usually you type file location first and then next parameters.

5. while shame to admit kubernetes has so many commands it was much more convienent to use: history | grep "kubectl" and see previous commands to memorize them and understand what we were typing. I use notes and scroll thru console but it is much better now especially with "| tail -10" at the end.
Example: history | grep "kubectl" | tail -10
Gives: last 10 commands that we used in the terminal with kubectl
While complicating it all, a command: history 10
will do just fine.


Moreover, Over the last 24h...
Wifi from USB ZTE 4G modem with built-in wi-fi router was my default home network device.
But now changes has been made.
1. Raspbery Pi: Installed OpenWrt (Wrt stands for Wireless Router)
Download link: https://firmware-selector.openwrt.org/
2. Detected missing package that supports usb-modems: kmod-usb-net-cdc-ether and installed it using UI, CLI also avaliable: apk add kmod-usb-net-cdc-ether
3. Configured wi-fi for stable connections, masqueraded zte modem interface into rpi's interface that gives the connection using ethernet port to ubuntu desktop and wi-fi to nearby devices.
4. Disabled ZTE modem built-in wi-fi router, so RPI has internet only from cellular tower and only hands it to the Rpi thru USB.

Eliminated problems & learning curve:
 1. iOS devices using built-in zte modem wifi had forced private addressses (often changing) that couldn't detect properly home network services now when iOS devices connect to openWrt wi-fi, DHCP works flawless and sets up correct ip addresses.
2. Increased wi-fi stability
3. Enhanced overall control of home network (logs, configurations, etc) opening new possibilities

Media streaming via Ubuntu Desktop (to act as a nvidia shadowplay for video game streaming but self hosted)

Moonlight on iOS that is client to sunshine streaming server:
https://github.com/moonlight-stream/moonlight-ios

Moonlight server as AppImage:
https://github.com/LizardByte/Sunshine/releases

Enabled wake on lan in the network in ASRock BIOS.
Filled ip, mac address, port of the desktop pc to wake it up using mobile app that sends "magic packet" - Simple Wake on Lan for iOS by herzhenr
https://github.com/herzhenr/simple-wake-on-lan

Moonlight auto-configures mac address if you set it up by just clicking interface and not putting ipv4 address by yourself, so now I use moonlight to wake devices up.