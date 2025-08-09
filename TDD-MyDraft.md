# TDD

# Presenter Intro & TDD Backdrop

  - Rengasamy Venkittaraman
  - Experience : 18 Years / Current Role : Architect
  - Accidental Engg. - Late access to computers & S/W
  - Catch up game on **Knowledge**   
  - Stumbled into a Gem - TDD - **Happiness**
    
# Live Coding

# Bitter & Sweet

  - Fear of Unknown - Problem of too many - Difficult Problem - End to End Solutioning Attempt
    
        No need to know - Manageable small steps - Who Cares - Evolutionary design
    
  - Fear & Lack of clarity - Comm. less - Shy Away from feedback
    
        Clarity on intent - helpful, concrete & Quick feedback
    
  - Tentative Programming - Dont touch a working code
    
        Confident Experiments - Aggresive refactoring - Safety Net
    
  - Requirement Misunderstanding - Written vs Meant
    
        Req - Acceptance Creteria - Verification Points - Test Case - TDD
    
  - Estimates
    
        Number of problems to solve VS Number of things to Test (3 VS 30/ 3 VS 5)
    
        Better breakdown & comm.

  - Documentation

        Live & Executable

   - Design

         Evolutionary,YAGNI, NO Premature Optimization

         Less Cognitive burden
      
# TDD Lego Blocks

  - Failing Test - Write Code - Passing Test - Refactor
  - Testable vs non Testable Code
  - ODD & Arch
```    
    1. Long Running Process
    2. Parallel vs Sequencial execution
    3. Code & Dependency Path - Stratergy Used
    4. Cache vs DB
    5. Encryption before persistence
    6. External Call (HTTP,DBQuery,Messaging,Async,Caching)
    7. Garbage Collection Frequency
    8. DORA Metrics 
    9. Arch Constraints
```

---

## 🏗️ **TEST ARCHITECTURE & STRATEGY TABLE**

| **Strategy** | **Scope** | **Speed** | **Maintenance** | **Feedback Quality** | **Recommended Ratio** |
|--------------|-----------|-----------|------------------|---------------------|----------------------|
| **Unit Tests** | Single unit/function | Very Fast | Low | Quick, specific | Many (70%) |
| **Integration Tests** | Multiple components | Medium | Medium | Component interaction | Some (20%) |
| **Broad Stack Tests** | End-to-end system | Slow | High | User journey validation | Few (10%) |
| **Component Tests** | Single component/service | Fast-Medium | Medium | Component behavior | Varies by architecture |
| **Contract Tests** | API boundaries | Fast | Low | Interface compliance | Per external dependency |

---

## 🎯 **TEST CLASSIFICATION DETAILED TABLE**

| **Test Type** | **Purpose** | **Scope** | **Typical Tools** | **Example Scenario** |
|---------------|-------------|-----------|-------------------|----------------------|
| **Unit Test** | Verify individual units work correctly | Single class/method | JUnit, NUnit, Jest | Testing a calculator function |
| **Integration Test** | Verify components work together | Multiple units | TestContainers, WireMock | Database + Service integration |
| **Broad Stack Test** | End-to-end user scenarios | Full application | Selenium, Cypress | Complete user registration flow |
| **Component Test** | Test service in isolation | Single service/component | Spring Boot Test | Testing a microservice |
| **Contract Test** | Verify API contracts | Service boundaries | Pact, Spring Cloud Contract | API between services |
| **Story Test** | Business acceptance criteria | Feature-level | Cucumber, SpecFlow | User story validation |
| **Business Facing Test** | Domain-oriented testing | Business process | FitNesse, Concordion | Business rule verification |
| **Subcutaneous Test** | Just below UI layer | Application without UI | Direct API calls | Testing without browser |
| **Threshold Test** | Performance/quality gates | Metrics monitoring | Custom tools | Performance regression detection |

---

## 🛠️ **TEST IMPLEMENTATION PATTERNS TABLE**

| **Pattern** | **Problem Solved** | **Implementation** | **Pros** | **Cons** |
|-------------|--------------------|--------------------|----------|----------|
| **Test Double** | External dependencies | Replace with controlled object | • Fast<br>• Reliable<br>• Controlled | • May not match real behavior |
| **Page Object** | UI test brittleness | Encapsulate page interactions | • Maintainable<br>• Reusable<br>• Readable | • Additional abstraction layer |
| **Object Mother** | Test data creation | Factory for test objects | • Consistent data<br>• Easy setup | • Can become complex |
| **Humble Object** | Untestable code | Move logic to testable parts | • Increases testability<br>• Separates concerns | • Additional complexity |
| **Clock Wrapper** | Time-dependent tests | Abstract system time | • Deterministic<br>• Fast | • Extra abstraction |

---

## 🔍 **TEST DOUBLES HIERARCHY**

| **Type** | **Behavior** | **Use Case** | **Verification** |
|----------|--------------|--------------|------------------|
| **Dummy** | No behavior, just fills parameter lists | Parameter filling | None |
| **Fake** | Working implementation, but not suitable for production | In-memory database | State verification |
| **Stub** | Provides canned answers to calls | Return predetermined values | State verification |
| **Spy** | Records information about how they were called | Capture method calls | Behavior verification |
| **Mock** | Pre-programmed with expectations | Verify specific interactions | Behavior verification |

---

## 🚀 **MICROSERVICES TESTING STRATEGY**

| **Test Level** | **Microservice Context** | **Challenges** | **Solutions** |
|----------------|--------------------------|----------------|---------------|
| **Unit Tests** | Individual service logic | Service dependencies | Mock external services |
| **Integration Tests** | Service + database | Multiple data stores | TestContainers |
| **Contract Tests** | API compatibility | Service evolution | Consumer-driven contracts |
| **Component Tests** | Service in isolation | External dependencies | Service virtualization |
| **End-to-End Tests** | Full system flow | System complexity | Minimal, focus on critical paths |

---

## 📈 **TEST IMPACT ANALYSIS (TIA)**

| **Aspect** | **Traditional Testing** | **Test Impact Analysis** |
|------------|-------------------------|--------------------------|
| **Scope** | Run all tests | Run only affected tests |
| **Speed** | Slow feedback | Fast feedback |
| **Resource Usage** | High | Optimized |
| **Complexity** | Simple | Requires call-graph analysis |
| **Accuracy** | 100% coverage | Risk of missing dependencies |

---

## 🎨 **MODERN TESTING APPROACHES**

| **Approach** | **Traditional View** | **Modern Approach** | **Benefits** |
|--------------|----------------------|---------------------|--------------|
| **Testing Phase** | After development | During/before development | Earlier feedback, better design |
| **Tester Role** | Separate QA team | Developers own testing | Faster feedback loops |
| **Test Environment** | Pre-production only | Production monitoring | Real-world validation |
| **Test Design** | Script-based | Exploratory + automated | Better bug discovery |
| **Quality Assurance** | Gate before production | Continuous monitoring | Ongoing quality assessment |

---
         
# Anti Patterns

  - Test Code NOT Behaviour
  - Test first & Test last both are TDD 
  - Only code is tested NOT Infra/Platform
  - 100% Test Coverage (Counter Productive) VS Meaningful Test
  - More Asserts in Single Test
  - Exposing only for Testing
  - Ice Cream Cone vs Test Pyramid
  - Tests are order dependent
  - Testing implementation (Too Many Test) - Test Cancer
    
# FAQ

  - What/WHY/HOW of TDD
  - When NOT to use TDD
  - Misconceptions of TDD
  - How Much TDD (Test Pyramid)
  - How to Measure TDD
  - ODD TDD BDD FDD

# 🎯 **KEY TAKEAWAYS**

### **DO:**
- ✅ Write self-testing code
- ✅ Follow test pyramid structure  
- ✅ Use appropriate test doubles
- ✅ Keep tests fast and reliable
- ✅ Write tests before or with code
- ✅ Monitor production systems
- ✅ Use exploratory testing
- ✅ Maintain clean test code

### **DON'T:**
- ❌ Rely only on broad stack tests
- ❌ Ignore flaky tests
- ❌ Focus only on coverage numbers
- ❌ Test implementation details
- ❌ Skip refactoring tests
- ❌ Neglect production monitoring
- ❌ Create brittle UI tests
- ❌ Over-mock dependencies

---

1. **Testing is a Design Activity**: Good tests improve code design and provide living documentation
2. **Balance is Key**: Use the right mix of different test types (Test Pyramid)
3. **Fast Feedback**: Prioritize fast, reliable tests for quick feedback loops  
4. **Production Matters**: Extend testing thinking into production with monitoring and observability
5. **Evolution**: Testing practices must evolve with architecture (monolith → microservices)
6. **Culture**: Foster a testing culture where quality is everyone's responsibility

# Cheet Sheet
<img width="1101" height="766" alt="image" src="https://github.com/user-attachments/assets/124069b0-3622-4531-8bee-5885f88691e1" />
