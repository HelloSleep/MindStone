https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/tutorials-wordpress.html

```shell
#!/bin/bash
sudo yum install -y httpd
sudo service httpd start
sudo systemctl enable httpd
sudo yum install git -y

wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
cd wordpress
cp wp-config-sample.php wp-config.php
cd ~
sudo cp -r wordpress/* /var/www/html/
```

```shell
vi wp-config-sample

DB-NAME = DB의 이름 (임의로)
DB-USER = DB 사용자 이름 (임의로)
DB-PASSWORD = DB 사용자 패스워드 (임의로)
DB-HOST = RDS 데이터베이스의 Endpoint


#mariaDB php 설치
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo systemctl restart httpd
```


ALB접속시 첫 페이지 수정하기
```shell
grep -r '원하는 문장' [위치]
#ec grep -r 'Mind' .
/var/www/html/wp-content/themes/twentytwentythree/templates/home.html


```


```sh
sudo yum install git -y
git clone https://github.com/WordPress/WordPress.git /tmp/WordPress

```
