```yaml
version: "3"
services:
  # 项目 item
  sauceapp-portal:
    user: "$USERID:$GROUPID"
    image: tomcat:8
    container_name: sauceapp-portal
    depends_on:
      - sauceapp-mysql
      - sauceapp-redis
    environment:
      TZ: Asia/Shanghai
    ports:
      - 8081:8080
    volumes:
      - /projects/sauceapp/log_app/portal_api_logs:/logs
      - /projects/sauceapp/log_tomcat/portal_tomcat_logs:/usr/local/tomcat/logs
      - /projects/sauceapp/apps_tomcat/portal_api_webapps:/usr/local/tomcat/webapps
    restart: always
    networks:
      - sauceapp-network
networks:
  sauceapp-network:
    external: true
```

