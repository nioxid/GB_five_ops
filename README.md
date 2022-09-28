Инструкция:

Для запуска проекта необходимо:
  1. Наличие 4х серверов, имена и ip-адреса которых должны быть описаны в inventory/hosts.yaml.
     Характеристики серверов должны быть не менее 1vcpu, 2G vram, 20Gb диск.
     По умолчанию имена серверов выглядят так: ubuntu-basic-1-2-20gb-1, ubuntu-basic-1-2-20gb-2, ubuntu-basic-1-2-20gb-3, ubuntu-basic-1-2-20gb-4
     Если у вас другие имена или ip-адреса, измените их в inventory/hosts.yaml.   
  
  2. На серверах должны быть открыты сетевые порты : 80,443,22,3000(для Grafana), 9090(Prometheus) 
  
  3. К серверам должен быть доступ по сертификату. 
     Приватный ключ необходимо добавить в settings-security-secrets c именем SSH_PRIVATE_KEY.
     Публичный ключ должен храниться в public_keys c расширением .pub
     
  4. Обязательно наличие SSL-сертификатов для сайта.
     Они должны храниться в папке ssl_certs  
     
  5. В настройках DNS вашего домена должны быть прописаны A-записи с ip-адресами ваших серверов приложений.   
  

Проект разворачивается на сервера 2-мя способами:

  1. При клонировании репозитория командой git clone https://github.com/simanchuk/GB_five_ops.git
     и запуском плейбука командой ansible-playbook main.yml.
     Указать нужный ssh-ключ можно добавив --key-file "~/.ssh/keyname".
     Для запуска плея по tags: "php", "nginx", "os", "managessh" и др - ansible-playbook --tags "tag_name" main.yml
 
  2. Проект устанавливется на сервера при коммите или мердж-реквесте в ветку master.


После успешного развёртывания проекта на серверах:
  
  1. По адресу https://www.5ops.site/wp-login.php?loggedout=true&wp_lang=ru_RU 
     доступна админ-панель wordpress, сам сайт по адресу https://www.5ops.site
     
  2. По адресу http://84.23.55.123:3000 доступна Grafana
  
  3. По адресу http://84.23.55.123:9090 доступен Prometheus


Для настройки оповещений использовались следующие ресурсы:
 - Github в telegramm: https://www.youtube.com/watch?v=kGbScE_PFvE
 - Оповещения об успешности развертывания: https://github.com/yanzay/notify-telegram
 - Алертинг в Grafana: https://docs.ispsystem.ru/vmmanager-admin/monitoring/grafana/grafana-nastrojka-uvedomlenij
   (ChatID и BotID использовались из шага выше)


Описание файлов проекта:

 - В файле main.yml указываются группы хостов и роли, применяемые к ним.

 - Роли описаны в папке Roles/role_name/task/main.yml.
   
 - Переменные(включая пароли) описаны в файле group_vars/all.yml , изменить и добавить их можно там.

 - Публичные ключи для доступа к серверам необходимо хранить в папке public_keys

 - SSL- сертификаты для сайта хранятся в папке ssl_certs

 - В файле .github/workflows/Blank.yml описан код развертывания проекта на сервера.
