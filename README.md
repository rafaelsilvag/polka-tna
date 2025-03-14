[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

**This is still a work in progress. Feedback is welcome!**

## Requirements

- git 
- python3
- matplotlib
- networkX
- regex
- polka-routing

**Necessary to run compile, run and set P4 code**

 - Stratum. *Please refer to the official building guide (https://github.com/stratum/stratum/blob/main/stratum/hal/bin/barefoot/README.build.md)*
 - P4Studio. *Tested with SDE 9.5+*

### Compile P4 Code and Run Tofino Switch

In **Terminal 1**, compile the P4 code using the official P4 compiler.

If you are using P4 Studio, follow the next steps to compile the P4 codes:

```
cd bf-sde-9.13.2
```
```
. ../tools/set_sde.bash
```
Compile PolKA's p4 code

```
../tools/p4_build.sh ~/polka-tna/p4src/polka.p4
```

Run the switch:

```
./run_switchd.sh -p polka -c ~/polka-tna/p4src/multiprogram_custom_bfrt.conf
```

### Load the Tables and Set Ports

In **Terminal 2**:
```
cd bf-sde-9.13.2
```
```
. ../tools/set_sde.bash
```
Load table information:
```
bfshell -b ~/polka-tna/controlplane/bfrt.py
```
Configure ports:
```
bfshell -f ~/polka-tna/controlplane/ports_config.txt -i
```

### Send Traffic

In **Terminal A**, access host 1.

```
sudo ifconfig enp3s0f0.1920 192.168.0.10 up
```

In **Terminal B**, access host 2.

```
sudo ifconfig enp6s0f0.1920 192.168.0.20 up

```

To validate the route that PolKa is using to reach the destination.

The file [calculator](https://docs.google.com/spreadsheets/d/19dWWfbyr4qZv1m4FIzHOO8c77znra906RL7u58ySk80/edit?usp=sharing) shows the information of the tables and the result of the TTL when it reaches the destination.

For example, a ping from H1 to H2:

```
ping 192.168.0.20
```

### Slice

To validate a specific route, use hping3. Then, you can specify the TCP port

```
sudo hping3 192.168.0.20 -p 8080

## Contributing
PRs are very much appreciated. For bugs/features consider creating an issue before sending a PR.

## Documentation
For further information, please read our wiki (https://github.com/nerds-ufes/polka-tna/wiki)

## Team
We are members of [NERDS (Núcleo de Estudos em Redes Definidas por Software)](http://nerds.ufes.br) at Federal University of Espírito Santo - Vitória, ES, Brazil.

Thanks to all [contributors](https://github.com/nerds-ufes/polka-tna/wiki#team)!
