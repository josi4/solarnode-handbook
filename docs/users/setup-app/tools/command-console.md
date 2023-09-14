# Command Console

SolarNode includes a Command Console page where troubleshooting commands from supporting plugins are
displayed. The page shows a list of available command topics and lets you toggle the inclusion of
each topic's commands at the bottom of the page.

![SolarNode Command Console screen](../../../images/users/setup/setup-command-console%402x.png){width=889}

## Modbus Commands

The [Modbus TCP Connection][nifty-tcp-con] and [Modbus Serial Connection][nifty-serial-con]
components support publishing [mbpoll][mbpoll] commands under a **modbus** command topic. The `mbpoll`
utility is included in SolarNodeOS; if not already installed you can install it by logging in to the
[SolarNodeOS shell](../../sysadmin/index.md) and running the following command:

```sh
sudo apt install mbpoll
```

Modbus command logging must be enabled on each Modbus Connection component by toggling the
**CLI Publishing** setting on.

![SolarNode Modbus Connection CLI Publishing setting](../../../images/users/setup/setup-modbus-connection-cli-publishing@2x.png){width=660}

Once CLI Publishing has been enabled, every Modbus request made on that connection will generate an equivalent
`mbpoll` command, and those commands will be shown on the Command Console.

![SolarNode Command Console Modbus commands](../../../images/users/setup/setup-command-console-modbus%402x.png){width=741}

You can copy any logged command and paste that into a SolarNodeOS shell to execute the Modbus
request and see the results.

```sh
mbpoll -g -0 -1 -m rtu -b 4800 -s 1 -P none -a 1 -o 5 -t 4:hex -r 0 -c 2 /dev/tty.usbserial-FTYS9FWO
-- Polling slave 1...
[0000]: 	0x00FC
[0001]: 	0x0A1F
```

[mbpoll]: https://github.com/epsilonrt/mbpoll
[nifty-serial-con]: https://github.com/SolarNetwork/solarnetwork-node/tree/develop/net.solarnetwork.node.io.modbus.nifty.jsc
[nifty-tcp-con]: https://github.com/SolarNetwork/solarnetwork-node/tree/develop/net.solarnetwork.node.io.modbus.nifty
