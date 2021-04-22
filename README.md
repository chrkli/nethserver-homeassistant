# nethserver-homeassistant

**Forked from stephdl/nethserver-pihole and adopted to install, manage and update homeassistant docker container**

# Warning
**Development in progress, at the moment there is no way to easily install**

# ToDo
- adopt the work of stephdl for pihole docker container to manage homeassistant docker container
- incorporate homeassistant docker env variables
- test on fresh nethserver installation
- documentation
- create RPM so that following will work ```yum install nethserver-homeassistant```

# initial configuration & maintenance
- Create a bridge with the network panel or use `br0` (created by nethserver-dc), it is needed by aeria or macvlan.
- Decide if you want to use aeria (**experimental**) or macvlan (**Recommended**)

For aeria (**experimental**):
```
config setprop docker bridgeAeria br0
signal-event nethserver-docker-update
```
- check aeria is up : `docker network ls`

- then assign `aeria` to hassNetwork

`config setprop homeassistant hassNetwork aeria`

For macvlan (**Recommended**) 

check for explanations: https://github.com/NethServer/nethserver-docker/blob/master/README.rst#macvlan

```
config setprop docker macVlanGateway 192.168.1.1 macVlanLocalNetwork 192.168.1.0/24 macVlanNetwork 192.168.1.224/27 macVlanNic br0
signal-event nethserver-docker-update
```

- check macvlan is up : `docker network ls`
- then assign `macvlan` to hassCoreNetwork and set the IP to `hassMacVlanIP` (in macvlan range)

`config setprop hassCore hassNetwork macvlan hassMacVlanIP 192.168.1.234`

review the pihole conf : `config show hassCore`
```
hassCore=configuration
    mac=00:60:2f:0a:66:06    # once generated, it is static mac
    password=admin  #web admin password
    timezone=UTC
```
- change the admin password (default is `admin`)

`config setprop hassCore password azertyuiop`

- `signal-event nethserver-homeassistant-update`
- The time depends of your internet bandwith
- check docker homeassistant is up : `docker ps`

homeassistant facilities wrapper to docker command

    homeassistant ip : find the IP of pihole given by your dhcp server for aeria network
    homeassistant status : retrieve the status of pihole container
    homeassistant env : retrieve all the environment vars of the container
    homeassistant bash : start a shell inside the container
    homeassistant start: Start the pihole container
    homeassistant stop: Stop the pihole container
    homeassistant destroy: Delete the pihole container
    homeassistant build: Delete then create the pihole container
    homeassistant ps: Container information
    homeassistant log: Display the error log of the container

go to the IP of homeassistant port 8123, the admin page is up.
