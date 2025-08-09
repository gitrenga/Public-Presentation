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

### 🗳️ Quick Poll
**Show of hands: Who writes tests BEFORE writing code?**
- Always 🙋‍♂️
- Sometimes 🤷‍♂️
- Rarely 😅
- Never 🙈
  
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

### 🎯 The Mindset Shift:
```
FROM: "How do I solve this problem?"
TO:   "How will I know when it's solved?"
```

---

## 💻 Live Coding Demo: Building with TDD
**(10 minutes)**

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

## 🏗️ Beyond Code: ODD (Operations-Driven Development)
**(6 minutes)**

---

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

---

### TDD Principles Apply Beyond Just Code:

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
- **Test the public interface** - What your users/callers see
- **One assertion per concept** - Clear failure diagnosis
- **Make tests readable** - They're documentation too
- **Refactor test code** - Keep it clean like production code

---
## 🚦 When to Use TDD (And When Not To)
**(3 minutes)**

### ✅ Perfect for TDD:
- **Business logic** - Your core value proposition
- **Complex algorithms** - Multiple edge cases
- **API endpoints** - Clear contracts
- **Bug fixes** - Write failing test, then fix
- **Critical paths** - Cannot afford to break

### ⏸️ Skip TDD For:
- **Spike/exploration code** - You're learning
- **Trivial getters/setters** - No business logic
- **Pure UI layouts** - Visual design work
- **Performance optimization** - Measurement-focused

### 🎯 The Decision Framework:
**Ask yourself**: 
- "Will this break user workflows if it fails?"
- "Are there multiple ways this could go wrong?"
- "Will I need to change this frequently?"

**If YES → Use TDD**

---

## 📊 TDD Success Stories: Real Impact
**(3 minutes)**

### Industry Data & Real Results:

**📈 Microsoft Study (2008-2012):**
- 15-35% increase in development time initially
- 40-90% decrease in defect density
- **ROI: 2-3x within 6 months**

**🏢 IBM Case Study:**
- 50% reduction in production defects
- 15% improvement in development productivity
- 25% faster time to market for new features

**💰 Business Impact:**
- **Defect Cost**: 10x cheaper to fix in development vs production
- **Developer Confidence**: 85% report higher confidence in changes
- **Technical Debt**: 60% reduction in code complexity

### 🎯 What This Means for Your Organization:
- **Developers**: Less debugging, more building
- **Leads**: Predictable delivery, fewer fire drills
- **Managers**: Lower maintenance costs, faster features
- **Architects**: Better design, cleaner interfaces
  
---

## 🚀 Organizational Adoption Strategy
**(4 minutes)**

### 🎯 Role-Specific Benefits & Actions:

#### For **Developers**:
**Benefits**: Less debugging time, more confident refactoring, clearer requirements
**Action**: Start with one small feature this sprint

#### For **Team Leads**:
**Benefits**: Predictable velocity, fewer production issues, better team morale
**Action**: Introduce TDD in retrospectives, track defect trends

#### For **Managers**:
**Benefits**: Reduced maintenance costs, faster feature delivery, lower risk
**Action**: Measure before/after metrics, support team training time

#### For **Architects**:
**Benefits**: Better system design, clearer interfaces, easier integration
**Action**: Promote testable architectures, review design patterns

### 📈 Adoption Roadmap:

**Week 1-2: Start Small**
- Pick one developer, one feature
- Write tests for new code only
- Measure initial metrics (defects, confidence)

**Month 1-3: Team Adoption**
- Team TDD workshops
- Pair programming with TDD
- Refactor legacy code when touching it

**Month 3-6: Organization-wide**
- Share success stories
- Cross-team knowledge sharing
- Make TDD part of definition of done

### 🛡️ Handling Common Resistance:

**"We don't have time"** → Start with bug fixes, show time savings
**"Tests slow us down"** → Measure debugging time before/after
**"Legacy code is too hard"** → Begin with new features only
**"Not everything needs tests"** → Agree! Focus on critical paths

---

## 🛠️ Getting Started Monday: Your Action Plan
**(3 minutes)**

### 🎯 Choose Your Adventure:

#### **Beginner Path** (Never done TDD):
1. **Pick the smallest feature** in your current sprint
2. **Write one test** before writing code
3. **Make it pass** with minimal code
4. **Notice how it feels** - the confidence boost
5. **Share the experience** with your team

#### **Intermediate Path** (Some TDD experience):
1. **Identify a complex business rule** you're working on
2. **List all edge cases** as test scenarios
3. **Implement with TDD** using red-green-refactor
4. **Compare** with your usual approach
5. **Teach someone else** what you learned

#### **Advanced Path** (Regular TDD user):
1. **Extend TDD to integration testing** with TestContainers
2. **Add contract testing** for your APIs
3. **Implement DORA metrics** testing in your pipeline
4. **Mentor team members** in TDD practices
5. **Present your success story** to leadership

### 📋 Success Checklist:
- [ ] Identified first TDD candidate
- [ ] Set up testing framework
- [ ] Written first failing test
- [ ] Made test pass
- [ ] Refactored with confidence
- [ ] Measured the difference
- [ ] Planned next steps

---

## 📊 Key Takeaways by Role
**(2 minutes)**

### 💻 **Developers**: Your Daily Toolkit
- TDD = Design technique first, testing second
- Red-Green-Refactor becomes muscle memory
- Confidence to experiment and refactor fearlessly

### 👥 **Team Leads**: Your Success Metrics
- Track defect reduction and cycle time improvement
- Use TDD for knowledge sharing and code reviews
- Build team confidence through shared practices

### 📈 **Managers**: Your Business Case
- 40-90% defect reduction (Microsoft data)
- 2-3x ROI within 6 months
- Faster feature delivery with lower risk

### 🏗️ **Architects**: Your Design Philosophy
- Testable code = better architecture
- Clear interfaces and loose coupling
- Foundation for microservices and cloud-native design

---

## ❓ Q&A & Interactive Discussion
**(5 minutes)**

### 🗳️ Final Poll: What's Your Next Step?
- Start TDD on current feature
- Set up testing framework
- Convince my team
- Need more training
- Ready to be TDD champion

### Common Questions Covered:
- **Speed concerns** → Show time savings data
- **Legacy code challenges** → Start with new features
- **Team adoption** → Begin with volunteers
- **Tool selection** → Use what fits your stack

---

## 🚀 Your TDD Transformation Starts Now

### The Promise:
Transform from **"I hope this works"** to **"I know this works"**

### Remember:
- **Start small**, build confidence
- **Focus on behavior**, not implementation
- **Test-drive your design** decisions
- **Share your wins** with the team

### The Ultimate Goal:
**Happy developers building reliable software with confidence**

---

**Thank you! Let's discuss your TDD journey!**

🔗 **Connect**: [LinkedIn](https://www.linkedin.com/in/rengasamy-venkittaraman/)

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
