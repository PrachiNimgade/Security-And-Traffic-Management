# Allow website on particular machine

      # Client1             Client2                FOR ALL
      youtube.com          Azure website           google

      microsoft.com        Heroku cloud            wiwkipedia

      cisco                redhat site              centos

      aws cloud




    # YOU NEED TO FLUSH ALL TABLES WITH COMMANDS

    # iptables --flush

    # iptables -A FORWARD -s hostip of network  -d dnsip -p udp --dport 53 -j ACCEPT
    # iptables -A FORWARD -d hostip of network  -s dnsip -p udp --sport 53 -j ACCEPT


    EX:
    iptables -A FORWARD -s 10.10.10.0/24  -d 192.168.72.20 -p udp --dport 53 -j ACCEPT
    iptables -A FORWARD -d 10.10.10.0/24  -s192.168.72.20 -p udp --sport 53 -j ACCEPT
    ===================================================================================

    ALLOW SITES ON CLIENT
    =====================

    # iptables -A FORWARD -s hostip of client  -d www.youtube.com -p tcp --dport 443 -j ACCEPT
    # iptables -A FORWARD -d hostip of client  -swww.youtube.com -p tcp --dport 443 -j ACCEPT

    EX:
    FOR CLIENT ONE
    ====================
    # iptables -A FORWARD -s 10.10.10.132  -d www.youtube.com -p tcp --dport 443 -j ACCEPT
    # iptables -A FORWARD -d 10.10.10.132  -s www.youtube.com -p tcp --sport 443 -j ACCEPT
    DO THE SAME FOR ALL WEBSITES

  ================================================================

    FOR CLIENT TWO
  =====================

    # iptables -A FORWARD -s 10.10.10.133  -d www.redhat.com -p tcp --dport 443 -j ACCEPT
  
    # iptables -A FORWARD -d 10.10.10.133  -s www.redhats.com -p tcp --sport 443 -j ACCEPT
    DO THE SAME FOR ALL WEBSITES
    =========================================================================

    FOR ALL CLIENT
    ==========================
    # iptables -A FORWARD -s 10.10.10.0  -d www.google.com -p tcp --dport 443 -j ACCEPT

    # iptables -A FORWARD -d 10.10.10.0  -s www.google.com -p tcp --sport 443 -j ACCEPT
    DO THE SAME FOR ALL WEBSITES
    ===============================================================================
    check with nslookup and website name in all  clinet machines
    # EX:
    # nslookup www.google.com
    #################################################################################


    TO ACCESS INTERNET FAST
    ==================================================================================

    # iptables -A FORWARD -s HostIpOfClient  -d www.google.com -p tcp --dport 443 -m state --state NEW,ESTABLISHED -j ACCEPT 
    # iptables -A FORWARD -d HostIpOfClient  -s www.google.com -p tcp --sport 443 -m state --state NEW,ESTABLISHED -j ACCEPT

    EX:
    ========
    iptables -A FORWARD -s 10.10.10.132 -d www.google.com -p tcp --dport 443 -m state --state NEW,ESTABLISHED -j ACCEPT 
    iptables -A FORWARD -d 10.10.10.132  -s www.google.com -p tcp --sport 443 -m state --state NEW,ESTABLISHED -j ACCEPT
    SAME FOR OTHER CLIENT
    ======================================================================================


