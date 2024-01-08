Домашнее задание
Подготовить стенд на Vagrant как минимум с одним сервером. На этом
сервере используя Ansible необходимо развернуть nginx со следующими
условиями:
- необходимо использовать модуль yum/apt
- конфигурационные файлы должны быть взяты из шаблона jinja2 с
переменными
- после установки nginx должен быть в режиме enabled в systemd
- должен быть использован notify для старта nginx после установки
- сайт должен слушать на нестандартном порту - 8080, для этого использовать
переменные в Ansible
* Сделать все это с использованием Ansible роли
Домашнее задание считается принятым, если:
- предоставлен Vagrantfile и готовый playbook/роль ( инструкция по запуску
стенда, если посчитаете необходимым )
- после запуска стенда nginx доступен на порту 8080
- при написании playbook/роли соблюдены перечисленные в задании условия

Установка Ansible
● Версия Ansible =>2.4 требует для своей работы Python 2.6 или выше
● Убедитесь что у Вас установлена нужная версия:

sabo@sabo-HP-Pavilion-Notebook:~$ python3 -V 
Python 3.10.12

● Далее произведите установку для Вашей ОС по инструкции и убедитесь что
Ansible установлен корректно:

sabo@sabo-HP-Pavilion-Notebook:~$ ansible --version
ansible 2.10.8

Подготовка окружения

sabo@sabo-HP-Pavilion-Notebook:~$ cd /home/sabo/Ansible/
sabo@sabo-HP-Pavilion-Notebook:~/Ansible$ vagrant up
Bringing machine 'nginx' up with 'virtualbox' provider...
==> nginx: Checking if box 'centos/7' version '2004.01' is up to date...
==> nginx: Clearing any previously set forwarded ports...
==> nginx: Clearing any previously set network interfaces...
==> nginx: Preparing network interfaces based on configuration...
    nginx: Adapter 1: nat
    nginx: Adapter 2: hostonly
==> nginx: Forwarding ports...
    nginx: 22 (guest) => 2222 (host) (adapter 1)
==> nginx: Running 'pre-boot' VM customizations...
==> nginx: Booting VM...
==> nginx: Waiting for machine to boot. This may take a few minutes...
    nginx: SSH address: 127.0.0.1:2222
    nginx: SSH username: vagrant
    nginx: SSH auth method: private key
==> nginx: Machine booted and ready!
==> nginx: Checking for guest additions in VM...
    nginx: No guest additions were detected on the base box for this VM! Guest
    nginx: additions are required for forwarded ports, shared folders, host only
    nginx: networking, and more. If SSH fails on this machine, please install
    nginx: the guest additions and repackage the box to continue.
    nginx: 
    nginx: This is not an error message; everything may continue to work properly,
    nginx: in which case you may ignore this message.
==> nginx: Setting hostname...
==> nginx: Configuring and enabling network interfaces...
==> nginx: Rsyncing folder: /home/sabo/Ansible/ => /vagrant
==> nginx: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> nginx: flag to force provisioning. Provisioners marked to run always will still run.


abo@sabo-HP-Pavilion-Notebook:~/Ansible$ vagrant ssh-config
Host nginx
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /home/sabo/Ansible/.vagrant/machines/nginx/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
  PubkeyAcceptedKeyTypes +ssh-rsa
  HostKeyAlgorithms +ssh-rsa


sabo@sabo-HP-Pavilion-Notebook:~/Ansible$ ansible nginx -m ping
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}

Окончательный вариант

sabo@sabo-HP-Pavilion-Notebook:~/Ansible$ ansible-playbook nginx.yml

PLAY [NGINX | Install and configure NGINX] *************************************

TASK [Gathering Facts] *********************************************************
ok: [nginx]

TASK [NGINX | Install EPEL Repo package from standart repo] ********************
ok: [nginx]

TASK [NGINX | Install NGINX package from EPEL Repo] ****************************
ok: [nginx]

TASK [NGINX | Create NGINX config file from template] **************************
ok: [nginx]

PLAY RECAP *********************************************************************
nginx                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   



Проверка доступа к nginx


sabo@sabo-HP-Pavilion-Notebook:~/Ansible$ curl http://192.168.56.10:8080
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
  <title>Welcome to CentOS</title>
  <style rel="stylesheet" type="text/css"> 

	html {
	background-image:url(img/html-background.png);
	background-color: white;
	font-family: "DejaVu Sans", "Liberation Sans", sans-serif;
	font-size: 0.85em;
	line-height: 1.25em;
	margin: 0 4% 0 4%;
	}

	body {
	border: 10px solid #fff;
	margin:0;
	padding:0;
	background: #fff;
	}

	/* Links */

	a:link { border-bottom: 1px dotted #ccc; text-decoration: none; color: #204d92; }
	a:hover { border-bottom:1px dotted #ccc; text-decoration: underline; color: green; }
	a:active {  border-bottom:1px dotted #ccc; text-decoration: underline; color: #204d92; }
	a:visited { border-bottom:1px dotted #ccc; text-decoration: none; color: #204d92; }
	a:visited:hover { border-bottom:1px dotted #ccc; text-decoration: underline; color: green; }
 
	.logo a:link,
	.logo a:hover,
	.logo a:visited { border-bottom: none; }

	.mainlinks a:link { border-bottom: 1px dotted #ddd; text-decoration: none; color: #eee; }
	.mainlinks a:hover { border-bottom:1px dotted #ddd; text-decoration: underline; color: white; }
	.mainlinks a:active { border-bottom:1px dotted #ddd; text-decoration: underline; color: white; }
	.mainlinks a:visited { border-bottom:1px dotted #ddd; text-decoration: none; color: white; }
	.mainlinks a:visited:hover { border-bottom:1px dotted #ddd; text-decoration: underline; color: white; }

	/* User interface styles */

	#header {
	margin:0;
	padding: 0.5em;
	background: #204D8C url(img/header-background.png);
	text-align: left;
	}

	.logo {
	padding: 0;
	/* For text only logo */
	font-size: 1.4em;
	line-height: 1em;
	font-weight: bold;
	}

	.logo img {
	vertical-align: middle;
	padding-right: 1em;
	}

	.logo a {
	color: #fff;
	text-decoration: none;
	}

	p {
	line-height:1.5em;
	}

	h1 { 
		margin-bottom: 0;
		line-height: 1.9em; }
	h2 { 
		margin-top: 0;
		line-height: 1.7em; }

	#content {
	clear:both;
	padding-left: 30px;
	padding-right: 30px;
	padding-bottom: 30px;
	border-bottom: 5px solid #eee;
	}

    .mainlinks {
        float: right;
        margin-top: 0.5em;
        text-align: right;
    }

    ul.mainlinks > li {
    border-right: 1px dotted #ddd;
    padding-right: 10px;
    padding-left: 10px;
    display: inline;
    list-style: none;
    }

    ul.mainlinks > li.last,
    ul.mainlinks > li.first {
    border-right: none;
    }

  </style>

</head>

<body>

<div id="header">

    <ul class="mainlinks">
        <li> <a href="http://www.centos.org/">Home</a> </li>
        <li> <a href="http://wiki.centos.org/">Wiki</a> </li>
        <li> <a href="http://wiki.centos.org/GettingHelp/ListInfo">Mailing Lists</a></li>
        <li> <a href="http://www.centos.org/download/mirrors/">Mirror List</a></li>
        <li> <a href="http://wiki.centos.org/irc">IRC</a></li>
        <li> <a href="https://www.centos.org/forums/">Forums</a></li>
        <li> <a href="http://bugs.centos.org/">Bugs</a> </li>
        <li class="last"> <a href="http://wiki.centos.org/Donate">Donate</a></li>
    </ul>

	<div class="logo">
		<a href="http://www.centos.org/"><img src="img/centos-logo.png" border="0"></a>
	</div>

</div>

<div id="content">

	<h1>Welcome to CentOS</h1>

	<h2>The Community ENTerprise Operating System</h2>

	<p><a href="http://www.centos.org/">CentOS</a> is an Enterprise-class Linux Distribution derived from sources freely provided
to the public by Red Hat, Inc. for Red Hat Enterprise Linux.  CentOS conforms fully with the upstream vendors
redistribution policy and aims to be functionally compatible. (CentOS mainly changes packages to remove upstream vendor
branding and artwork.)</p>

	<p>CentOS is developed by a small but growing team of core
developers.&nbsp; In turn the core developers are supported by an active user community
including system administrators, network administrators, enterprise users, managers, core Linux contributors and Linux enthusiasts from around the world.</p>

	<p>CentOS has numerous advantages including: an active and growing user community, quickly rebuilt, tested, and QA'ed errata packages, an extensive <a href="http://www.centos.org/download/mirrors/">mirror network</a>, developers who are contactable and responsive, Special Interest Groups (<a href="http://wiki.centos.org/SpecialInterestGroup/">SIGs</a>) to add functionality to the core CentOS distribution, and multiple community support avenues including a <a href="http://wiki.centos.org/">wiki</a>, <a
href="http://wiki.centos.org/irc">IRC Chat</a>, <a href="http://wiki.centos.org/GettingHelp/ListInfo">Email Lists</a>, <a href="https://www.centos.org/forums/">Forums</a>, <a href="http://bugs.centos.org/">Bugs Database</a>, and an <a
href="http://wiki.centos.org/FAQ/">FAQ</a>.</p>

	</div>

</div>


</body>
</html>
sabo@sabo-HP-Pavilion-Notebook:~/Ansible$ 
