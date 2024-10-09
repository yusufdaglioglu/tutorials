############################

############################
# DATABASE H2
############################

############################

# H2
Starting the server doesn't open a database. Databases are opened as soon as a client connects.

# dialect and driver
H2 server has it's own JDBC driver. It will not work with others.

But H2 server is compatible with different dialects. But the database connection should initialize with spesific mode. example JDBC URL compatible with MSSQLServer:

```
jdbc:h2:~/test;MODE=MSSQLServer
```

or it can be change anytime using simple SQL:

```sql
SET MODE MSSQLServer
```

H2 itself has it's own SQL dialect. It is enabled as default.

# modes

This term should not be confused with the "mode" property which is using on JDBC-URL.

- Embedded Mode

  does not opens a "H2 server". only 1 connection is available on the same database. Only the process itself can connect to DB.

- Server Mode (or remote mode or client/server mode)

  A standalone process starts the "H2 server" app. It opens JDBC or ODBC API via TCP.

- Mixed Mode

  "H2 server" starting inside from Java app itself. Other process can connect via TCP.

# Start server

- inside from Java app:

  ```java
  import org.h2.tools.Server;

  Server server = Server.createTcpServer(args).start();
  server.stop();
  ```

  This method calling "__Embedded Mode__".

- using standalone executable file:

  ```sh
  java -cp "/h2/bin/h2-1.4.199.jar" "org.h2.tools.Server"
  ```

  This method calling "__Server Mode (or remote mode or client/server mode)__".

# Connection URL & Mode
- Local file path:

  > JDBC:h2:~/file_path/test3

  On above example "~" means home directory of course.

  > JDBC:h2:file_path/test3

  On above example, the file path does not start with file seperator. Therefore it use the home directory as base path. Which means: $HOME/file_path/test3

- TCP

  > JDBC:h2:tcp://localhost/~/file_path/test2

- In-Memory

  Does not persist DB to a file. H2 uses only RAM.

  > JDBC:h2:mem:test1

# H2 Console
embedded to "h2.jar". It can be optionally started together with "h2 server". It serves a simple web page to connect and manage (execute query, view tables...) different databases. When the end-user try to connect to a database, the web page sends HTTPrequests to the H2 Console backend and the backend connects to DB via TCP and returns the results to the front-end.
