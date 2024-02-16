# Домашнее задание к занятию 9 «Процессы CI/CD» - Сергей Ситкарёв

### Подготовка к выполнению
Создайте два VM в Yandex Cloud с параметрами: 2CPU 4RAM Centos7 (остальное по минимальным требованиям).

Пропишите в inventory playbook созданные хосты.

Добавьте в files файл со своим публичным ключом (id_rsa.pub). Если ключ называется иначе — найдите таску в плейбуке, которая использует id_rsa.pub имя, и исправьте на своё.

Запустите playbook, ожидайте успешного завершения.

Проверьте готовность SonarQube через браузер.

Зайдите под admin\admin, поменяйте пароль на свой.

![Задание0](https://github.com/SSitkarev/cicd-3/blob/main/img/01.jpg)

Проверьте готовность Nexus через бразуер.

Подключитесь под admin\admin123, поменяйте пароль, сохраните анонимный доступ.

![Задание0](https://github.com/SSitkarev/cicd-3/blob/main/img/02.jpg)

### Знакомоство с SonarQube

Создайте новый проект, название произвольное.

Скачайте пакет sonar-scanner, который вам предлагает скачать SonarQube.

Сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).

Проверьте sonar-scanner --version.

Запустите анализатор против кода из директории example с дополнительным ключом -Dsonar.coverage.exclusions=fail.py.

Посмотрите результат в интерфейсе.

![Задание1](https://github.com/SSitkarev/cicd-3/blob/main/img/11.jpg)

Исправьте ошибки, которые он выявил, включая warnings.

Запустите анализатор повторно — проверьте, что QG пройдены успешно.

Сделайте скриншот успешного прохождения анализа, приложите к решению ДЗ.

![Задание1](https://github.com/SSitkarev/cicd-3/blob/main/img/12.jpg)

### Знакомство с Nexus

В репозиторий maven-public загрузите артефакт с GAV-параметрами:

groupId: netology;

artifactId: java;

version: 8_282;

classifier: distrib;

type: tar.gz.

В него же загрузите такой же артефакт, но с version: 8_102.

Проверьте, что все файлы загрузились успешно.

![Задание2](https://github.com/SSitkarev/cicd-3/blob/main/img/21.jpg)

В ответе пришлите файл maven-metadata.xml для этого артефекта.

```xml
<metadata modelVersion="1.1.0">
	<groupId>netology</groupId>
	<artifactId>java</artifactId>
	<versioning>
		<latest>8_282</latest>
		<release>8_282</release>
		<versions>
			<version>8_102</version>
			<version>8_282</version>
		</versions>
		<lastUpdated>20240216092126</lastUpdated>
	</versioning>
</metadata>
```

### Знакомство с Maven

Подготовка к выполнению

Скачайте дистрибутив с maven.

Разархивируйте, сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).

Удалите из apache-maven-<version>/conf/settings.xml упоминание о правиле, отвергающем HTTP- соединение — раздел mirrors —> id: my-repository-http-unblocker.

Проверьте mvn --version.

Заберите директорию mvn с pom.

Основная часть

Поменяйте в pom.xml блок с зависимостями под ваш артефакт из первого пункта задания для Nexus (java с версией 8_282).

Запустите команду mvn package в директории с pom.xml, ожидайте успешного окончания.

Проверьте директорию ~/.m2/repository/, найдите ваш артефакт.

![Задание3](https://github.com/SSitkarev/cicd-3/blob/main/img/31.jpg)

В ответе пришлите исправленный файл pom.xml.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.netology.app</groupId>
  <artifactId>simple-app</artifactId>
  <version>1.0-SNAPSHOT</version>
   <repositories>
    <repository>
      <id>my-repo</id>
      <name>maven-public</name>
      <url>http://51.250.106.112:8081/repository/maven-public/</url>
    </repository>
  </repositories>
  <dependencies>
    <dependency>
      <groupId>netology</groupId>
      <artifactId>java</artifactId>
      <version>8_282</version>
      <classifier>distrib</classifier>
      <type>tar.gz</type>
    </dependency>
  </dependencies>
</project>
```