## Namespaces (пространства имен):
### 1. Cgroups (CLONE_NEWCGROUP) - изолирует физические ресурсы
### 2. IPC (CLONE_NEWIPC) - изолирует очереди сообщений POSIX
### 3. Network (CLONE_NEWNET) - изолирует сетевые устройства, порты
### 4. Mount (CLONE_NEWNS) - изолирует файловую систему
### 5. PID (CLONE_NEWPID) - изолирует PIDs
### 6. Time (CLONE_NEWTIME) - изолирует 
### 7. User (CLONE_NEWUSER) - изолирует UIDs и GIDs.
### 8. UTS (CLONE_NEWUTS) - Unix Time Sharing изолирует hostname и доменное имя NIS (отличие от DNS ниже).

#### Факты:
1. Просмотры namespace процесса:
```bash
ls -l /proc/<pid>/ns
```
3. unshare - команда для запуска процесса в другом namespace (-u другое UTC):
```bash
	sudo unshare -u bash
```
4. Контейнеры по своей сути — обыкновенные процессы с отличающимися от других процессов namespaces, чем меньше у контейнера общих с другими namespace, тем более он контейнернее
5. При использовании команды ниже, флаг net говорит docker, что не надо создавать отдельный network namespace для процесса:
```bash
docker run --net=host redis
```
6. Узнать PID текущего процесса:
```bash
echo $$
```
7. В NIS хранится информация не только об именах и адресах хостов, но и о пользователях, самой сети и сетевых службах. Эта совокупность сетевой информации называется пространством имен NIS. В DNS же хранится информация только об именах и адресах хостов.
8. В User Namespace меняется id возвращаемый системными вызовами getuid и getguid на тот который замаплен в файлах /proc/\<pid>/uid_map и /proc/\<pid>/gid_map. Мапинг происходит с помощью записи трех чисел в данные файлы первое - начало диапазона id в родительском Nmasepace, второе начало диапазона id в дочернем Nmasepace, третье - размер диапазона. Соответственно, если в файле три следующих числа 0 1000 1, то пользователь с id 1000 в родительском процессе будет root в дочернем процессе.
9. User Namespace единственное Namespace, для создания которого не нужны root права, кроме того, при создании нового User Namespace в совокупности с другими Namespace для них также убирается необходимость root прав
10. Если в User Namespace не замапить uid и gid то все действия будет выполняться от пользователя nobody:nobody с id 65534, у которого практически нет прав. Мапить можно только один раз
11. Если держать ns файл открытым или замаунтить его куда-нибудь то после завершения процесса, namespace не исчезает 
12. Pid namespace очень интересно себя ведет, так как при указании флага нового Pid namespace сам процесс в новый Pid namespace не переходит, а только ставится флаг, что все дети данного процесса будут в этом Pid Namespace. Соответственно если первый ребенок данного процесса завершится то другие дети не смогут запуститься, так как будет отсутствовать так называемый init процесс с id 1.
13. Также Pid Namespace неразрывно связан с mount namespace, так как такие утилиты как ps и тд используют каталог proc для получения информации о процессах, соответственно, если хочется не видеть родительских процессов, то необходимо перемаунтить данный каталог
14. Немного о init процессе в Pid Namespace. У данного процесса ppid равен 0, как у всех процессов, родитель которых находится в другом namespace. Init процесс просто так не убить, так как он считается системообразующим и без него использовать namespace не выйдет (даже kill -9 изнутри namespace не сработает). Также init автоматически становится родителем всем сиротам в namespace, а значит должен уметь их собирать. 
15. Зачем такие махинации с Pid namespace, а из-за того что со стародавних времён программы считают, что на протяжении всего жизненного цикла у них постоянный Pid, а при запуске процесса в новом Pid namespace без этих махинаций его Pid поменяется на 1 (работает только для setns и unshare, не касается clone, так как он создаёт новый процесс в совершенно новом namespace, а не меняет namespaces текущего)
16. Clone - создать новый процесс в новом namespace
17. Setns - присоединить текущий процесс к существующему namespace 
18. Unshare - отсоединить текущий процесс от текущего namespace и присоединить к совершенно новому
19. even if root employs clone(CLONE_NEWUSER), the resulting child process will have no capabilities in the parent namespace. 
20. In other words, the user ID 1000 in the parent user namespace (which was formerly mapped to 65534) has been mapped to user ID 0 in the user namespace created by demo_userns. 
21. The /proc/PID/uid_map file is owned by the user ID that created the namespace, and is writeable only by that user (or a privileged user).
22. when a process with non-zero user IDs performs an execve(), the process's capability sets are cleared. To avoid this problem, it is necessary to create a user ID mapping inside the user namespace before performing the execve(). 
23. 