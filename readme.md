# Example configuration of Nginx + uwsgi + django

## Enviroment Setup
### Install python packages
cd <trafficking dir>
pip install -r requirements.txt

### Install redis
sudo yum install redis

### Install supervisor
sudo yum install supervisor

### Install Nginx
sudo yum install nginx

### Install UWSGI
sudo yum install uwsgi

### Install python-dev
sudo yum install python-dev

### Install uwsgin plugin for python
sudo yum install uwsgi-plugin-python

## Change config file
### update config file of supervisord
```
vi /etc/supervisord.conf
# append all the text in supervisord.conf in project folder
```


### update config of nginx
```
sudo vim /etc/nginx/nginx.conf
```
append text below:
```
location /trafficking {
               uwsgi_pass    127.0.0.1:8001;
               include uwsgi_params;
               access_log  off;
 }

location /static{
  root /home/centos/trafficking
}
```

### change django settings.py
```
DEBUG = False
ALLOWED_HOSTS = ['localhost','127.0.0.1','*.<YOUR DOMMAIN>']
```

### auto restart service on reboot
```
sudo chkconfig redis on

sudo chkconfig supervisord on

sudo chkconfig nginx on
```

### config crontab
```
crontab -l
crontab -e
# 复制crontab文件内容
```

- **解决Nginx的connect() to 127.0.0.1:8080 failed (13: Permission denied) while connect**
```
setsebool -P httpd_can_network_connect 1
```

## Startup app
```
sudo service supervisord restart
sudo service nginx restart

# clean log file
sudo rm /var/log/trafficking/* -f
sudo tail /var/log/nginx/error.log
```
<em>
Note: if anything wrong, check log file in /var/log/trafficking
</em>
