# 📝 Logging Best Practices (To Avoid 3 AM Debugging)

> [!quote] Core Philosophy
> Good logging isn't about capturing everything; it's about capturing the right things in the right way at the right time.

---

## 🎯 Overview

> [!info] Why it matters
> Logging is the backbone of observability. Without a strategy, you’ll either drown in noise or miss critical signals.  

---

## ⚙️ Features

> [!tip] 1. Clear Game Plan & Objectives
>
>- **Don't:** Throw log statements everywhere randomly.
>    
>- **Do:** Before writing logs, ask:
>    
 >   - What are the application's main goals?
>        
 >   - What critical operations need monitoring?
>        
>    - What KPIs actually matter?
>
> - **Strategy:** Start by over-logging, then **trim back the noise**. Periodically review your logging strategy.
>

> [!note] 2. Use Log Levels Effectively
>
> - `INFO`: Business as usual (e.g., `User completed checkout, order #123`).
>
> - `WARNING`: Early warning system (e.g., `Payment processing taking longer than usual`).
>
> - `ERROR`: Real problems (e.g., `Database connection failed`).
>
> - `FATAL`: Application is crashing (e.g., `System out of memory, shutting down`).
>
> - **Pro Tip:** Run production at `INFO` by default but have a plan to **temporarily increase verbosity** (e.g., to `DEBUG`) when investigating issues.
>

> [!example] 3. Structured Logging
>
> - **Don't:** Use unstructured "wall of text" logs that are hard for machines to parse.
>
> - **Do:** Log in a structured format (like JSON) where every piece of data is a separate field.
>    
> - **Why:** Enables powerful filtering, searching, and analysis (e.g., "find all timeout errors from last Tuesday").
>    
> - **How:** Use logging frameworks or tools like **Vector** to transform unstructured logs.

> [!important] 4. **Provide Rich Context (The "Who, What, Where, Why")**
>
> - **Bad Log:** `"Error: Something went wrong"`
>
> - **Good Log:** Includes:
>    
 >   - **Request IDs** (for tracing across services)
>        
 >   - **User IDs**
 >       
 >   - **System state** (DB/Cache status)
>        
 >   - **Full error context** (stack traces)
>        
> - **Goal:** Your logs should be a "black box recorder" allowing you to replay and understand any scenario.
>

> [!warning] 5. Log Sampling to Control Costs
>
> - **Problem:** High-traffic systems can generate terabytes of logs, which is expensive to store.
>    
> - **Solution:** **Sample** logs (e.g., store only 20% of `INFO` logs) but be smart about it:
>    
>    - Keep **all** `ERROR` logs during an incident.
>        
>    - Sample non-critical logs more aggressively.
>        
> - **Tools:** Frameworks like **OpenTelemetry** have built-in sampling features.
>

> [!tip] 6. Correlate Events for easier Debugging Across Services
>
>- **Problem:** Logs are scattered across many microservices.
>    
>- **Solutions:**
>    
>    1. **Canonical Log Lines:** A single summary log entry per request that captures the entire story (outcome, user, duration, errors).
>        
 >   1. **Better Solution: Distributed Tracing** (e.g., with **OpenTelemetry**). This links all steps (spans) of a request across all services into one trace, providing a complete view.
>

> [!info] 7. Centralized & Aggregated Logs
>
> - **Don't:** SSH into 20 different servers to debug one issue.
>    
> - **Do:** Funnel all logs from all services into **one central place**.
>    
> - **Why:** Enables searching across everything at once and seeing how issues in one service cause failures in another.
>

> [!note] 8. Define Log Retention Policy
>- Not all logs need to be kept forever. Define rules based on importance:
>    
>    - **Error Logs:** Keep for 90 days.
>        
>    - **Debug Logs:** Keep for 7 days.
>        
>    - **Security/Audit Logs:** Keep for 1+ years.
>        
> - Move older logs to cheaper cold storage. **Set this up early** before cloud bills explode.

> [!danger] 9. Secure Your Logs
>
> - Logs contain sensitive data (user IDs, IPs, internal errors). Protect them:
>    
>    1. **Encryption in Transit:** Protect logs as they move to storage.
>        
>    2. **Encryption at Rest:** Protect stored logs.
>        
>    3. **Access Controls:** Restrict log access based on role (e.g., junior devs vs. security team). Use audit logs to track who accessed what.
>
  
> [!failure] 10. Never Log Sensitive Data
>
> - **Never log:** Passwords, API keys, credit card numbers, Social Security numbers, etc.
>    
> - **How to prevent:**
>    
>    - Use logging libraries that can redact specific fields (e.g., Go's `slog` package can be configured to only show user ID, not the entire user object).
>        
>    - Set up **filters** in your logging pipeline (e.g., in an **OpenTelemetry Collector**) to catch and redact sensitive data patterns _before_ they are stored.
>

> [!tip] 11. Mitigate Performance Impact
>
> - Logging has a cost (CPU, memory, I/O).
>    
> - **To minimize impact:**
>    
>    1. Choose an **efficient logging library** (e.g., Go's `slog` vs. `logrus` showed a 3% vs. 20% performance hit).
>        
>    2. Use **log sampling** in high-traffic paths.
>        
>    3. Log to a **separate disk partition** from your application.
>        
>    4. **Load test** to identify logging bottlenecks.
>

> [!example] 12. Logs vs. Metrics
>
> - **Logs:** Answer **"What happened?"** Best for deep-dive debugging of specific incidents.
>    
> - **Metrics:** Answer **"How often is this happening?"** Best for real-time health monitoring, spotting trends, and setting alerts.
>    
> - **Rule of Thumb:** Use **metrics** to know _when_ you have a problem. Use **logs** to figure out _why_ it happened.
>

---  

## 🚀 Getting Started

> [!tip] First Steps
>
> 1. Enable centralized logging.
>
> 2. Add request/user IDs.
>
> 3. Run production with `INFO` level logs.
>

---

## 🔧 Customization and Configuration

> [!info] Tune your setup
>
> - Configure **log sampling** rules per severity.
>
> - Apply **filters** to redact sensitive fields.
>
> - Tune **log retention policies** based on compliance & budget.
>

---

## 🔗 Related

> [!example] Tools & Standards
>
> - [OpenTelemetry](https://opentelemetry.io/) – tracing & logging standard.
>
> - [Vector](https://vector.dev/) – log transformation pipeline.
>  

---

## 🌍 Explore More

> [!abstract] Further Reading
>
> - Distributed Tracing concepts.
>
> - Observability triangle: Logs, Metrics, Traces.
>
> - Cost-optimized logging in cloud environments.
> 
---
## 📚 Tags

#logging #observability #debugging #opentelemetry #devops #best-practices