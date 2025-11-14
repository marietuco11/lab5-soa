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


## 📚 Learning Objectives Achieved

By completing this lab, I have:
- Understood Enterprise Integration Patterns (EIP) standard catalog
- Applied Spring Integration DSL using Kotlin
- Analyzed and debugged integration flows using EIP diagrams
- Created visual representations of integration architectures
- Fixed integration issues in message-driven systems
- Documented work with clear technical explanations


## Bonus opportunities

Be the first to complete **at least two** of the following tasks to earn a bonus:

### 1. **Content Enricher Pattern**

- **Description**: Implement a Content Enricher that adds additional data to messages as they flow through the system.
- **Implementation**: Add a content enricher that augments messages with metadata (timestamp, message ID, or external data lookup).
- **Goal**: Demonstrate understanding of the Content Enricher pattern for message enhancement.
- **Benefit**: Shows mastery of message enrichment patterns in integration scenarios.

### 2. **Splitter and Aggregator**

- **Description**: Implement message Splitter and Aggregator patterns to process composite messages.
- **Implementation**: Split a batch of numbers into individual messages, process them separately, then aggregate results.
- **Goal**: Master composite message processing patterns.
- **Benefit**: Demonstrates understanding of parallel processing and result consolidation in integration flows.

### 3. **Dead Letter Channel**

- **Description**: Implement proper error handling with a Dead Letter Channel for failed messages.
- **Implementation**: Add error handling that routes failed messages to a dead letter channel with retry logic.
- **Goal**: Implement robust error handling in integration flows.
- **Benefit**: Shows understanding of enterprise-grade error handling and recovery patterns.

### 4. **Wire Tap**

- **Description**: Implement a Wire Tap to monitor messages without affecting the main flow.
- **Implementation**: Add wire taps to observe message content at key points without altering flow behavior.
- **Goal**: Enable non-intrusive monitoring of integration flows.
- **Benefit**: Demonstrates understanding of observability patterns in message-driven systems.

### 5. **Message History**

- **Description**: Track message history as messages flow through the integration system.
- **Implementation**: Add message history tracking that records each component a message passes through.
- **Goal**: Enable message flow traceability for debugging and auditing.
- **Benefit**: Shows understanding of message tracking and audit trail patterns.

### 6. **Dynamic Router**

- **Description**: Implement a Dynamic Router that can change routing rules at runtime.
- **Implementation**: Create a router with configurable rules that can be updated without restarting the application.
- **Goal**: Demonstrate runtime reconfiguration of integration flows.
- **Benefit**: Shows mastery of adaptive integration patterns and dynamic configuration.

### 7. **Claim Check Pattern**

- **Description**: Implement the Claim Check pattern to handle large message payloads efficiently.
- **Implementation**: Store large payloads externally and pass references through the flow, retrieving content when needed.
- **Goal**: Optimize message processing for large payloads.
- **Benefit**: Demonstrates understanding of performance optimization in integration systems.

### 8. **Idempotent Receiver**

- **Description**: Implement an Idempotent Receiver to handle duplicate messages safely.
- **Implementation**: Add idempotency support that detects and handles duplicate messages without side effects.
- **Goal**: Ensure message processing reliability in unreliable network conditions.
- **Benefit**: Shows understanding of reliable messaging patterns and duplicate detection.

### 9. **Integration Testing Framework**

- **Description**: Create a comprehensive integration testing suite for Spring Integration flows.
- **Implementation**: Use Spring Integration Test support to write tests that verify flow behavior, message routing, and transformations.
- **Goal**: Ensure integration flow correctness through automated testing.
- **Benefit**: Demonstrates testing strategies for message-driven architectures.

### 10. **Metrics and Monitoring**

- **Description**: Add comprehensive monitoring and metrics collection for integration flows.
- **Implementation**: Integrate Spring Boot Actuator and Micrometer to expose metrics about message rates, processing times, and error rates.
- **Goal**: Enable production observability of integration flows.
- **Benefit**: Shows understanding of operational concerns in integration systems.
