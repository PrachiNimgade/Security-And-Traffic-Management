		ACL => ACCESS CONTROL LIST
		ACL Types => Helps you to identify endpoints i.e. client or server


		ACL Types:-

		src		-		source ip
	        dst		-		destination ip
		dstdomain	- destination domain
		srcdomain - source domain
		time			- time based
 		url_regex - search url contents

		 =============================================================================================

 
		# ON MACHINE ONE

		iptables -t nat -F
		iptables -A INPUT -s hostnetwork ip -p tcp --dport 3128 -j ACCEPT
		yum install squid
		cd /etc/squid/squid.conf
  		vim /etc/squid/squid.conf
 		  	uncomment == cache_dir ufs /var/spool/squid 2048 16 256
      			visible_hostname proxy.hpcsa.lab (proxy name)
     		:wq

      (2048 is size of cache  16 is create directories the 16 directories is created in that directories 256 dir created)

      		ls /var/spool/squid
		squid -z 
  			ls /var/sppool/squid
     			16 dir created in binary form
			01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

   
			systemctl start squid
			systemctl status squid

   			
			check ip on master machine

   			go to the client machine
			open browser
			setting
			search for proxy
			go to setting
			select manual proxy write master machine host ip in http proxy

			socks host  master machine host ip  select SOCKS_vS

			TRY  TO BLOCK SITES


			vim /etc/squid/squid.conf

			write below to the insert statement

			acl hpcsa src network ip of host (10.10.10.0/24)
			acl microsoft dstdomain .microsoft.com
			http_access deny microsoft hpcsa 
			httpc_access allow hpcsa


			comment below rule
			http_access allow localnet

			:wq

   			systemctl restart squid
			systemctl status squid
   
			go to client machine
			refresh browser and check microsoft site it will not open


		        vim /etc/squid/suid.conf

			modifiy the rule that we are added before

			acl hpcsa src network ip of host (10.10.10.0/24)
			acl blocked_sites dstdomain "/etc/squid/blocked-sites.txt"
			http_access deny blocked-sites hpcsa 
			http_access allow hpcsa

			:wq

			edit below file for bocking site we want
			redhat
			.microsoft.com
			.youtube.com
			.facebook.com
			
			:wq
			
			systemctl restart squid
			systemctl status squid
			
			goto the client machine and check in browser if the above site open or blocked
			
			
			
			On main machine
			
			
			vim /etc/squid/client1-blocked.txt
			.redhat.com
			
			
			vim /etc/squid/suid.conf
			
			acl hpcsa src network ip of host (10.10.10.0/24)
			acl client1 src client host ip (10.10.10.132)
			acl blocked_sites dstdomain "/etc/squid/blocked-sites.txt"
			acl client1-blocked dstdomain "/etc/squid/client1-blocked.txt"
			http_access deny client1-blocked client1
			http_access deny blocked-sites hpcsa 
			http_access allow hpcsa
			
			
			:wq
			
			
			systemctl restart squid
			
			
			
			day of week
			S- subday
			M- monday
			T- tuesday
			W- wednsday
			H-thursday
			F-friday
			A- saturday
			
			time based acl   time 16.00- 18.00
			
			
			
			vim /etc/squid/suid.conf
			
			acl hpcsa src network ip of host (10.10.10.0/24)
			acl client src client host ip (10.10.10.132)
			acl cdac-time time MWF 11.00-14.00   (monday wed thurs)
			acl cdac dstdomain .cdac.in
			acl blocked_sites dstdomain "/etc/squid/blocked-sites.txt"
			acl client1-blocked dstdomain "/etc/squid/client1-blocked.txt"
			http_access deny cdac cdac-time
			http_access deny client1-blocked client1
			http_access deny blocked-sites hpcsa 
			http_access allow hpcsa
			
			:wq
			
			systemctl restart squid
			systemctl status squid
			
			
			
			
			
			
			vim /etc/squid/suid.conf
			
			acl hpcsa src network ip of host (10.10.10.0/24)
			acl client src client host ip (10.10.10.132)
			acl cdac-time time MWF 11.00-14.00   (monday wed thurs)
			acl cdac dstdomain .cdac.in
			acl blocked_sites dstdomain "/etc/squid/blocked-sites.txt"
			acl client1-blocked dstdomain "/etc/squid/client1-blocked.txt"
			acl badwords url_regex -i "/etc/squid/badwords.txt"
			http_access deny cdac cdac-time
			http_access deny client1-blocked client1
			http_access deny blocked-sites hpcsa 
			http_access deny badwords hpcsa
			http_access allow hpcsa
			
			:wq
			
			
			vim /etc/squid/badwords.txt
			edit with
			
			torrent
			cricket
			ipl
			football
			movies
			bookmyshow
			
			
			:wq
			
			systemctl restart squid
			systemctl status squid
			
			search with that badwords in client machine browser
			
			
			c
			
			acl hpcsa src network ip of host (10.10.10.0/24)
			acl client src client host ip (10.10.10.132)
			acl cdac-time time MWF 11.00-14.00   (monday wed thurs)
			acl cdac dstdomain .cdac.in .yahoo.com
			acl blocked_sites dstdomain "/etc/squid/blocked-sites.txt"
			acl client1-blocked dstdomain "/etc/squid/client1-blocked.txt"
			acl badwords url_regex -i "/etc/squid/badwords.txt"
			http_access deny cdac cdac-time
			http_access deny client1-blocked client1
			http_access deny blocked-sites hpcsa 
			http_access deny badwords hpcsa
			http_access allow hpcsa
			
			:wq
			
			vim /etc/squid/badwords.txt
			
			flipkart
			cricket
			myntra
			ipl
			
			
			:wq
			
			
			
			
			systemctl restart squid
			systemctl status squid
			
			
			
			
			vim /etc/squid/squid.conf
			auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/squid-users
			auth_param basic children 5
			auth_param basic realm Squid Basic Authentication
			auth_param basic credentialsttl 2 hours
			acl acts_users proxy_auth REQUIRED
			
			
			
			http_access allow acts_user
			
			
			
			
			:wq
			
			
			
			yum install httpd-tools
			cd squid
			htpasswd
			htpasswd -c /etc/squid/squid-users user1
			
			
			












   

			


