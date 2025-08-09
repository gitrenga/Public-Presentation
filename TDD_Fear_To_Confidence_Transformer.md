# Test-Driven Development (TDD)
## Transforming From Fear to Confidence

<img width="518" height="578" alt="Test-Agenda" src="https://github.com/user-attachments/assets/13bf25b2-524c-4e28-8ffe-01d69a9a4070" />

---
## 👨‍💻 About Me & My TDD Journey

- **Rengasamy Venkittaraman**
- **18 Years Experience | Current Role: Architect**
- 🔗 **Connect**: [LinkedIn](https://www.linkedin.com/in/rengasamy-venkittaraman/)
- **The Accidental Engineer**
  - Late access to computers & software
  - Playing constant "catch up" on knowledge
  - Then I stumbled into a gem: **TDD**
  - From struggle to **happiness** in development

> *"TDD didn't just change my code - it changed my entire relationship with programming"*
>
---

## 🔄 What is TDD?

<img width="401" height="231" alt="TDD-loop" src="https://github.com/user-attachments/assets/37926e82-eee9-46d9-8e92-797c1cc97517" />

### 🎯 The Mindset Shift:
```
FROM: "How do I solve this problem?"
TO:   "How will I know when it's solved?"
```
---
## 💻 Live Coding Demo: Building with TDD


### 🎭 Myth vs Reality Check

| **Myth** | **Reality** |
|----------|-------------|
| "TDD slows you down" | Initial investment, long-term speed gain |
| "100% coverage required" | Focus on behavior, not metrics |
| "Only for unit tests" | Applies to all testing levels |
| "Too complex for beginners" | Start small, build confidence |

### Key Observations from Demo:
- ✅ Started with simplest possible implementation
- ✅ Each test forced better code design
- ✅ Confidence to make changes immediately
- ✅ Clear specification of behavior
- ✅ Refactoring became fearless
---
## 💔 The Pain → 💚 The Transformation
### Before TDD: The Struggles

**🔥 Fear of the Unknown**
- Problem: Too many unknowns, difficult end-to-end solutions
- **TDD Solution**: No need to know everything - take manageable small steps

**😰 Lack of Clarity & Communication**
- Problem: Shy away from feedback, unclear requirements
- **TDD Solution**: Clear intent, concrete & quick feedback

**🚫 Tentative Programming**
- Problem: "Don't touch working code!"
- **TDD Solution**: Confident experiments with aggressive refactoring safety net

**📝 Requirements Misunderstanding**
- Problem: What's written vs what's meant
- **TDD Solution**: Requirements → Acceptance Criteria → Test Cases

**⏰ Poor Estimates**
- Problem: "How many problems to solve?" (vague)
- **TDD Solution**: "How many things to test?" (concrete: 3 vs 30, 3 vs 5)

**📚 Living Documentation**
- Problem: Outdated docs
- **TDD Solution**: Executable specifications that never lie
---
### Testable vs Non-Testable S/W
<img width="1530" height="531" alt="Screenshot from 2025-08-09 15-17-17" src="https://github.com/user-attachments/assets/6a315fdf-3de3-4265-bc58-89b88d182ff2" />

**CODE**

**❌ Non-Testable:**

```java
// Hard to test - direct external dependencies
public class OrderService {
    public void processOrder(Order order) {
        // Direct database call
        database.save(order);
        // Direct email service call  
        emailService.sendConfirmation(order.getEmail());
        // Direct payment processing
        paymentGateway.charge(order.getAmount());
    }
}
```

**✅ Testable:**
```java
// Easy to test - dependency injection
public class OrderService {
    private final OrderRepository repository;
    private final EmailService emailService;
    private final PaymentService paymentService;
    
    public OrderService(OrderRepository repository, 
                       EmailService emailService,
                       PaymentService paymentService) {
        this.repository = repository;
        this.emailService = emailService;
        this.paymentService = paymentService;
    }
    
    public OrderResult processOrder(Order order) {
        repository.save(order);
        emailService.sendConfirmation(order.getEmail());
        return paymentService.charge(order.getAmount());
    }
}

// Now we can test with mocks/stubs
@Test
public void shouldProcessOrderSuccessfully() {
    // Given
    OrderRepository mockRepo = mock(OrderRepository.class);
    EmailService mockEmail = mock(EmailService.class);
    PaymentService mockPayment = mock(PaymentService.class);
    
    when(mockPayment.charge(100.0)).thenReturn(PaymentResult.success());
    
    OrderService service = new OrderService(mockRepo, mockEmail, mockPayment);
    
    // When
    OrderResult result = service.processOrder(new Order(100.0));
    
    // Then
    assertTrue(result.isSuccess());
    verify(mockRepo).save(any(Order.class));
    verify(mockEmail).sendConfirmation(anyString());
}
```

**ARCHITECTURE**

**❌ Not-So-Testable**              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **✅ Testable** 

<img width="910" height="483" alt="ARCHITECTURE" src="https://github.com/user-attachments/assets/6fa88292-63ef-4e42-b96e-6b5a6ef02db5" />

---
### TDD Principles Apply Beyond Just Code:

<img width="673" height="683" alt="Test-Everything" src="https://github.com/user-attachments/assets/82a38158-1487-4053-bfcd-f7e14e9d491b" />


### 🌟 TDD is a Design Philosophy, Not Just Testing

The same principles that make code testable make systems:
- **More modular**
- **Easier to deploy**
- **Simpler to monitor**
- **Faster to debug**

### From Code to Systems: The Natural Evolution

**Testable Code Principles:**
- Dependency injection
- Single responsibility
- Clear interfaces
- Predictable behavior

**↓ Applied to Systems:**
- Container-based deployment
- Service-oriented architecture
- API-first design
- Observable operations
  

### Beyond Functional Test

**🔧 Infrastructure Testing:**
```java
// Test your Azure deployment with TestContainers
@Test
public void applicationShouldStartInAzureContainer() {
    try (GenericContainer<?> app = new GenericContainer<>("myapp:latest")
            .withExposedPorts(8080)
            .withEnv("AZURE_STORAGE_CONNECTION_STRING", azureConnectionString)) {
        app.start();
        assertTrue(app.isRunning());
        
        // Test Azure health endpoint
        Response response = given()
            .port(app.getMappedPort(8080))
            .when()
            .get("/health")
            .then()
            .statusCode(200)
            .extract().response();
            
        assertEquals("UP", response.jsonPath().getString("status"));
        assertNotNull(response.jsonPath().getString("azureStorage.status"));
    }
}
```

**🚀 Pipeline Testing (DORA Metrics Focus):**
```java
// Test your Azure DevOps pipeline against DORA metrics
@Test  
public void azureDevOpsPipelineShouldMeetDORAMetrics() {
    long startTime = System.currentTimeMillis();
    
    // Trigger Azure pipeline via REST API
    AzureDevOpsClient client = new AzureDevOpsClient(personalAccessToken);
    PipelineRun run = client.triggerPipeline("MyProject", pipelineId);
    
    // Wait for completion and measure
    PipelineResult result = client.waitForCompletion(run.getId());
    long deploymentTime = System.currentTimeMillis() - startTime;
    
    // Test Lead Time for Changes (Code commit to production)
    assertTrue("Lead time should be < 1 hour", 
               deploymentTime < Duration.ofHours(1).toMillis());
    
    // Test Deployment Frequency capability
    List<PipelineRun> recentRuns = client.getRecentRuns(24); // Last 24 hours
    assertTrue("Should support multiple daily deployments", 
               recentRuns.size() >= 3);
    
    // Test Mean Time to Recovery
    if (result.getStatus() == PipelineStatus.FAILED) {
        Duration recoveryTime = measureRecoveryTime(client, run);
        assertTrue("MTTR should be < 15 minutes", 
                   recoveryTime.toMinutes() < 15);
    }
    
    // Test Change Failure Rate from Azure DevOps analytics
    double failureRate = client.getChangeFailureRate(30); // Last 30 days
    assertTrue("Change failure rate should be < 15%", failureRate < 0.15);
}
```

**📊 Architecture Decision Testing:**
```java
// Test architectural constraints
@Test
public void servicesShouldNotHaveCircularDependencies() {
    ArchRule rule = classes()
        .that().resideInPackage("com.myapp.service..")
        .should().not().dependOnClassesThat()
        .resideInPackage("com.myapp.service..");
        
    rule.check(importedClasses);
}
```

**🎯 Key Non Functional Scenarios to Test:**

1. **Long Running Processes** - Container health, graceful shutdown
2. **Parallel vs Sequential Execution** - Race conditions, deadlocks  
3. **Code & Dependency Paths** - Strategy pattern implementations
4. **Cache vs Database** - Consistency, fallback mechanisms
5. **Encryption Before Persistence** - Data security validation
6. **External Calls** - HTTP timeouts, retries, circuit breakers
7. **Garbage Collection** - Memory leak detection
8. **DORA Metrics** - Deployment frequency, lead time testing
9. **Architecture Constraints** - Dependency rules, layer violations
10. **NFR** - HA,Fault tolerance etc..

---

## 🏗️ Testing Strategy That Works

<img width="692" height="348" alt="Test-Pyramid" src="https://github.com/user-attachments/assets/58078528-c1ab-4c58-853b-5bab45bacf36" />

### Golden Rules:
1. **Fast feedback first** - Unit tests run in milliseconds
2. **Test behavior, not implementation** - What it does, not how
3. **Independent tests** - No order dependencies
4. **One assertion per concept** - Clear failure messages

---


