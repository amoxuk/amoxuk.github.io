# python使用dpkt读取pcap/pcapng

## read pkt

```python
        try:
            pkt = dpkt.pcap.Reader(fd)
        except:
            fd.seek(0, 0)
            pkt = dpkt.pcapng.Reader(fd)
        time, buf, = pkt.next()
```

> ffmpeg 本地发到本地的端口显示 `NULL/Lookback`

python读取时,需要先过滤掉前面的4个字节：

```python
        time, buf, = pkt.next()
        eth = Ethernet(buf)
        ip = dpkt.ip.IP(buf[4:])
        rtp = RTP(ip.data.data)
        print(rtp.pt)
```

## wireshark

许多协议wireshark都实现了解析的功能，只不过是C的实现方式：
GITHUB 源码：<https://github.com/IFGHou/Wireshark>
查询列表：<https://www.cxymm.net/article/weixin_33753845/92617747> 

---

> [跳转到目录](index.md)
