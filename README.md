**1** Попробуйте запустить playbook на окружении из test.yml, зафиксируйте значение, которое имеет факт some_fact для указанного хоста при выполнении playbook.
	
проверяем плейбук
**vi site.yml** 
Факт **some_fact** выводится с помощью **debug** и названием таски **Print fact**  
соответсвенно 
**ansible-playbook site.yml -i inventory/test.yml**

	TASK [Print fact] **********************************************************************************************************************************************************************************************************
	ok: [localhost] => {
		"msg": 12
	}	


**some_fact = 12**	


![01_some_fact](https://github.com/user-attachments/assets/4222c9f8-abbc-4856-98d1-8c4a5b2e3a08)


----------------------	
		
**2**.Найдите файл с переменными (**group_vars**), в котором задаётся найденное в первом пункте значение, и поменяйте его на **all default fact**. 
в папке **group_vars** ищем тот yaml у который нам подходит. в нашем случае "**all**"
применяем 

**ansible-playbook site.yml -i inventory/test.yml**


	TASK [Print fact] 							**********************************************************************************************************************************************************************************************************
	ok: [localhost] => {
		"msg": "all default fact."
	}


![02_all_default_fact](https://github.com/user-attachments/assets/6fb1f088-810e-42f2-bd04-ee98394635e6)

----------------------	
	
**3**.Воспользуйтесь подготовленным (используется **docker**) или создайте собственное окружение для проведения дальнейших испытаний.

Контейнеры для ansible:

sudo docker run -dit --name centos7 pycontribs/centos:7 sleep 6000000

sudo docker run -dit --name ubuntu pycontribs/ubuntu:latest sleep 6000000

----------------------	

**4**.Проведите запуск playbook на окружении из **prod.yml**. Зафиксируйте полученные значения **some_fact** для каждого из managed host.

Запускаем плейбук:
**sudo ansible-playbook site.yml -i inventory/prod.yml**


![04_prode_some_fact](https://github.com/user-attachments/assets/8761df99-de69-4919-8f24-b3d59f09ad0c)


----------------------	

**5**. Добавьте факты в **group_vars** каждой из групп хостов так, чтобы для **some_fact** получились значения: для deb — **deb default fact**, для el — **el default fact**.

Меняем в файлах **deb\examp.yml** и **el\examp.yml**

----------------------	
	
**6**. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.

**sudo ansible-playbook site.yml -i inventory/prod.yml**


![06_prode_some_fact](https://github.com/user-attachments/assets/e1b4f005-7825-4abc-be6e-374e2f3e2a97)


----------------------	

**7**. При помощи ansible-vault зашифруйте факты в **group_vars/deb** и **group_vars/el** с паролем **netology**.

ansible-vault encrypt group_vars/deb/examp.yml

ansible-vault encrypt group_vars/el/examp.yml

----------------------	

**8**.Запустите playbook на окружении **prod.yml**. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.

**sudo ansible-playbook site.yml -i inventory/prod.yml --ask-vault-pass**


![08_vault_pass](https://github.com/user-attachments/assets/36da6a13-6285-4bcd-891a-c5fdc2f0aea8)


----------------------	

**9**.Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.

Получим список плагинов:
**ansible-doc -t connection -l**


![09_connection_plugins](https://github.com/user-attachments/assets/a68673ac-8937-4c56-ba61-4e5f3737328a)


----------------------	 

**10**.В **prod.yml** добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения. 
Добавляет в указанный файл блок:

---yml
	  local:
	    hosts:
	      localhost:
	        ansible_connection: local
---

----------------------	 
	
**11**.Запустите playbook на окружении **prod.yml**. При запуске ansible должен запросить у вас пароль. Убедитесь, что факты **some_fact** для каждого из хостов определены из верных group_vars.

**sudo ansible-playbook site.yml -i inventory/prod.yml --ask-vault-pass**


![11_prod](https://github.com/user-attachments/assets/223bebf7-6da2-4cc3-bc60-2035f315f5f7)


----------------------






