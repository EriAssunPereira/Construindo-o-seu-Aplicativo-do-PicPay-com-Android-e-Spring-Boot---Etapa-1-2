# Construindo-o-seu-Aplicativo-do-PicPay-com-Android-e-Spring-Boot-Etapa-1-2

Desenvolver um aplicativo semelhante ao PicPay envolve a criação de um sistema completo que permite realizar pagamentos entre usuários, visualizar histórico de transações e editar perfil. Utilizaremos Android Nativo com Kotlin para o desenvolvimento do aplicativo móvel e Spring Boot com Java 8 para construir o backend com API Rest. Vamos detalhar cada etapa desde a configuração inicial até a implementação das funcionalidades principais.

### Configuração Inicial do Projeto

1. **Setup do Projeto:**
   - Instale o Android Studio para desenvolvimento Android e configure o ambiente.
   - Configure um ambiente de desenvolvimento Java com JDK e Maven ou Gradle para o backend.

2. **Estrutura do Projeto:**
   Organize o projeto em módulos para facilitar a manutenção e expansão.
   ```
   /picpay-app
   ├── /android-app
   │   ├── /app
   │   │   ├── /src/main/java/com/seuapp
   │   │   │   ├── /ui
   │   │   │   │   ├── MainActivity.kt
   │   │   │   │   ├── PaymentActivity.kt
   │   │   │   │   ├── TransactionHistoryActivity.kt
   │   │   │   │   ├── ProfileActivity.kt
   ├── /backend
   │   ├── /src/main/java/com/seuapp
   │   │   ├── /controller
   │   │   │   ├── PaymentController.java
   │   │   ├── /model
   │   │   │   ├── User.java
   │   │   ├── /service
   │   │   │   ├── PaymentService.java
   ├── /database
   │   ├── /scripts
   │   │   ├── create_tables.sql
   ├── /docs
   ├── README.md
   ├── build.gradle
   ```

### Desenvolvimento do Backend com Spring Boot

1. **Configuração do Spring Boot:**
   - Configure o projeto Spring Boot para servir como backend RESTful.

   Exemplo básico de configuração (`PicpayApplication.java`):
   ```java
   // /backend/src/main/java/com/seuapp/PicpayApplication.java

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class PicpayApplication {

       public static void main(String[] args) {
           SpringApplication.run(PicpayApplication.class, args);
       }
   }
   ```

2. **Implementação dos Controllers e Services:**
   - Crie controladores REST para lidar com as operações de pagamento e perfil.
   - Implemente serviços para realizar a lógica de negócios.

   Exemplo de controller (`PaymentController.java`):
   ```java
   // /backend/src/main/java/com/seuapp/controller/PaymentController.java

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.*;
   import java.util.List;

   @RestController
   @RequestMapping("/api/payments")
   public class PaymentController {

       @Autowired
       private PaymentService paymentService;

       @GetMapping("/history/{userId}")
       public List<Payment> getPaymentHistory(@PathVariable Long userId) {
           return paymentService.getPaymentHistory(userId);
       }

       @PostMapping("/pay")
       public Payment makePayment(@RequestBody Payment payment) {
           return paymentService.makePayment(payment);
       }

       // outros métodos para atualizar, cancelar pagamentos, etc.
   }
   ```

   Exemplo de service (`PaymentService.java`):
   ```java
   // /backend/src/main/java/com/seuapp/service/PaymentService.java

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   import java.util.List;

   @Service
   public class PaymentService {

       @Autowired
       private PaymentRepository paymentRepository;

       public List<Payment> getPaymentHistory(Long userId) {
           return paymentRepository.findByUserId(userId);
       }

       public Payment makePayment(Payment payment) {
           // Implementação da lógica para realizar o pagamento
           return paymentRepository.save(payment);
       }

       // outros métodos de serviço para atualizar, cancelar pagamentos, etc.
   }
   ```

### Desenvolvimento do Aplicativo Android com Kotlin

1. **Configuração do Android Studio:**
   - Configure o projeto Android para se comunicar com o backend através de chamadas HTTP.

   Exemplo de configuração de chamada HTTP (`PaymentActivity.kt`):
   ```kotlin
   // /android-app/app/src/main/java/com/seuapp/ui/PaymentActivity.kt

   import android.os.Bundle
   import androidx.appcompat.app.AppCompatActivity
   import kotlinx.android.synthetic.main.activity_payment.*
   import retrofit2.Call
   import retrofit2.Callback
   import retrofit2.Response

   class PaymentActivity : AppCompatActivity() {

       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_payment)

           btnPay.setOnClickListener {
               val userId = 1 // Id do usuário logado, exemplo
               val amount = etAmount.text.toString().toDouble()
               val payment = Payment(userId, amount)

               // Chamada para API de pagamento
               val call = ApiClient.instance.makePayment(payment)
               call.enqueue(object : Callback<Payment> {
                   override fun onResponse(call: Call<Payment>, response: Response<Payment>) {
                       if (response.isSuccessful) {
                           // Tratamento de resposta bem-sucedida
                       } else {
                           // Tratamento de erro
                       }
                   }

                   override fun onFailure(call: Call<Payment>, t: Throwable) {
                       // Tratamento de falha na comunicação
                   }
               })
           }
       }
   }
   ```

### Integração e Teste Completo do Sistema

1. **Integração Frontend e Backend:**
   - Teste a integração entre o aplicativo Android e o backend Spring Boot para garantir que as operações de pagamento e perfil funcionem corretamente.

2. **Implementação de Recursos Adicionais:**
   - Implemente outras telas como Histórico de Transações e Edição de Perfil conforme necessário.

### Conclusão

Construir um aplicativo semelhante ao PicPay envolve uma arquitetura completa, com backend em Spring Boot para a lógica de negócios e API Rest, e frontend em Android Nativo com Kotlin para uma interface de usuário responsiva e eficiente. Este projeto não apenas demonstra a implementação prática de funcionalidades como pagamento entre usuários e gerenciamento de perfil, mas também prepara para explorar e expandir o sistema com novas funcionalidades e melhorias, seguindo boas práticas de desenvolvimento para garantir escalabilidade e robustez.
