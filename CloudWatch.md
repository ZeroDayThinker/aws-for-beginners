---
it is a: monitoring and observability service.
---
### Key Terms:
 **Metrcs**: Time-series data (e.g., EC2 CPU Utilization every 5 mins)
 **Logs**: Text records from apps, OS, or AWS services (stored in Log Groups)
 **Alarms**: Trigger actions when a metric crosses a threshold (e.g., send email, stop instance)
 **Dashboard**:Custom views of your metrics (like a live operations center)
### Example:
 *You run a web server on EC2:*

- CloudWatch automatically tracks its CPU, disk, and network.
- Your app writes error logs → sent to CloudWatch Logs.
- You set an alarm: “If CPU > 80% for 10 minutes, notify me.”
- At 3 AM, traffic spikes → alarm triggers → you get an SMS → you scale up before users notice!
→ Proactive, not reactive.
### Steps:
📝 Steps:
**Step 1:** Launch a Test EC2 (Quickly!)
We need a running EC2 to see real metrics.
1. Go to EC2 Console → Launch Instance
2. Name: cloudwatch-test
3. AMI: Amazon Linux 2023
4. Instance type: t2.micro (Free Tier)
5. Skip key pair (we won’t connect—just monitor)
6. Click "Launch instance"
Wait ~1 minute for it to be "Running".

**Step 2:** Open CloudWatch Console
- Search for "CloudWatch" → Click CloudWatch under Services.

**Step 3:** View EC2 Metrics
1. In left menu, click "Metrics" → "All metrics"
2. Click "EC2" → "By Instance"
3. Find your instance (cloudwatch-test) → check "CPUUtilization"
4. You’ll see a live graph! (Data appears within 5 minutes)
>🔍 Try generating CPU load (optional):
If you connect via SSH, run:
`stress --cpu 1 --timeout 60s`
(But not required—you’ll still see idle CPU %)

**Step 4:** Create a Simple Alarm (Optional but Cool!)
1. In the CPU graph, click "Create alarm"
2. Metric: Keep CPUUtilization
3. Conditions:
	- Threshold type: Static
	- Whenever CPU is... Greater > 50
	- For 1 consecutive period (5 mins)
4. Notification: Skip (or create a test SNS topic if you want—but we’ll skip to keep it simple)
5. Click "Next" → Name: HighCPU-Test → "Create alarm"
✅ Now, if CPU ever goes above 50%, the alarm turns RED.
 ___
## security check list 🔒
### set up once ✅
- [ ] **Encrypt all log groups with KMS**
	→ CloudWatch > Log groups > Select each > Actions > Enable encryption → Choose your KMS key (not default)
- [ ] **Set log retention for every group**
	→ CloudWatch > Log groups > Select each > Actions > Set retention (e.g., 90 or 365 days — never “Never”)
- [ ] **Remove sensitive data at the source**
	→ In your app/agent config (e.g., Fluentd, Lambda, EC2) > Filter out passwords, tokens, or PII before sending to CloudWatch
- [ ] **Restrict IAM permissions tightly**
	→ IAM > Policies > Replace `logs:*` with only needed actions like l`ogs:PutLogEvents`, `logs:CreateLogGroup`
- [ ] **Disable public sharing of log groups**
	→ CloudWatch > Log groups > Actions > Never add a resource policy with `"Principal": "\*"`
### Every Day 🔄
- [ ] **Look for new unencrypted log groups**
	→ CloudWatch > Log groups > Check “Encryption” column → Fix any marked “Disabled”
- [ ] **Scan for logs with no retention set**
	→ CloudWatch > Log groups > Sort by “Retention” → Fix any set to “Never”
- [ ] **Verify no suspicious log queries**
	→ CloudWatch > Logs Insights > Check recent queries → Look for unknown users or odd patterns (via CloudTrail if needed)