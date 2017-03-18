#### Check Package  Signature
`rpm --checksig pidgin-2.7.9-5.el6.2.i686.rpm`

#### check dependencies vom RPM
`rpm -qpR BitTorrent-5.2.2-1-Python2.4.noarch.rpm`

#### Überprüfen eines Installierten RPM Paketes 
`rpm -q BitTorrent`

#### Liste aller Dateien eines Installierten  RPM Paketes
`rpm -ql BitTorrent`

#### List kürzlich Installierter RPM Pakete
`rpm -qa --last`

#### Alle installierten RPM Pakete auflisten
`rpm -qa`

#### Suchen mit welchem RPM Packt ein Programm installiert wurde. 
`rpm -qf /usr/bin/htpasswd`

#### Infos zu einem Paket
`rpm -qi vsftpd`

#### Infos zu einem Paket vor der Installation 
`rpm -qip sqlbuddy-1.3.3-1.noarch.rpm`

#### Dokumentation zu einem Paket finden 
`rpm -qdf /usr/bin/vmstat`

## rebuild  einer Korrupten RPM Database
```
cd /var/lib
rm __db*
rpm --rebuilddb
rpmdb_verify Packages
```

**Quellen:**
[RHEL Packaging - (making life easier with RPM)](https://jnovy.fedorapeople.org/scl-utils/scl.pdf)
[]()