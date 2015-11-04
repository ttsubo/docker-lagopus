What's docker-lagopus
==========
lagopusでのopenflow基本動作を確認するためのテスト環境です。
なお、lagopus.confは、BGPxのディレクトリ配下に配置してあります。


     pc2 ---- BGP6 ---- BGP4 ---- BGP1 ---- BGP3 ---- pc1
               |         |         |         |
               \------- BGP5 ---- BGP2 ------/


### download images from Docker-Hub
dockerイメージをダウンロードします

	$ docker pull ttsubo/lagopus_v020:latest
	(...snip)

	$ docker pull ttsubo/lagopus_v021:latest
	(...snip)

	$ docker pull ttsubo/pc-term:latest
	(...snip)


dockerイメージがダウンロードされたことを確認します。

	$ docker images
	REPOSITORY                  TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	ttsubo/lagopus_v021         latest              0d6fd261c358        52 minutes ago      885.7 MB
	ttsubo/lagopus_v020         latest              5e8b35b487ae        5 hours ago         876.1 MB
	ttsubo/pc-term              latest              af1d34b3d434        6 months ago        253.7 MB

Quick Start
===========
### starting docker-lagopus with lagopus v0.2.0
dockerコンテナを起動します

	$ ./simpleRouter_v020.sh start
	(...snip)


dockerコンテナ上のinterface情報を確認します

	$ docker exec -it BGP1 bash
	root@BGP1:~# cd simpleRouter/rest-client
	root@BGP1:~/simpleRouter/rest-client# ./get_interface.sh 
	======================================================================
	get_interface
	======================================================================
	/openflow/0000000000000001/interface
	----------
	reply: 'HTTP/1.1 200 OK\r\n'
	header: Content-Type: application/json; charset=UTF-8
	header: Content-Length: 399
	header: Date: Wed, 04 Nov 2015 11:15:53 GMT
	+++++++++++++++++++++++++++++++
	2015/11/04 11:15:53 : PortTable
	+++++++++++++++++++++++++++++++
	portNo   IpAddress       MacAddress        RouteDist
	-------- --------------- ----------------- ---------
	       1 192.168.101.102 00:00:00:00:01:01 
	       2 192.168.104.101 00:00:00:00:01:02 
	       3 192.168.105.101 00:00:00:00:01:03


しばらくしてから、dockerコンテナ上でAPRテーブルを確認します

	root@BGP1:~/simpleRouter/rest-client# ./get_arp.sh 
	======================================================================
	get_arp
	======================================================================
	/openflow/0000000000000001/arp
	----------
	reply: 'HTTP/1.1 200 OK\r\n'
	header: Content-Type: application/json; charset=UTF-8
	header: Content-Length: 330
	header: Date: Wed, 04 Nov 2015 11:13:56 GMT
	+++++++++++++++++++++++++++++++
	2015/11/04 11:13:56 : ArpTable 
	+++++++++++++++++++++++++++++++
	portNo   MacAddress        IpAddress
	-------- ----------------- ------------
	       1 00:00:00:00:04:01 192.168.101.101
	       2 00:00:00:00:02:02 192.168.104.102
	       3 00:00:00:00:03:01 192.168.105.102


### stopping lagopus
dockerコンテナ一式を終了します

	$ ./simpleRouter_v020.sh stop
	(...snip)

