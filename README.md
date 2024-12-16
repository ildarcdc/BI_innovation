# BI_innovation

Requirements:
 * vagrant init eurolinux-vagrant/oracle-linux-8 --box-version 8.10.5
# install additional package
 * vagrant plugin install vagrant-disksize
 
# Postgrsql 
 * Credentials:
   - ETCD cluster: etcd01/192.168.1.100 pgsql01/192.168.1.101 pgsql02/192.168.1.102
   - PG cluster: pgsql01/192.168.1.101 pgsql02/192.168.1.102
   - Patroni cluster: pgsql01/192.168.1.101 pgsql02/192.168.1.102
   - Ansible control node: docker01/192.168.1.103
   - All nodes: user/root password/vagrant
  
 * Create user "test_user" grant access to connect PG
 
   - CREATE ROLE Read_Only_User WITH LOGIN PASSWORD 'password';
   - GRANT Read_Only_User TO test_user;
   
 * Create DB "test_db1" and grant access to test_user
 
   - CREATEDB test_db1;
   - GRANT CONNECT ON DATABASE test_db1 TO test_user WITH GRANT OPTION;