**it is a**: *access control | create user | MFA*

### Key Terms:
**Root account**: the CEO with full power

**IAM**: employees (can be a person or an app that need aws access)

**Groups**: group the employees into devs,market,etc for easier control

**Policies**: a Json document that defines permission

**Role**: a temporary permission (like a manager(he can get promoted or fired))
### Example:
*You’re working with a teammate on a project:*
- You create an IAM user for them.
- Add them to the "Developers" group.
- Attach a policy that lets them:
	- Launch EC2 instances
	- Read/write to a specific S3 bucket
	- But NOT delete databases or change billing
→ Secure, controlled, and safe!
### Steps:
1. sign in to [AWS Console](https://aws.amazon.com/)
2. Open IAM
3. create a group
	1. Name the group: *devs*
	2. Add users to the group
	3. Attach permissions policies
4. create a user
	1. user name: *banana*
	2. set permissions
	3. create
___
## security check list 🔒
### set up once ✅
- [ ] **Enable MFA for your root account**
	
	→ IAM > Dashboard > Activate MFA on your root account → Use an authenticator app or hardware key
- [ ] **Delete or disable root access keys**
	
	→ IAM > Security credentials (under root account) > Delete any active access keys
- [ ] **Create a strong admin group** (not for daily use)
	
	→ IAM > Groups > Create “Admins” > Attach AdministratorAccess → Only add trusted users
- [ ] **Require MFA for sensitive actions**
	
	→ IAM > Policies > Create policy with "Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}} → Attach to admin/privileged users
- [ ] **Set a strong password policy**

	→ IAM > Account settings > Enforce: 12+ chars, numbers, symbols, and 90-day rotation
### Every Day 🔄
- [ ] **Check for new users or unexpected access**

	→ IAM > Users > Look for unfamiliar accounts → Investigate or delete
- [ ] **Review users with active access keys**
	
	→ IAM > Users > Click each user > Security credentials tab > Rotate or delete unused keys
- [ ] **Verify no one is using the root account**

	→ AWS CloudTrail (or IAM Dashboard) > Confirm “root” hasn’t been used today
