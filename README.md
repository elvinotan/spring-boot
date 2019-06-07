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
Gunakan @Autowired untuk mengambil object dari container, atau lewat applicationContext</br>

06. Dependency Injection</br>
Otomatis injection lewat parameter DataBean</br>
```
	@Bean
	private DataBean getDataBean() {
		return DataBean();
	}
	
	@Bean
	private SampleBean createSampleBean(DataBean databean) {
		return new SampleBean(databean);
	}
	
	//dilain tampat
	Application context = ...;
	SampleBean bean = context.getBean(SampleBean.class);
	System.out.println(bean.dataBean);
```

07. Dependency Injection 2</br>
Untuk container bisa membedakan antara object a dan b, itu berdasarkan pada classname,</br>
sehingga bila ada 2 deklarsi seperti dibawah ini makan akan error, untuk mengatasi
hal ini kita perlu membedakannya dgn memberi nama pada bean dgn syarat pada saat ambil juga harus tau berdasarkan apa, 
bisa juga menggunakan @Primary</br>
```
	@Bean
	private DataBean getDataBean() {
		return DataBean();
	}
	
	@Bean
	private DataBean getDataBean2() {
		return DataBean();
	}
	
	@Bean
	private SampleBean createSampleBean(DataBean databean) {
		return new SampleBean(databean);
	}
	
	//dilain tampat
	Application context = ...;
	SampleBean bean = context.getBean(SampleBean.class);
	System.out.println(bean.dataBean);
```

```
	@Bean(name = "bean1")
	private DataBean getDataBean() {
		return DataBean();
	}
	
	@Bean(name = "bean2")
	private DataBean getDataBean2() {
		return DataBean();
	}
	
	@Bean
	private SampleBean createSampleBean(@Qualifier("bean1") DataBean databean) {
		return new SampleBean(databean);
	}
	
	//dilain tampat
	Application context = ...;
	SampleBean bean = context.getBean(SampleBean.class);
	System.out.println(bean.dataBean);
```

08. Dependency Injection 3</br>
Excercise : Buat OtherBean yang dependence ke DataBean('bean2') dan SampleBean</br>

09. Circular Dependency</br>
Object A depend B</br>
Object B depend C</br>
Object C depend A</br>
@Configuration : Menandakan ini adalah class Configuration yg akan di inisitate pertama kali</br>
Agar configurasi class ke load kita gunakan anotation @Import({Classname}.class) di SpringBootApplication</br>

10. Component</br>
@ComponentScan = Spring akan meng-scan semua package classpath dan bila ada tag @Component akan di create dan dimasukan dalam container</br>
```
	@Component
	public class DataBean() {}
	
	//dilain tampat
	Application context = ...;
	DataBean bean = context.getBean(DataBean.class);
	System.out.println(bean);

```
11. Component Injection</br>
Spring mewajibkan untuk membuat constractor kosong, terkecuali di define @Autowired</br>
Bila object tidak memiliki Constractor, sebenarnya ada constractor 1,yaitu constractor kosong</br>
```
@Component
public class SayHello {
	private DataBean bean;
	
	@Autowired
	public SayHello(DataBean bean) {
		this.bean = bean;  
	}
```
Pastikan saja untuk DataBean di beri anotasi @Component sehigga bisa di generate oleh spring</br>

12. Scope</br>
Scope ini mengacu pada bagimana spring memperlakukan object pada saat object itu diminta</br>
@Scope("singleton") : Object hanya di buata sekali dan setiap permintaan akan oper object yang sama, Secara default spring menerapkan prinsip ini</br>
@Scope("prototype") : Object akan selalu di buat baru pada saat permintaan</br>
@Scope("session") : Object akan di buat setiap instance session di buat di web framework</br>
@Scope("request") : Object akan di buat setiap ada request di web framework</br>

```
@Bean
@Scope("prototype")
public DataBean createBean() {
	return new DataBean();
}
```
Scope juga bisa di pasang pada class</br>
```
@Component
@Scope('prototype')
public class OtherNewBan {
}
```

13. Lifecycle</br>
Kadang kita perlu suatu event yang dijalankan pada saat setelah Bean di buat, dan saat Bean sebelum di Destroy</br>
Spring mendukung ini dgn menggunakan anotation yg di pasang pada suatu method</br>
@PostConstract : Akan menjalankan method setelah object di constract</br>
@PreDestroy  : Akan menjalankan method ini sebelum object di destroy</br>
PreDestory hanya di panggil apabila object tersebut bersifat singleton, untuk object yang bersifat prototype akan menjadi tanggung jawab programer</br>
```
public class ObjectBean {
	@PostConstract 
	public void init() {}
	
	@PreDestroy 
	public void destory() {}
}
```

14. Aware</br>
Sebuah interface yang dapat kita implemenetasi untuk mengakcess object yang ada di awareness tsb</br>
```
public class DataBean implements ApplicationContextAware{
	@Override
	public void setApplicationContext(ApplicationContext app) throws BeansException {
		this.app = app;
	}
}
```
Selain ApplicationContextAware spring juga meng-support untuk aware yang lain, untuk info lebih lanjut mohon lihat referensi</br>

15. Extensions Point</br>
Kalo pada Lifecycle kita memiliki hak access untuk @PostConstract dan @PreDestory, namun event ini terjadi pada object itu sendiri</br>
Bagaiman kalo kita ingin event yang sama tapi dari pihak luar dari object itu, maka yang kita perlukan adalah Extensions Point</br>
Extensions Point memiliki byk macam, kita akan bahas 1 saja</br>
```
@Component
public class LogPlugin extends InstantiationAwareBeanPostProcessorAdapter {
	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("Created object "+beanName);
		retur bean;
	}
}
```

16. Profile</br>
Profile ini buat untuk menentukan aplikasi berjalan dalam mode apa</br>
Pada umumnya ini di gunakan devleoper untuk membedakna antara development dan production</br>
Kita bisa menggunakan profile dgn memberikan anotation @Profile</br>
```
@Bean
@Profile("dev")
public Data getDataDev() {
	return new Data();
}

@Bean
@Profile("prod")
public Data getDataProd() {
	return new Data();
}
```
Mirip dengan @Qualifier, kita butuh mendeklarasikan mode apa yang kita kan gunakan, bisa dgn application.properties maupun lewat program</br>
```
System.setProperty("spring.profiles.active", "development");
```
Apabila developer tidak membrikan property maka secara default, spring mencari profile default, dan bila tidak ada juga maka akan error</br>
Developer juga dapat memberikan nilai property pada saat menjalankan jar</br>
```
-Dspring.profiles.active=dev
```

17. Internationalization
Tujuannya untuk meng-support berbagai macam bahasa
```
Buat file properties di folder resources
	/messages/hello.properties
	/messages/hello_id_ID.properties
	
@Bean(name="messageSource")
public MessageSource createMessgeSource() {
	ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
	messageSource.setBasename("messages.hello");
	return messageSource;
}	

public class SayHello implements MessageSourceAware {

	private MessageSource ms;

	public String hello (String name) {
		return ms.getMessage("hello", null, Locale.getDefault())+name;
	}
	
	@Override
	public void setMessageSource(MessageSource ms) {
		this.ms = ms;
	}
}

```
Cara ubah locale secara progamtically
```
	Locale.ssetDefault(new Locale("in", "ID"));
```
Message juga bis meng-inject parameters</br>

18. Resource Loader</br>
Class yang di buat oleh spring untuk meng-access suatu resource</br>
Resource bisa berbagai bentuk misal : file, http, ftp dll....</br>
Kita cukup menggunakan deklarsikan parameter alamat resource dan spring akan tau jenis resource apa yang kita coba akses (lihat refrence untuk lebih lanjut)</br>
```
@Component
public class FileBean implements ResourceLoaderAware {
	private ResourceLoader loader;
	
	@Override
	public void setResourceLoader(ResourceLoader loader) {
		this.loader = loader;
	}
	
	public void printInfo() throws IOException {
		Resource resource = loader.getResource("classpath:/resources/info.txt");
		Scanner scanner = new Scanner(resource.getInputStream());
		while (scanner.hasNextLine()) {
			String line = scanner.nextline();
			System.out.println("Line "+line);
		}
		scanner.close();
	}
}
```

19. Property Source</br>
Tool yang di buat untuk meng-access configuration property file</br>
```
@Configuration
@PropertySource("classpath:/com/app.properties")
public class Config{
	@Autowire
	Enviroment env;
	
	public String getPropery() {
		return env.getProperty("application.name");
	}
}	
```
Bila ingin meload lebih dari 1 properties file bisa dilakukan dgn cara</br>
```
@Configuration
@PropertySources({
	@PropertySource("classpath:/com/app.properties")
	@PropertySource("classpath:/com/app1.properties")
})
```
Atau dengan menggunkan @Value yang mengambil default application.properties</br>
```
@Value("${application.name}")
private String applicationName;
```	