---
it is a: private cloud where you can launch resources (like EC2 instances) in a virtual network that you define.
---
### Key Terms:
**VPC**: the private network (e.g., 10.0.0.0/16)
**Subnet**: a segment of VPC (e.g., 10.0.1.0/24 = "public", 10.0.2.0/24 = "private")
**Internet Gateway (IGW)**: Lets resources in your VPC access the internet
**Route Table**: Rules that say “where traffic should go” (e.g., “send internet-bound traffic to IGW”)
**Security Group**: Firewall for your EC2 instance (we saw this in EC2!)
### Example:
*You’re launching a web app:*
- Web server → in a public subnet (accessible from internet)
- Database → in a private subnet (no direct internet access only talks to web server)
- Only port 80 (HTTP) is open to the world
- Database is safe, even if the web server gets hacked
→ This is secure architecture, made possible by VPC.
### Steps:
1. sign in to [AWS Console](https://aws.amazon.com/)
2. Click VPC under Services.
3. leave it a s default
___
## security check list 🔒
### set up once ✅
- [ ] **Use separate public and private subnets**
	→ VPC > Your VPC > Subnets > Create at least 1 public + 1 private subnet per AZ
- [ ] **Block public IPs in private subnets**
	→ VPC > Subnets > Select private subnet > Actions > Edit subnet settings → Uncheck “Auto-assign public IPv4”
- [ ] **Set up a secure route table for private subnets**
	→ VPC > Route Tables > Create new > Only route 0.0.0.0/0 to NAT gateway (not internet gateway)
- [ ] **Enable VPC Flow Logs** (to CloudWatch or S3)
	→ VPC > Your VPC > Flow logs > Create flow log → Log all REJECT traffic (or ALL)
- [ ] **Disable DNS hostnames if not needed** (optional but tighter)
	→ VPC > Your VPC > Actions > Edit DNS settings > Turn off “Enable DNS hostnames” unless required
### Every Day 🔄
- [ ] **Check for public subnets with overly open routing**
	→ VPC > Route Tables > Look for public route tables → Ensure 0.0.0.0/0 only points to Internet Gateway (not weird targets)
- [ ] **Verify no EC2/RDS is in public subnet by mistake**
	→ EC2 & RDS consoles > Check instance/subnet placement → Critical resources should be in private subnets
- [ ] S**can for unexpected VPC peering or endpoints**
	→ VPC > Peering Connections / Endpoints > Look for unknown connections → Delete if unused