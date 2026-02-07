---
it is a: virtual computer in the cloud
tags:
  - aws
  - blueprint
---
### Key Terms:
**Instance**: a virtual server
**AMI** or **Amazon Machine Image** : a templater/an operating system
**Key Pair**: secure way to log in (instead of a password)
**Security Group**: a firewall that controls who can access
### Example:
lunch a website
1. launch EC2
2. install a web server
3. Upload the website files
4. share the IP
  ⭐ The Website Is Live!!

### Steps:
1. sign in to [AWS Console](https://aws.amazon.com/)
2. Open EC2 dashboard 
3. Configuration:
	1. Name and tags : *hello*
	2. Application and OS Images (Amazon Machine Image) : *Amazon Linux*
	3. Instance type : *t3.micro*
	4. Key pair : *my-key-name-is-mouse* (it will download a `.pem` file **save it**)
	5. Network settings : **default**
	6. Configure storage : like *8gb* is enough 
	 🚀 LUNCH
  ⭐ **CONNECT TO EC2**
4. in the terminal run `chmod 400 my-key.pem` (`chmod`=premonition `400` = owner read only)
5. run `ssh -i "my-key-name-is-mouse.pem" ec2-user@<YOUR_INSTANCE_PUBLIC_IP>`

(create a snapshot)-----------------------------------------------
1. stop the instance
2. under **Elastic Block Store** go to **Volumes**
3. under **Actions** (after selecting the volume) select **Create snapshot**
4. add a description then **Create snapshot**
  ⭐ and we are done
  ___
## security check list 🔒
### set up once ✅
- [ ] **Lock down SSH/RDP access**
	→ EC2 > Security Groups > Remove 0.0.0.0/0 on ports 22 / 3389 → Allow only your IP or trusted network
- [ ] **Enable EBS encryption by default**
	→ EC2 > Volumes > Account settings > Turn on “Always encrypt new EBS volumes”
- [ ] **Attach IAM roles instead of using keys**
	→ EC2 > Instances > Select each instance > Actions > Security > Attach least-privilege IAM role
- [ ] **Disable public IPs for internal instances**
	→ When launching non-public instances > Configure network > Uncheck “Auto-assign public IP”
- [ ] **Enable Session Manager (optional but recommended)**
	→ Systems Manager > Session Manager > Set up → Then you can remove SSH entirely
### Every Day 🔄
- [ ]  **Check for unexpected running instances**
	→ EC2 > Instances > Look for unknown or test instances → Stop if not needed
- [ ] **Verify no new open security groups**
	→ EC2 > Security Groups > Scan for new rules with 0.0.0.0/0 → Delete or fix immediately
- [ ]  **Review IAM roles on critical instances**
	→ EC2 > Instances > Select important ones > Security tab > Confirm correct IAM role is attached