Задание
Добавить в Vagrantfile еще дисков.
Собрать RAID-массив уровня 0/5/10 на выбор.
Прописать собранный массив в конфиг, чтобы он собирался при загрузке
Сломать/починить RAID.
Создать GPT-таблицу и 5 разделов поверх массива, смонтировать их в системе.


Итог

[root@otuslinux ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        489M     0  489M   0% /dev
tmpfs           496M     0  496M   0% /dev/shm
tmpfs           496M  6,7M  489M   2% /run
tmpfs           496M     0  496M   0% /sys/fs/cgroup
/dev/sda1        40G  4,8G   36G  12% /
tmpfs           100M     0  100M   0% /run/user/1000
/dev/md0p1      139M  1,6M  127M   2% /raid/part1
/dev/md0p2      140M  1,6M  128M   2% /raid/part2
/dev/md0p3      142M  1,6M  130M   2% /raid/part3
/dev/md0p4      140M  1,6M  128M   2% /raid/part4
/dev/md0p5      139M  1,6M  127M   2% /raid/part5
[root@otuslinux ~]# 
