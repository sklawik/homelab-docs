# homelab-docs
Initial thoughts with tools and devices

Devices: 
1. Raspberry Pi 4 (1 ethernet, 2xUSB 2.0 2xUSB 3.0, WiFi, Bluetooth),
2. 4G ZTE USB Modem with built-in router
3. MacBook M1 (Asahi Linux, USB-C including support for NTFS, ext4 disks since it runs Linux)
4. Ubuntu Desktop PC x64/86 on AM4 with AMD CPU & GPU
5. NTFS HDD (USB)
6. ext4 SSD (USB)


Thinking proces:

First problem with solution: 
Set up Macbook M1 with Asahi Linux. Made backup using wi-fi transfer since MacOS doesn't support external disk drives as ext4 and NTFS ones I have. Used tool copyparty.
Asahi Linux installed and completed basic configuration along with connection thru ssh from the ubuntu desktop.

Basic commands to manage pods, services
kubectl get pods
kubectl get services or kubectl get svc
while learning kubernetes the complete basic is service api and i found it myself using kubectl get services under running name
"kubernates" which has port of 443 and going to that page shows unathorized access message for the api.


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