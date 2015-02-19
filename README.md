# ssh-connection-test
this is to check about background process in the remote server with ssh protocol.

I want to check ssh connection, when i use backthread process in the server.  
this is simple test script for check a time on ssh protocol.  
```
[client]+--------+[server]

                  e1,sh
                  e1wrap,sh
                  e1mon,sh

** content
```

[e1.sh]
- count 5 sec and echo each count.

```
#!/bin/sh

for cnt in `seq 1 5`
do
  echo count=$cnt
  sleep 1
done
```

[e1wrap.sh]
- wrapper e1.sh for doing backgrand jobs.

```
#!/bin/sh

nohup ./e1.sh &
```

[e1mon.sh]
- monitor e1.sh process in the server.

```
#!/bin/sh

while true
do
  ps -ef|grep e1.sh
  sleep 1
done 

```

## Test Operation

###### pre setting for monitoering
[server]
```
./e1mon.sh
```

###### check 01
[client]
```
time ssh user@server './e1.sh'
```

###### check 02
[client]
```
time ssh user@server 'nohup ./e1.sh &'
```

###### check 03
[client]
```
time ssh user@server './e1wrap.sh'
```

###### check 04
[client]
```
time ssh user@server 'nohup ./e1wrap.sh &'
```

###### check 05
[server]: another teraterm
```
time ssh user@server './e1.sh'
```

###### check 06
[server]: another teraterm
```
time ssh user@server 'nohup ./e1.sh &'
```

###### check 07
[server]: another teraterm
```
time ssh user@server './e1wrap.sh'
```

###### check 08
[server]: another teraterm
```
time ssh user@server 'nohup ./e1wrap.sh &'
```

#### Gas-Nuki

[nohupがcoreosで上手く動かない対処法 - gist](https://gist.github.com/kotaro-dev/b2c81b6e9775dc1e0256 nohupがcoreosで上手く動かない対処法)
