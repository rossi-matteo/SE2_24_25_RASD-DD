# Software Engineering 2 Project - RASD & DD - Students & Companies 

## Information
These projects were done in the **Software Engineering 2 2024/2025** course during the MSc in Computer Science & Engineering at Politecnico di Milano.
The deliverables include a Requirements Analysis & Specification Document and a Design Document, as well as the slides used for the final presentation.

# Score
Final Score: 13.7 / 14

## 👥 Authors
* Riccardo Piantoni
* Matteo Rossi
* Jacopo Sacramone

## 📖 Overview
**Students & Companies (S&C)** is a comprehensive platform designed to act as an intermediary system facilitating the internship matching process between university students and companies. The transition from academia to the job market often presents significant difficulties for students trying to align their skills with industry expectations. S&C fills this gap by enabling companies to effectively reach emerging talent and students to find tailored internship opportunities.

## 🎯 Goals & Requests
The system is built to serve three primary actors: **Students**, **Companies**, and **Universities**. Key requirements and functionalities include:
* **Profile & Offer Management:** Students can create profiles and upload CVs. Companies can post detailed internship offers.
* **Smart Recommendations:** An automated system generating asymmetric recommendations. It matches student CVs to internship descriptions and vice versa.
* **Selection Process:** Integrated tools to schedule and conduct in-platform or in-person interviews and track candidates.
* **Monitoring & Feedback:** Tools to monitor ongoing internships, report issues to the University for resolution, and collect end-of-internship feedback.
* **Profile Optimization:** An LLM-powered module providing suggestions to help students make their CVs more appealing and to guide companies in optimizing their internship descriptions.

## ⚙️ Architecture & Data
The platform is designed with a highly scalable, distributed architecture:
* **Microservices:** Divided into bounded contexts (SecurityManager, ProfileManager, OfferManager, InterviewManager, IMFManager, RecommendationManager, OptimizationManager).
* **Polyglot Persistence:** * Relational databases (SecurityDBMS, ProfilesDBMS, InternshipsDBMS) for structured user and offer data.
  * Graph database (RecommendationsDBMS) to efficiently map and query relationships between skills, students, and offers.
* **Distributed Patterns:** Adopts an API Gateway (NGINX), Circuit Breaker patterns, and Event Sourcing via Change Data Capture (CDC) to maintain Eventual Consistency.

## 🚧 Challenges

Understanding the domain and architecting a solution at scale presented several key challenges, which are summarized in the table below:

| Challenge Area | Description | Solution / Approach |
| :--- | :--- | :--- |
| **Domain-Level Mismatch** | Academic skills and company requirements often lack direct intersection, making standard keyword matching ineffective. | Implemented asymmetric recommendation mechanisms and an Optimization Manager that uses an LLM to generate targeted profile/offer improvements. |
| **Data Consistency** | Maintaining consistency across isolated databases for different microservices without violating the CAP theorem's availability. | Adopted Eventual Consistency using Multi-Leader Replication and an Event Sourcing pattern with Apache Kafka to synchronize data across nodes. |
| **Logic Verification** | Ensuring complex business rules (e.g., recommendations are not generated spontaneously without new data) remain foolproof. | Conducted a rigorous Formal Analysis using **Alloy** to mathematically verify the state dynamics and constraints of the recommendation engine. |
| **Scalability & Resiliency** | The platform needs to handle traffic spikes during peak hiring seasons and gracefully handle isolated service failures. | Designed a containerized 4-Tier Architecture using dynamic orchestration, load balancers, and Circuit Breakers to isolate faults and prevent cascading failures. |
| **Complex Communication Lifecycles** | Managing the multi-step internship lifecycle (application, interview, ongoing, complaint, feedback) involving three distinct user roles. | Created dedicated state machines and independent microservices (`InterviewManager`, `IMFManager`) to securely manage private messages, interview scheduling, and university complaint resolution. |
