```properties
# awsS3.yml import  
spring.config.import= classpath:/awsS3.yml
spring.profiles.include=aws
  
# S3 fileSize setting  
spring.servlet.multipart.max-file-size= 10MB
spring.servlet.multipart.max-request-size= 10MB
  
  
# H2 Console Setting  
spring.h2.console.enabled= true
spring.h2.console.settings.web-allow-others= true
spring.datasource.url=jdbc:h2:mem:testdb;MODE=MYSQL;
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.properties.hibernate.dialect= org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.properties.hibernate.globally_quoted_identifiers= true
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update

  
# MySQL Setting  
#spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
#spring.datasource.url=jdbc:mysql://prscsl4.cdslgeg4iosa.ap-northeast-2.rds.amazonaws.com/prscsl18  
#spring.datasource.username=g4dalcom  
#spring.datasource.password=rmaehd486  
#spring.jpa.properties.hibernate.globally_quoted_identifiers=true  
#spring.jpa.hibernate.ddl-auto=update  
  
  
# kakao login  
kakao.client_id = 1af8a4039402102e8193e16de2a1b4fb  
kakao.redirect_uri = http://localhost:3000/oauth/callback/kakao  
  
# REDIS  
spring.redis.host=43.201.78.63  
spring.redis.port=6379  
spring.redis.password=redredqq  
  
# JWT  
jwt.secretKey = ENC(nMqfM8muUnc9ykoMvtyc/NQUSDTZSPvi8OK9sxz7FD9rOa1zoqw9K3OlgSEgrn9K)  
  
# TOURIST OPEN API KEY  
restAPI.key=3t3ZRksGWDsuB9PCUSD%2F6uASFB3RfGSoi9vIxciQ1de2wb8sieQfeIGSkbz5nMctFfa8nw9j70VzLDd371WxcA%3D%3D

## Email Settings
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=repoyoume@gmail.com
spring.mail.password=wfgzzngnsoxvkdnw
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true

spring.mail.transport.protocol=smtp
spring.mail.debug=true
spring.mail.default.encoding=UTF-8
```