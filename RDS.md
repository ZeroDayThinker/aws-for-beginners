**it is a**: *relational database service | managing the database*
---
### Key Terms:
**Multi-AZ deployment**: High availability (auto-failover if primary fails)

**Read Replicas**: Scale read-heavy apps (e.g., dashboards, reports)

**Security**: Encryption at rest & in transit, VPC isolation
### Example:
*You’re building a blog app:*

- Users register → data stored in RDS (PostgreSQL)
- AWS handles:
	- Daily backups
	- Security patches
	- Scaling storage
- Your app connects via a database endpoint (like `mydb.abc123.us-east-1.rds.amazonaws.com`)
→ No DBA needed!

### Steps:
 **Step 1:** Open RDS Console
- In AWS Console, search for "RDS" → Click RDS under Services.
- Click "Create database".
**Step 2:** Configure Your Database
1. Engine options: Choose PostgreSQL
2. Templates: Select "Free tier"
3. Settings:
	- DB instance identifier: `my-first-rds`
	- Master username: `admin`
	- Master password: `mypassword123` (use something strong in real life!)
4. DB instance size: Should auto-select db.t2.micro (Free Tier)
5. Storage: Keep 20 GiB (minimum, included in Free Tier)
6. Connectivity:
	- Compute resource: Leave as "Don’t connect"
	- VPC: Default is fine
	- Public access: ❌ Keep "No" (best practice private DB)
	- VPC security group: Create new
7. Authentication: Keep password-based
8. Additional configuration:
	- Initial database name: `mydb`
	- Backup retention: 1 day (minimum)
9. Click "Create database"

⏳ Wait 2–5 minutes while AWS provisions it (status = "Creating").

🔍 You’ll see it in the list with status "Available" when ready.

**Step 3**: Observe (Don’t Connect It’s Private!)
Since we set Public access = No, you can’t connect from your laptop and that’s good!
In real apps, your EC2 or Lambda would connect from inside the same VPC.

But you can still:

See the endpoint (under “Connect”)
View monitoring metrics (CPU, storage, connections)
Check automatic backups
✅ This shows how RDS works securely.
___
## security check list 🔒
### set up once ✅
- [ ] 1. **Disable public access when creating DB**  
   
	→ RDS > _Databases_ > Launch DB > Set “Publicly accessible” = No
- [ ] **Put RDS in a private subnet**  
 
	→ During setup > _Connectivity_ > Choose private subnets only (no public subnets)
- [ ] **Use a tight security group**  
 
	→ RDS > _Databases_ > Select DB > _Connectivity & security_ > Edit VPC security group → Only allow app servers’ IPs/SGs on DB port (e.g., 3306, 5432)
- [ ] **Enable encryption at rest**  

	→ When launching DB > _Additional configuration_ > Check “Enable encryption” (uses KMS)
- [ ] **Enable auto backups + deletion protection**  

	→ RDS > _Databases_ > Modify > Set backup retention (7+ days) + Enable “Deletion protection”
### Every Day 🔄
- [ ] **Check for publicly accessible DBs**

	→ RDS > Databases > Look for any DB with “Publicly accessible = Yes” → Fix immediately
- [ ] **Review security group rules**

	→ EC2 > Security Groups > Find your RDS SG > Ensure no 0.0.0.0/0 on DB port
- [ ] **Verify no manual snapshots are public**

	→ RDS > Snapshots > Check “Shared” status → Never share with “All AWS accounts”
