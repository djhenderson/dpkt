fast, simple packet creation / parsing, with definitions for the basic TCP/IP protocols.

```
>>> from dpkt.ip import IP                            
>>> from dpkt.icmp import ICMP
>>> ip = IP(src='\x01\x02\x03\x04', dst='\x05\x06\x07\x08', p=1)
>>> ip.v
4
>>> ip.src
'\x01\x02\x03\x04'
>>> ip.data
''
>>> 
>>> icmp = ICMP(type=8, data=ICMP.Echo(id=123, seq=1, data='foobar'))
>>> icmp
ICMP(type=8, data=Echo(id=123, seq=1, data='foobar'))
>>> len(icmp)
14
>>> ip.data = icmp
>>> ip.len += len(ip.data)
>>> ip
IP(src='\x01\x02\x03\x04', dst='\x05\x06\x07\x08', len=34, p=1, data=ICMP(type=8, data=Echo(id=123, seq=1, data='foobar')))
>>> pkt = str(ip)
>>> pkt
'E\x00\x00"\x00\x00\x00\x00@\x01j\xc8\x01\x02\x03\x04\x05\x06\x07\x08\x08\x00\xc0?\x00{\x00\x01foobar'
>>> IP(pkt)
IP(src='\x01\x02\x03\x04', dst='\x05\x06\x07\x08', sum=27336, len=34, p=1, data=ICMP(sum=49215, type=8, data=Echo(id=123, seq=1, data='foobar')))
```