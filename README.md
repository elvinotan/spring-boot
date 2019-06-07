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


03. Membuat Project Spring </br>
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
Untuk menjalankan :</br>
a. Jalankan layaknya java main</br>
b. Gunakna maven ```mvnw spring-boot:run```</br>
c. Gunakna maven, untuk generate runable jar  ```mvnw clean package```</br>

04. Container</br>
Pada Spring semua object yang di create di sebut dgn @Bean, yang akan di simpan dalam container</br>
Pada saat pertama kali jalan SpringBoot akan menload semua konfigurasi yang ada di @SpringBootApplication dan semua class di dalam pathnya

05. Bean</br>
Pembuatan object degan IoC
```
	@Bean
	private DataBean getDataBean() {
		return DataBean();
	}
	
	//dilain tampat
	@Autowired
	private Databean dataBean;
	
	//dilain tampat
	Application context = ...;
	DataBean bean = context.getBean(DataBean.class)
```
Penamaan method pada anotasi @Bean bebas, krn yang dilihat adalah object returnan</br>
Bila ada 2 Object yang sama yang di return akan bentrok, kita harus pake qualifier untuk membedakannya</br>
Gunakan @Autowired untuk mengambil object dari container, atau lewat applicationContext</br>

06. Dependency Injection
