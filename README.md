# raspberry-pg

First run `$ docker volume create pgdata`.

Be sure the Docker Compose CLI is installed, easiest way to do that is `$ pip install docker-compose`

To allow incoming connections from the local network (thank you Giuseppe Broccolo @ [RaspberryPG](http://raspberrypg.org/index.html@p=53.html))
```shell
$ sudo iptables -A INPUT -i eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
$ sudo iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT                                                                                                                    $ sudo iptables -A OUTPUT -o eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
$ sudo iptables -A INPUT -i eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT 
```

and edit the `/etc/hosts` file to include:
```
[Your IP Address on your Network]    0.0.0.0:5432
...
```

Then either reboot the PI (`$ sudo reboot`) or something like:
```shell
sudo ifdown wlan0 \
  && sleep 30 \
  && sudo ifup wlan0
```

Finally, from the root of this repository run `$ docker-compose up -d`.

Establish a TCP connection to the database at `postgres://postgres@<Raspberry PI's IP Address (10.0.1.XX)>:5432`.
