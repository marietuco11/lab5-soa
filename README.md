[![Build Status](../../actions/workflows/CI.yml/badge.svg)](../../actions/workflows/CI.yml)

# Web Engineering 2025-2026 / Lab 5: Integration and SOA

A Spring Boot + Kotlin application demonstrating Enterprise Integration Patterns using Spring Integration. This project implements a message-driven architecture that routes numbers based on even/odd logic, showcasing multiple EIP patterns including Content-Based Router, Publish-Subscribe Channel, Message Filter, and Message Transformer.


## 🛠 Tech Stack

- **Spring Boot** 3.5.3
- **Spring Integration** (with Kotlin DSL)
- **Kotlin** 2.2.10
- **Java** 21 (toolchain)
- **Gradle** 8.5
- **Ktlint** for code quality

## ✅ Prerequisites

- Java 21 or higher
- Git

## 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/UNIZAR-30246-WebEngineering/lab5-soa.git
cd lab5-soa

# Build the project
./gradlew clean build

# Run the application
./gradlew bootRun
```

The application will start and begin processing messages. Observe the console output with emoji indicators to trace message flow through the integration patterns.

## 📁 Project Structure

```
lab5-soa/
├── src/main/kotlin/soa/
│   └── CronOddEvenDemo.kt          # Fixed Spring Integration flows
├── src/main/resources/
│   └── application.yml             # Application configuration
├── docs/
│   ├── GUIDE.md                    # Assignment instructions
│   └── EIP.png                     # Target EIP diagram (correct implementation)
├── diagrams/
│   └── before.png                  # EIP diagram of starter code (buggy version)
├── REPORT.md                       # Detailed analysis and documentation
└── README.md                       # This file
```

## 🎯 What This Application Does

The application demonstrates a complete integration flow with multiple patterns:

### Message Sources
1. **Atomic Incrementer**: Generates sequential numbers (0, 1, 2, 3...) every 100ms
2. **Gateway Injection**: Scheduled task injects negative random numbers every 1000ms

### Message Routing
- **Even numbers** (0, 2, 4...) → `evenChannel` → transformation → logging
- **Odd numbers** (1, 3, 5...) → `oddChannel` → filtering → transformation → logging + service
- **Negative numbers** (-77, -76...) → `numberChannel` → `oddChannel` → service (no transformation)

### Integration Patterns Used
- **Polling Consumer**: Polls atomic incrementer at fixed rate
- **Content-Based Router**: Routes numbers based on even/odd logic
- **Publish-Subscribe Channel**: Broadcasts messages to multiple subscribers
- **Message Filter**: Validates odd numbers before processing
- **Message Transformer**: Converts Integer → String format
- **Service Activator**: Connects to business logic
- **Messaging Gateway**: Provides clean API for message injection
## 🔧 Key Fixes Implemented

### Bug 1: Channel Type Mismatch
**Problem**: `oddChannel` was a DirectChannel (load-balancing)  
**Fix**: Changed to PublishSubscribeChannel to broadcast to all subscribers

### Bug 2: Inverted Filter Logic
**Problem**: Filter rejected odd numbers instead of accepting them  
**Fix**: Changed condition from `p % 2 == 0` to `p % 2 != 0`

### Bug 3: Gateway Routing Error
**Problem**: Gateway sent messages directly to evenChannel  
**Fix**: Created `numberChannel` as dedicated gateway entry point

### Bug 4: Unnecessary Complexity
**Problem**: Unused discardChannel added complexity  
**Fix**: Removed discardChannel and simplified flow

## 📊 Expected Output

When running correctly, you should see output like this:

```
🚀 Gateway injecting: -77
  🔧 Service Activator: Received [-77] (type: Integer)
📥 Source generated number: 0
🔀 Router: 0 → evenChannel
  ⚙️  Even Transformer: 0 → 'Number 0'
  ✅ Even Handler: Processed [Number 0]
📥 Source generated number: 1
🔀 Router: 1 → oddChannel
  🔍 Odd Filter: checking 1 → PASS
  ⚙️  Odd Transformer: 1 → 'Number 1'
  ✅ Odd Handler: Processed [Number 1]
  🔧 Service Activator: Received [Number 1] (type: String)
```

### Emoji Legend
- 🚀 Gateway injection
- 📥 Source generation
- 🔀 Router decision
- 🔍 Filter validation
- ⚙️ Message transformation
- ✅ Handler processing
- 🔧 Service activation

## 🧪 Code Quality

Format and check code style:
```bash
./gradlew ktlintFormat  # Format code according to Kotlin conventions
./gradlew ktlintCheck   # Check for style violations
```

## 📚 Learning Objectives Achieved

By completing this lab, I have:
- Understood Enterprise Integration Patterns (EIP) standard catalog
- Applied Spring Integration DSL using Kotlin
- Analyzed and debugged integration flows using EIP diagrams
- Created visual representations of integration architectures
- Fixed integration issues in message-driven systems
- Documented work with clear technical explanations

## 🎓 Assignment Documentation

Complete assignment documentation is available in:
- [REPORT.md](REPORT.md) - Detailed analysis, bug explanations, and learning outcomes
- [docs/GUIDE.md](docs/GUIDE.md) - Original assignment instructions
- [diagrams/before.png](diagrams/before.png) - EIP diagram of buggy starter code
- [docs/EIP.png](docs/EIP.png) - Target EIP diagram (correct implementation)

## 🔗 Useful Resources

### Enterprise Integration Patterns
- [EIP Pattern Catalog](https://www.enterpriseintegrationpatterns.com/patterns/messaging/)
- [Spring Integration Reference](https://docs.spring.io/spring-integration/reference/)
- [Spring Integration Kotlin DSL](https://docs.spring.io/spring-integration/reference/dsl/kotlin-dsl.html)

### Spring Integration Components
- [Message Channels](https://docs.spring.io/spring-integration/reference/channel.html)
- [Router](https://docs.spring.io/spring-integration/reference/router.html)
- [Filter](https://docs.spring.io/spring-integration/reference/filter.html)
- [Transformer](https://docs.spring.io/spring-integration/reference/transformer.html)
- [Service Activator](https://docs.spring.io/spring-integration/reference/service-activator.html)

## 🏆 Bonus Opportunities

This lab offers bonus opportunities for implementing additional EIP patterns. See [docs/GUIDE.md](docs/GUIDE.md) section 12 for details on:
- Content Enricher Pattern
- Splitter and Aggregator
- Dead Letter Channel
- Wire Tap
- Message History
- Dynamic Router
- Claim Check Pattern
- Idempotent Receiver
- Integration Testing Framework
- Metrics and Monitoring

## 👨‍💻 Author

**Your Name**  
Web Engineering 2025-2026  
Universidad de Zaragoza

## 📝 License

This project is part of the Web Engineering course at Universidad de Zaragoza.

## 🤝 Acknowledgments

- Course instructors for providing the assignment framework
- Spring Integration team for excellent documentation
- Enterprise Integration Patterns book by Gregor Hohpe and Bobby Woolf

---
