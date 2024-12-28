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
 
   - 'psql -c "ALTER USER postgres WITH PASSWORD ''password'';"'
   - 'psql -c "CREATE USER test_user WITH PASSWORD ''password'' LOGIN;"'
   - 
   
 * Create DB "test_db1" and grant access to test_user
 
   - CREATEDB test_db1;
   - 'psql -c "CREATE DATABASE test_db1 WITH OWNER test_user;"'
   
 * Create User for monitoring: svc_monitoring
 
   - 'psql -c "CREATE USER svc_monitoring WITH PASSWORD ''password'';"'
   - 'psql -c "grant pg_monitor to svc_monitoring;"'
   
_____________________________________________________________________________________

 * Checking availability PG
 
  - 192.168.1.101 port: 5432
  - 192.168.1.102 port: 5432
  
 * Grafana access: http://192.168.1.103:3000 admin/admin
 * Prometheus access: http://192.168.1.103:9090/targets
 
  - Dashboards Node Exporter: /shared/Dashboard.json 
  - Dashboards Postgres Exporter: /shared/PG_Dashboard.json
_____________________________________________________________________________________ 
