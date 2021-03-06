---
title: Использование Java и JDBC с Базой данных SQL Azure
description: Узнайте, как использовать Java и JDBC с Базой данных SQL Azure.
services: sql-database
author: jdubois
ms.author: judubois
ms.service: sql-database
ms.subservice: development
ms.topic: quickstart
ms.devlang: java
ms.date: 06/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 829a106a643c10626a65855152375c349cd76f9a
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "87833595"
---
# <a name="use-java-and-jdbc-with--azure-sql-database"></a>Использование Java и JDBC с Базой данных SQL Azure

В этой статье показано, как создать пример приложения, которое использует Java и [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) для сохранения данных в [Базе данных SQL Azure](https://docs.microsoft.com/azure/sql-database/) и их извлечения из нее.

JDBC — это стандартный API Java для подключения к классическим реляционным базам данных.

## <a name="prerequisites"></a>Предварительные условия

- Учетная запись Azure. Если у вас ее нет, [получите бесплатную пробную версию](https://azure.microsoft.com/free/).
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) или [Azure CLI](/cli/azure/install-azure-cli) Мы рекомендуем использовать Azure Cloud Shell, так вы автоматически войдете в систему и получите доступ ко всем необходимым средствам.
- Поддерживаемый [пакет средств разработки Java (JDK)](https://aka.ms/azure-jdks) версии 8 (включена в Azure Cloud Shell).
- Средство сборки [Apache Maven](https://maven.apache.org/).

## <a name="prepare-the-working-environment"></a>Подготовка среды выполнения

Мы будем использовать переменные среды, чтобы минимизировать ошибки ввода и упростить дальнейшую настройку конфигурации в соответствии с требованиями.

Настройте следующие переменные среды с помощью следующих команд.

```bash
AZ_RESOURCE_GROUP=database-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_SQL_SERVER_USERNAME=demo
AZ_SQL_SERVER_PASSWORD=<YOUR_AZURE_SQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

Замените заполнители следующими значениями, которые используются в этой статье:

- `<YOUR_DATABASE_NAME>`: Имя сервера Базы данных SQL Azure. Оно должно быть уникальным в Azure.
- `<YOUR_AZURE_REGION>`: регион Azure, который вы будете использовать. Вы можете использовать `eastus` по умолчанию, но мы рекомендуем настроить регион, расположенный близко к месту проживания. Полный список доступных регионов можно получить, введя `az account list-locations`.
- `<AZ_SQL_SERVER_PASSWORD>`: Пароль для сервера Базы данных SQL Azure. Такой пароль должен содержать не менее восьми символов и включать знаки всех следующих типов: прописные латинские буквы, строчные латинские буквы, цифры (0–9) и небуквенно-цифровые знаки (!, $, #, % и т. д.).
- `<YOUR_LOCAL_IP_ADDRESS>`: IP-адрес локального компьютера, с которого будет запускаться приложение Java. Одним из удобных способов его поиска является ввод в обозревателе адреса [whatismyip.akamai.com](http://whatismyip.akamai.com/).

Чтобы создать группу ресурсов, выполните следующую команду:

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
  	| jq
```

> [!NOTE]
> Мы используем служебную программу `jq`, чтобы отобразить данные JSON и сделать их более удобочитаемыми. Эта программа устанавливается по умолчанию в [Azure Cloud Shell](https://shell.azure.com/). Если вам не нравится эта служебная программа, вы можете безопасно удалить `| jq` часть всех команд, которые мы будем использовать.

## <a name="create-an-azure-sql-database-instance"></a>Создание экземпляра Базы данных SQL Azure

Для начала мы создадим управляемый сервер Базы данных SQL Azure.

> [!NOTE]
> См. сведения о создании сервера Базы данных SQL Azure в руководстве по [ созданию отдельной базы данных в Базе данных SQL Azure](/azure/sql-database/sql-database-single-database-get-started).

Выполните следующую команду в [Azure Cloud Shell](https://shell.azure.com/):

```azurecli
az sql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --admin-user $AZ_SQL_SERVER_USERNAME \
    --admin-password $AZ_SQL_SERVER_PASSWORD \
  	| jq
```

Эта команда создает сервер Базы данных SQL Azure.

### <a name="configure-a-firewall-rule-for-your-azure-sql-database-server"></a>Настройка правила брандмауэра для сервера Базы данных SQL Azure

Экземпляры Базы данных SQL Azure по умолчанию защищены. В них включен брандмауэр, который блокирует все входящие подключения. Чтобы вы могли использовать нашу базу данных, добавьте правило брандмауэра, которое разрешит локальному IP-адресу обращаться к серверу базы данных.

Так как вы настроили локальный IP-адрес ранее, вы можете открыть брандмауэр сервера, выполнив следующую команду:

```azurecli
az sql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
  	| jq
```

### <a name="configure-a-azure-sql-database"></a>Настройка базы данных SQL Azure

Сервер Базы данных SQL Azure, который вы создали ранее, сейчас пустой. На нем отсутствует база данных, которую можно использовать с приложением Java. Создайте базу данных с именем `demo`, выполнив следующую команду:

```azurecli
az sql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server $AZ_DATABASE_NAME \
  	| jq
```

### <a name="create-a-new-java-project"></a>Создание нового проекта Java

Используя любую интегрированную среду разработки, создайте проект Java и добавьте файл `pom.xml` в его корневой каталог.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>

    <properties>
        <java.version>1.8</java.version>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.sqlserver</groupId>
            <artifactId>mssql-jdbc</artifactId>
            <version>7.4.1.jre8</version>
        </dependency>
    </dependencies>
</project>
```

Этот файл в формате [Apache Maven](https://maven.apache.org/) настраивает в проекте использование следующих компонентов:

- Java 8
- последняя версия драйвера SQL Server для Java.

### <a name="prepare-a-configuration-file-to-connect-to-azure-sql-database"></a>Подготовка файла конфигурации для подключения к Базе данных SQL Azure

Создайте файл *src/main/resources/application.properties* и добавьте в него следующее содержимое.

```properties
url=jdbc:sqlserver://$AZ_DATABASE_NAME.database.windows.net:1433;database=demo;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
user=demo@$AZ_DATABASE_NAME
password=$AZ_SQL_SERVER_PASSWORD
```

- Замените две переменные `$AZ_DATABASE_NAME` значением, которое вы настроили ранее.
- Замените переменную `$AZ_SQL_SERVER_PASSWORD` значением, которое вы настроили ранее.

### <a name="create-an-sql-file-to-generate-the-database-schema"></a>Создание файла SQL для генерации схемы базы данных

Для создания схемы базы данных мы будем использовать файл *src/main/resources/`schema.sql`* . Создайте такой файл со следующим содержимым:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

## <a name="code-the-application"></a>Добавление кода приложения

### <a name="connect-to-the-database"></a>Подключение к базе данных

Затем добавьте код Java, который будет использовать JDBC для сохранения данных в базу данных SQL Azure и их извлечения из нее.

Создайте файл *src/main/java/DemoApplication.java* со следующим содержимым.

```java
package com.example.demo;

import java.sql.*;
import java.util.*;
import java.util.logging.Logger;

public class DemoApplication {

    private static final Logger log;

    static {
        System.setProperty("java.util.logging.SimpleFormatter.format", "[%4$-7s] %5$s %n");
        log =Logger.getLogger(DemoApplication.class.getName());
    }

    public static void main(String[] args) throws Exception {
        log.info("Loading application properties");
        Properties properties = new Properties();
        properties.load(DemoApplication.class.getClassLoader().getResourceAsStream("application.properties"));

        log.info("Connecting to the database");
        Connection connection = DriverManager.getConnection(properties.getProperty("url"), properties);
        log.info("Database connection test: " + connection.getCatalog());

        log.info("Create database schema");
        Scanner scanner = new Scanner(DemoApplication.class.getClassLoader().getResourceAsStream("schema.sql"));
        Statement statement = connection.createStatement();
        while (scanner.hasNextLine()) {
            statement.execute(scanner.nextLine());
        }

        /*
        Todo todo = new Todo(1L, "configuration", "congratulations, you have set up JDBC correctly!", true);
        insertData(todo, connection);
        todo = readData(connection);
        todo.setDetails("congratulations, you have updated data!");
        updateData(todo, connection);
        deleteData(todo, connection);
        */

        log.info("Closing database connection");
        connection.close();
    }
}
```

Этот код Java, используя ранее созданные файлы *application.properties* и *schema.sql*, подключится к базе данных SQL Server и создаст схему для хранения данных.

Как видите, в этом файле мы закомментировали методы вставки, чтения, обновления и удаления данных. Эти методы мы создадим позже, и вы просто последовательно раскомментируете их.

> [!NOTE]
> Учетные данные для базы данных хранятся в свойствах *user* и *password* в файле *application.properties*. Эти учетные данные используются при выполнении `DriverManager.getConnection(properties.getProperty("url"), properties);`, так как файл свойств передается в качестве аргумента.

Теперь вы можете выполнить класс main в любом удобном инструменте.

- В любой среде IDE щелкните правой кнопкой мыши класс *DemoApplication* и выполните его.
- В Maven приложение можно запустить, выполнив команду `mvn exec:java -Dexec.mainClass="com.example.demo.DemoApplication"`.

Приложение должно подключиться к Базе данных SQL Azure, создать схему базы данных и закрыть подключение. При этом в журналах консоли вы увидите следующий результат.

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Closing database connection 
```

### <a name="create-a-domain-class"></a>Создание доменного класса

Создайте новый класс Java `Todo` рядом с классом `DemoApplication` и добавьте следующий код:

```java
package com.example.demo;

public class Todo {

    private Long id;
    private String description;
    private String details;
    private boolean done;

    public Todo() {
    }

    public Todo(Long id, String description, String details, boolean done) {
        this.id = id;
        this.description = description;
        this.details = details;
        this.done = done;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }

    @Override
    public String toString() {
        return "Todo{" +
                "id=" + id +
                ", description='" + description + '\'' +
                ", details='" + details + '\'' +
                ", done=" + done +
                '}';
    }
}
```

Этот класс является доменной моделью, сопоставленной с таблицей `todo`, которую вы создали при выполнении скрипта *schema.sql*.

### <a name="insert-data-into-azure-sql-database"></a>Вставка данных в базу данных SQL Azure

В файле *src/main/java/demoapplication.java* добавьте после метода main следующий метод, который вставляет данные в базу данных.

```java
private static void insertData(Todo todo, Connection connection) throws SQLException {
    log.info("Insert data");
    PreparedStatement insertStatement = connection
            .prepareStatement("INSERT INTO todo (id, description, details, done) VALUES (?, ?, ?, ?);");

    insertStatement.setLong(1, todo.getId());
    insertStatement.setString(2, todo.getDescription());
    insertStatement.setString(3, todo.getDetails());
    insertStatement.setBoolean(4, todo.isDone());
    insertStatement.executeUpdate();
}
```

Теперь можно раскомментировать две следующие строки в методе `main`.

```java
Todo todo = new Todo(1L, "configuration", "congratulations, you have set up JDBC correctly!", true);
insertData(todo, connection);
```

Теперь при запуске класса main вы увидите следующие выходные данные.

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Closing database connection
```

### <a name="reading-data-from-azure-sql-database"></a>Считывание данных из базы данных SQL Azure

Теперь мы считаем ранее вставленные данные, чтобы проверить работу нашего кода.

В файле *src/main/java/demoapplication.java* добавьте после метода `insertData` следующий метод, который считывает данные из базы данных.

```java
private static Todo readData(Connection connection) throws SQLException {
    log.info("Read data");
    PreparedStatement readStatement = connection.prepareStatement("SELECT * FROM todo;");
    ResultSet resultSet = readStatement.executeQuery();
    if (!resultSet.next()) {
        log.info("There is no data in the database!");
        return null;
    }
    Todo todo = new Todo();
    todo.setId(resultSet.getLong("id"));
    todo.setDescription(resultSet.getString("description"));
    todo.setDetails(resultSet.getString("details"));
    todo.setDone(resultSet.getBoolean("done"));
    log.info("Data read from the database: " + todo.toString());
    return todo;
}
```

Теперь можно раскомментировать следующую строку в методе `main`.

```java
todo = readData(connection);
```

Теперь при запуске класса main вы увидите следующие выходные данные.

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have set up JDBC correctly!', done=true} 
[INFO   ] Closing database connection 
```

### <a name="updating-data-in-azure-sql-database"></a>Обновление данных в базе данных SQL Azure

Давайте изменим ранее вставленные данные.

В том же файле *src/main/java/demoapplication.java* добавьте после метода `readData` следующий метод, который обновляет данные в базе данных.

```java
private static void updateData(Todo todo, Connection connection) throws SQLException {
    log.info("Update data");
    PreparedStatement updateStatement = connection
            .prepareStatement("UPDATE todo SET description = ?, details = ?, done = ? WHERE id = ?;");

    updateStatement.setString(1, todo.getDescription());
    updateStatement.setString(2, todo.getDetails());
    updateStatement.setBoolean(3, todo.isDone());
    updateStatement.setLong(4, todo.getId());
    updateStatement.executeUpdate();
    readData(connection);
}
```

Теперь можно раскомментировать две следующие строки в методе `main`.

```java
todo.setDetails("congratulations, you have updated data!");
updateData(todo, connection);
```

Теперь при запуске класса main вы увидите следующие выходные данные.

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have set up JDBC correctly!', done=true} 
[INFO   ] Update data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have updated data!', done=true} 
[INFO   ] Closing database connection 
```

### <a name="deleting-data-in-azure-sql-database"></a>Удаление данных из базы данных SQL Azure

Наконец, давайте удалим ранее вставленные данные.

В том же файле *src/main/java/demoapplication.java* добавьте после метода `updateData` следующий метод, который удаляет данные из базы данных.

```java
private static void deleteData(Todo todo, Connection connection) throws SQLException {
    log.info("Delete data");
    PreparedStatement deleteStatement = connection.prepareStatement("DELETE FROM todo WHERE id = ?;");
    deleteStatement.setLong(1, todo.getId());
    deleteStatement.executeUpdate();
    readData(connection);
}
```

Теперь можно раскомментировать следующую строку в методе `main`.

```java
deleteData(todo, connection);
```

Теперь при запуске класса main вы увидите следующие выходные данные.

```
[INFO   ] Loading application properties 
[INFO   ] Connecting to the database 
[INFO   ] Database connection test: demo 
[INFO   ] Create database schema 
[INFO   ] Insert data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have set up JDBC correctly!', done=true} 
[INFO   ] Update data 
[INFO   ] Read data 
[INFO   ] Data read from the database: Todo{id=1, description='configuration', details='congratulations, you have updated data!', done=true} 
[INFO   ] Delete data 
[INFO   ] Read data 
[INFO   ] There is no data in the database! 
[INFO   ] Closing database connection 
```

## <a name="conclusion-and-resources-clean-up"></a>Заключение и очистка ресурсов

Поздравляем! Вы создали приложение Java, которое использует JDBC для сохранения данных в базе данных SQL Azure и их извлечения из нее.

Чтобы очистить все ресурсы, используемые во время этого краткого руководства, удалите группу ресурсов с помощью следующей команды:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Дальнейшие действия

- [Разработка первой базы данных в Базе данных Azure SQL](design-first-database-tutorial.md)  
- [Драйвер Microsoft JDBC для SQL Server](https://github.com/microsoft/mssql-jdbc)  
- [Сообщите о проблеме или задайте вопросы](https://github.com/microsoft/mssql-jdbc/issues)  
