# Test-Driven Development (TDD)
## Transforming Fear into Confidence

---

## 👨‍💻 About Me & My TDD Journey
**(5 minutes)**

- **Rengasamy Venkittaraman**
- **18 Years Experience | Current Role: Architect**
- **The Accidental Engineer**
  - Late access to computers & software
  - Playing constant "catch up" on knowledge
  - Then I stumbled into a gem: **TDD**
  - From struggle to **happiness** in development

> *"TDD didn't just change my code - it changed my entire relationship with programming"*

---

## 🔄 What is TDD?
**(3 minutes)**

### The Simple Cycle:
```
🔴 RED → 🟢 GREEN → 🔵 REFACTOR
```

1. **🔴 RED**: Write a failing test
2. **🟢 GREEN**: Write minimal code to pass
3. **🔵 REFACTOR**: Clean up while keeping tests green

### Core Principle:
**Write tests BEFORE writing production code**

---

## 💔 The Pain → 💚 The Transformation
**(7 minutes)**

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

## 💻 Live Coding Demo: 
**(12 minutes)**

### Key Observations:
- ✅ Started with simplest possible implementation
- ✅ Each test forced better code
- ✅ Confidence to make changes
- ✅ Clear specification of behavior

---

## 🏗️ Beyond Code: ODD (Operations-Driven Development)
**(6 minutes)**

### TDD Principles Apply Beyond Just Code:

**🎯 The Big Picture:**
TDD is not just about unit tests - it's a mindset that extends to:
- **Environment setup**
- **Platform configuration** 
- **DevOps pipelines**
- **Architecture decisions**
- **Operations monitoring**

### Testable vs Non-Testable Scenarios

**❌ Non-Testable Approach:**
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

**✅ Testable Approach:**
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
**❌ EASILY Non-Testable Architecture**              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **✅ Testable Architecture** 

<img width="910" height="483" alt="ARCHITECTURE" src="https://github.com/user-attachments/assets/6fa88292-63ef-4e42-b96e-6b5a6ef02db5" />


### ODD: The Bigger Testing Picture

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

**🎯 Key ODD Scenarios to Test:**

1. **Long Running Processes** - Container health, graceful shutdown
2. **Parallel vs Sequential Execution** - Race conditions, deadlocks  
3. **Code & Dependency Paths** - Strategy pattern implementations
4. **Cache vs Database** - Consistency, fallback mechanisms
5. **Encryption Before Persistence** - Data security validation
6. **External Calls** - HTTP timeouts, retries, circuit breakers
7. **Garbage Collection** - Memory leak detection
8. **DORA Metrics** - Deployment frequency, lead time testing
9. **Architecture Constraints** - Dependency rules, layer violations

---

## 🏗️ Testing Strategy That Works
**(6 minutes)**

### The Test Pyramid (Simplified)

```
        🔺 Few E2E Tests (10%)
       /   Slow, Brittle, Expensive
      /
     🔷 Some Integration Tests (20%)
    /     Components working together
   /
  🟦 Many Unit Tests (70%)
 /       Fast, Reliable, Cheap
```

### Four Essential Test Types:

| Type | Purpose | Speed | When to Use |
|------|---------|-------|-------------|
| **Unit** | Test single function/class | ⚡ Very Fast | Always - your safety net |
| **Integration** | Test components together | 🚀 Fast | Database, APIs, file systems |
| **Component** | Test service in isolation | 🏃 Medium | Microservices, modules |
| **E2E** | Test complete user journey | 🐌 Slow | Critical business flows only |

### Golden Rules:
1. **Fast feedback first** - Unit tests run in milliseconds
2. **Test behavior, not implementation** - What it does, not how
3. **Independent tests** - No order dependencies
4. **One assertion per concept** - Clear failure messages

---

## ⚠️ Common TDD Pitfalls (Anti-Patterns)
**(4 minutes)**

### 🚨 Don't Do These:

**❌ Testing Implementation Details**
```java
// BAD: Testing internal structure
verify(userService).validateEmail(anyString());

// GOOD: Testing behavior  
User result = userService.createUser("test@example.com");
assertTrue(result.isValid());
```

**❌ Ice Cream Cone Testing**
- Too many slow E2E tests, few unit tests
- Fragile, expensive, slow feedback

**❌ 100% Coverage Obsession**
- Coverage ≠ Quality
- Focus on meaningful tests, not metrics

**❌ Order-Dependent Tests**
- Tests should run in any order
- Each test sets up its own data

**❌ Multiple Assertions Per Test**
- Hard to debug when they fail
- One concept per test

### ✅ Do These Instead:
- **Test behavior, not code**
- **Keep tests simple and focused**
- **Make tests readable as documentation**
- **Refactor test code too**

---

## 🎯 When NOT to Use TDD
**(2 minutes)**

### Skip TDD For:
- **Spike/Exploration code** - You're learning the domain
- **Trivial getters/setters** - No business logic
- **Pure UI layouts** - Visual design work
- **Performance experiments** - Optimization-focused code

### Use TDD For:
- **Business logic** - Your core value
- **Complex algorithms** - Multiple edge cases
- **API endpoints** - Clear input/output contracts
- **Bug fixes** - Write failing test first, then fix

---

## 🎯 Key Takeaways & Next Steps
**(3 minutes)**

### The TDD Mindset Shift:
```
FROM: "How do I solve this problem?"
TO:   "How will I know when it's solved?"
```

### Start Tomorrow:
1. **Pick a small feature** - Don't try to TDD everything at once
2. **Write the test first** - Just try it once
3. **Make it pass** - Simplest solution
4. **Notice the confidence** - Feel the safety net
5. **Refactor fearlessly** - Tests have your back

### 🛠️ Complete TDD Toolchain for Modern Development:

**☕ Java Ecosystem:**
- **Unit Testing**: JUnit 5 + AssertJ (fluent assertions)
- **Mocking**: Mockito (behavior verification)
- **Integration Testing**: TestContainers (real databases, message queues)
- **Architecture Testing**: ArchUnit (dependency rules, layer violations)
- **Build**: Maven Surefire/Failsafe plugins
- **Coverage**: JaCoCo with quality gates

**⚛️ React.js Frontend:**
- **Unit Testing**: Vitest + Testing Library (component behavior)
- **E2E Testing**: Playwright (cross-browser automation)
- **Visual Testing**: Storybook + Chromatic (UI regression)
- **API Mocking**: MSW (Mock Service Worker)
- **Performance**: Lighthouse CI (automated audits)

**☁️ Azure DevOps & Cloud:**
- **Pipeline Testing**: Azure DevOps REST API + Custom tasks
- **Infrastructure**: ARM Templates + Azure Resource Manager tests
- **Monitoring**: Application Insights + Custom telemetry
- **Load Testing**: Azure Load Testing service
- **Security**: Azure Security Center compliance tests
- **Deployment**: Blue-Green deployments with health checks

## ❓ Q&A
**(5 minutes)**

### Common Questions:

**Q: "Doesn't TDD slow you down?"**
A: Initially yes, long-term absolutely not. You spend less time debugging and more time building.

**Q: "What about legacy code without tests?"**
A: Start with new features. Add tests when you modify existing code.

**Q: "How do I convince my team?"**
A: Start with yourself. Demonstrate the confidence and speed gains.

---

## 🚀 Your TDD Journey Starts Now

### Remember:
- **TDD is a design technique**, not just testing
- **Start small**, build confidence
- **Focus on behavior**, not implementation
- **Refactor fearlessly** with your safety net

### The Promise:
Transform from **"I hope this works"** to **"I know this works"**

---

**Thank you! Questions?**

🔗 Contact: [Linked In]   (https://www.linkedin.com/in/rengasamy-venkittaraman/)

---

## 📋 Backup Slides

### TDD vs Other Approaches

| Approach | When Tests Written | Design Impact | Confidence Level |
|----------|-------------------|---------------|------------------|
| **Test Last** | After implementation | Minimal | Low |
| **Test Driven** | Before implementation | High | Very High |
| **Test Alongside** | During implementation | Medium | Medium-High |

### Testing Doubles Quick Reference

- **Mock**: Verifies interactions happened
- **Stub**: Returns predefined values  
- **Fake**: Working implementation (e.g., in-memory DB)
- **Spy**: Records how it was called

### Microservices Testing Strategy

```
Unit Tests → Component Tests → Contract Tests → E2E Tests
    ↑              ↑              ↑            ↑
  Fast &         Service      API Contracts  Full System
  Reliable      Behavior      Stay Aligned    Working
```

## 🌱 Spring Project TDD Toolkit

### Testing Tools Matrix by Layer & Intent

| Testing Layer | Primary Tools | Secondary Tools | Test Intent | Configuration | Example Use Cases | Coverage Target |
|---------------|---------------|-----------------|-------------|---------------|-------------------|-----------------|
| **Unit Tests (70%)** | JUnit 5, TestNG | Mockito, AssertJ, Hamcrest | Individual component logic testing | `@Test`, `@Mock`, `@InjectMocks`, `@DataProvider` | Service methods, business logic, utility functions | Fast feedback loop |
| **Integration Tests (20%)** | Spring Boot Test, TestNG | WireMock, TestContainers, H2, DBUnit | Component interaction within app | `@SpringBootTest`, `@AutoConfigureTestDatabase`, `@AutoConfigureWireMock` | Repository layer, service integration, API endpoints | Components working together |
| **Contract Tests** | Spring Cloud Contract, WireMock, Pact | TestNG, MockMvc | API contract validation | Contract DSL, `@AutoConfigureWireMock`, stub mappings | External service contracts, API versioning | Service boundaries |
| **Component Tests** | Spring Boot Test, TestNG | WireMock, TestContainers | Service in isolation | `@SpringBootTest(webEnvironment = MOCK)` | Microservices, modules, bounded contexts | Service behavior |
| **End-to-End Tests (10%)** | Selenium, TestNG, Cucumber | WireMock, TestContainers, Spring Boot Test | Complete workflow validation | `@SpringBootTest(webEnvironment = RANDOM_PORT)` | Critical business flows, user journeys | System integration |

### Specialized Testing Categories

| Category | Primary Tools | Secondary Tools | Purpose | Integration Pattern | When to Use |
|----------|---------------|-----------------|---------|-------------------|-------------|
| **Repository Testing** | Spring Data Test, JUnit 5 | TestContainers, H2, DBUnit | Data access layer validation | `@DataJpaTest`, `@AutoConfigureTestDatabase` | CRUD operations, custom queries, transactions |
| **Web Layer Testing** | Spring MVC Test, TestNG | MockMvc, WebTestClient, WireMock | Controller and web layer testing | `@WebMvcTest`, `@AutoConfigureMockMvc` | REST endpoints, request/response mapping |
| **Security Testing** | Spring Security Test, JUnit 5 | MockMvc, TestNG | Authentication and authorization | `@WithMockUser`, `@WithAnonymousUser` | Role-based access, JWT validation, CSRF |
| **Architecture Testing** | ArchUnit, JUnit 5 | TestNG | Architectural constraints validation | Custom rules, package scanning | Dependency rules, layer violations |
| **Performance Testing** | JMeter, Gatling | Spring Boot Actuator, Micrometer | Load and performance validation | Custom configuration, metrics collection | API throughput, response times |

### Azure DevOps & Cloud Testing

| Testing Aspect | Primary Tools | Secondary Tools | Purpose | Configuration | DORA Metrics Focus |
|----------------|---------------|-----------------|---------|---------------|-------------------|
| **Pipeline Testing** | Azure DevOps REST API, JUnit 5 | TestNG, Custom tasks | CI/CD pipeline validation | Personal access tokens, API calls | Lead Time for Changes |
| **Infrastructure Testing** | ARM Templates, TestContainers | Azure Resource Manager, JUnit 5 | Infrastructure as Code validation | Template validation, resource testing | Deployment Frequency |
| **Monitoring Testing** | Application Insights, JUnit 5 | Custom telemetry, TestNG | Observability validation | Custom metrics, health checks | Mean Time to Recovery |
| **Load Testing** | Azure Load Testing, JMeter | Gatling, performance counters | Scalability validation | Load test configuration | Change Failure Rate |
| **Security Testing** | Azure Security Center, JUnit 5 | Compliance tests, TestNG | Security compliance validation | Security policies, vulnerability scans | Security posture |

### Complete Spring Testing Setup

| Build Phase | Tools | Configuration | Purpose |
|-------------|-------|---------------|---------|
| **Dependencies** | Maven/Gradle | `spring-boot-starter-test`, `testng`, `wiremock-jre8`, `testcontainers` | Test framework setup |
| **Test Execution** | Maven Surefire/Failsafe | `testng.xml`, parallel execution, test groups | Test orchestration |
| **Coverage Analysis** | JaCoCo | Quality gates, branch coverage, exclusions | Code coverage validation |
| **Reporting** | TestNG Reports, Allure | HTML reports, test history, trends | Test result analysis |

---

## ⚛️ React.js Frontend TDD Toolkit

### Testing Tools Matrix by Layer & Intent

| Testing Layer | Primary Tools | Secondary Tools | Test Intent | Configuration | Example Use Cases | Coverage Target |
|---------------|---------------|-----------------|-------------|---------------|-------------------|-----------------|
| **Unit Tests (70%)** | Vitest, Jest | Testing Library, React Testing Library | Component logic & behavior testing | `vitest.config.js`, `@testing-library/react` | Component rendering, event handling, hooks | Individual components |
| **Integration Tests (20%)** | Testing Library, Vitest | MSW (Mock Service Worker), Cypress | Component interaction testing | API mocking, component integration | Form submissions, data fetching, routing | Component collaboration |
| **Visual Regression Tests** | Storybook, Chromatic | Percy, Applitools | UI consistency validation | `.storybook/main.js`, visual diff config | Design system components, responsive layouts | Visual consistency |
| **E2E Tests (10%)** | Playwright, Cypress | TestCafe | Complete user journey testing | Cross-browser configuration, page objects | Critical user flows, business scenarios | Full application workflows |
| **Performance Tests** | Lighthouse CI, Web Vitals | Bundlemon, webpack-bundle-analyzer | Performance validation | Lighthouse configuration, performance budgets | Core Web Vitals, bundle size, loading times | Performance metrics |

### Specialized React Testing Categories

| Category | Primary Tools | Secondary Tools | Purpose | Configuration | When to Use |
|----------|---------------|-----------------|---------|---------------|-------------|
| **Component Testing** | React Testing Library, Vitest | Enzyme (legacy), Jest | Individual React component testing | `@testing-library/react`, custom render | Props handling, state changes, lifecycle |
| **Hook Testing** | React Hooks Testing Library, Vitest | Testing Library utilities | Custom hooks validation | `@testing-library/react-hooks` | Custom hook logic, state management |
| **API Integration Testing** | MSW, Vitest | Nock, miragejs | API interaction testing | Service worker setup, request handlers | Data fetching, error handling, caching |
| **Accessibility Testing** | Jest-axe, Testing Library | Pa11y, axe-core | A11y compliance validation | Accessibility rules, screen reader testing | WCAG compliance, keyboard navigation |
| **State Management Testing** | Redux Toolkit, Zustand Testing | Recoil testing utilities | State management validation | Store configuration, action testing | Global state, complex state flows |

### Frontend Build & CI/CD Testing

| Testing Aspect | Primary Tools | Secondary Tools | Purpose | Configuration | Integration |
|----------------|---------------|-----------------|---------|---------------|-------------|
| **Build Testing** | Vite, Webpack | ESLint, Prettier | Build process validation | Build configuration, linting rules | Pre-commit hooks, CI pipeline |
| **Bundle Analysis** | Webpack Bundle Analyzer, Bundlemon | Source Map Explorer | Bundle optimization | Size budgets, tree shaking analysis | Performance monitoring |
| **Cross-Browser Testing** | Playwright, BrowserStack | Sauce Labs, LambdaTest | Browser compatibility | Browser matrix, device testing | Compatibility validation |
| **Security Testing** | npm audit, Snyk | OWASP ZAP, retire.js | Vulnerability scanning | Security policies, dependency scanning | Security compliance |

---
