Gebruik dit inventory-format (één pagina) om Secamo’s realiteit te noteren tijdens interviews:

**IaC**

- Terraform: 
	- repo(s) 
	- module-structuur
	- remote state backend
	- workspaces/tenants 
	- policy-as-code (OPA/Conftest?)
	- release flow
    
- Ansible: 
	- roles/playbooks
	- inventory (static/dynamic)
	- secret injection
	- waar draait het (runner/agent)?
    
**CI/CD**
- Platform: 
	- GitHub/GitLab/Jenkins/CodePipeline
	- runners
	- environments 
	- approvals
	- secrets handling
	- artifact storage
    
- DevSecOps gates: 
	- linting
	- SAST
	- IaC scanning
	- image scanning
	- change approvals
    
Cloud footprint (AWS focus)
    
- AWS Organizations? Aantal accounts + scheiding (shared services / klant / security / logging)?
    
- Netwerk: 
	- VPC patterns
	- connectivity naar on-prem
	- logging/monitoring standaard