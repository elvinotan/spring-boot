# Spring-Boot

01. Pengenalan Spring Framework</br>
Website : https://spring.io/</br>
Spring Framework terdiri byk framework, tapi inti dari spring adalah Spring framework</br>
Untuk referensi lengkap lihat https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/</br>
![Spring Framework](https://github.com/elvinotan/spring-boot/blob/master/images/springframework.png)</br>

02. Inversion of Control and Dependency Injection</br>
Tradisional : Manual injection, passing lewat constractor atau setter getter</br>
Spring way : Injection object, butuh contractor kosong, Object bersifat single</br>
![Inversion of Control and Dependency Injection](https://github.com/elvinotan/spring-boot/blob/master/images/ioc.gif)</br>

Cara buat project Spring-Boot :</br>
a. Manually : Buat maven project dan pasang dependecies</br>
b. Website : https://start.spring.io/. Pilih dependencies dan import project</br>
c. STS > Spring starter project : Mirip dgn Website tapi sudah terintegrasi dgn editor</p>

Beda Spring Framework dan Spring Boot</br>
Spring Framework : XML Oriented</br>
Spring Boot : Object/Class Oriented</p>

Pada saat pembuatan project kita akan mendapatkan :</br>
a. Configurasi project ada di poem.xml</br>
b. Starter class SpringBootApplication.java</br>

```
@SpringBootApplication
public class SpringOauth2Application {

	public static void main(String[] args) {
		SpringApplication.run(SpringOauth2Application.class, args);
	}
}	
```
Untuk menjalankan :
a. Jalankan layaknya java main
b. Gunakna maven mvnw spring-boot:run