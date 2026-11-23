# **CCNA Revision Notes – Terraform**

## **1. Infrastructure as Code (IaC)**

- **IaC:** Provisioning and managing infrastructure using machine-readable configuration files instead of manual CLI/GUI configuration.
- Benefits: consistency, scalability, repeatability, and automation.
- Examples: Ansible, Terraform, Puppet, Chef.

## **2. Provisioning vs Configuration Management**

| Configuration Management                  | Infrastructure Provisioning              |
| ----------------------------------------- | ---------------------------------------- |
| Manages existing infrastructure           | Creates, modifies, and deletes resources |
| Installs software and configures settings | Deploys VMs, networks, storage, etc.     |
| Ansible, Puppet, Chef                     | Terraform                                |

- Terraform and Ansible are often used together:
  - **Terraform:** Creates infrastructure.
  - **Ansible:** Configures and maintains it.
- Both tools have some overlap.

## **3. Mutable vs Immutable Infrastructure**

- **Mutable:** Existing infrastructure is modified in place (updates, patches, configuration changes).
  - Typically used by Ansible.
- **Immutable:** Existing resources are replaced with new updated resources rather than modified in place.
  - Typically used by Terraform.
  - Helps prevent configuration drift.

## **4. Procedural vs Declarative**

- **Procedural (Imperative):** Defines the exact steps and order required to achieve the result.
  - Ansible and Chef.
- **Declarative:** Defines the desired final state; the tool determines the steps needed.
  - Terraform and Puppet.

## **5. Terraform Basics**

- Open-source IaC tool developed by **HashiCorp**.
- Primarily an infrastructure provisioning tool.
- Supports providers such as AWS, Azure, GCP, Kubernetes, and Cisco platforms.
- Uses provider APIs to deploy infrastructure.
- Uses a **push model** and is **agentless** (no software agent required on managed infrastructure).
- Uses a declarative approach and typically immutable infrastructure.

### **Terraform components**

| Component           | Purpose                                           |
| ------------------- | ------------------------------------------------- |
| Terraform Core      | Main Terraform software; processes configurations |
| Configuration files | Define desired infrastructure state               |
| State file          | Tracks current deployed infrastructure state      |
| Providers           | Platforms Terraform deploys resources to          |

- Terraform Core is written in **Go**.
- Configuration files use **HCL (HashiCorp Configuration Language)**.
- HCL is a domain-specific language (DSL).

### **Terraform workflow**

1. **Write:** Define desired infrastructure in configuration files.
2. **Plan:** Preview the changes Terraform intends to make.
3. **Apply:** Execute the plan and deploy the changes.
4. **Destroy:** Delete resources when no longer needed (additional step).

## **CCNA Exam Focus**

- IaC = infrastructure managed through code.
- Ansible, Puppet, and Chef = primarily configuration management.
- Terraform = primarily provisioning.
- Ansible = typically mutable and procedural.
- Terraform = typically immutable and declarative.
- Both Ansible and Terraform are agentless and use a push model.
- Terraform uses Go and HCL.
- Memorize: **Write → Plan → Apply**.

**Quiz answers :**

- Terraform characteristics: Declarative + Immutable.
- Terraform workflow: Write, Plan, Apply.
- Configuration management tools: Ansible, Chef, Puppet.