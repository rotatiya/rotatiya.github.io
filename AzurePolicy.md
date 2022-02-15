#### What is Azure policy
Azure policy is one of Azure ecosystem service can be used for below reasons for existing or new resources. Generally it is used for resource consistency, regulatory compliance, security, cost, and management. Azure policy defined in JSON format.
- Compliance
- Enforce rules at different azure scopes.
- Meet Standards and SLA.
- Reference: https://docs.microsoft.com/en-us/azure/governance/policy/overview

### Policy Scope
As policy uconfigured to azure environment, it supports azure environment scopes viz: 
- Individual Resource
- Resource Group
- Subscription
- Management Group (formed by combining subscriptions together)
- Azure policy scopes and ARM scopes are the same.
- Reference https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview#understand-scope

![image](https://user-images.githubusercontent.com/38330170/154115439-56e93f18-f686-4651-82e7-4ca52658c023.png)

### Azure Policy vs RBAC
Azure policy dont restrict actions or operations. policy just evaluate based on definition of policy and scope defined for new or existing resource. However the RAC controls user actions or operations based on role assigned to it.
Azure policy can block resource operation either create or update based on policy definition but allow that operation to perform and evaluate where as RBAC controls if that action is permissible to that user or not.
In short RBAC is user centric whereas policy is resource centric. Meaning first RBAC comes and then if the action is permitted then only Policy comes in picture.

### RBAC action for Azure policy:
- Microsoft.Azuthorization
- Microsoft.PolicyInsights
Azure AD(RBAC) has many built-in roles grant permission to azure policy resources.

#### What is policy Initiative
Group or set of policy definition is called as Initiative.

#### Policy Modes
THere are two types of Azure policy modes:
- All : Evaluate everything
- Indexed: Evaluate resource types that supports Tag and location.


#### Policy Types
There are 3 different policy types using which we can enforce compliance or configured policies to azure resources.
- Built in  : Microsoft has define some of policy based on inputs received from industry and based on few compliances and these policies are managed and maintened by Microsoft only. If you need any changes then you can create your custom policy following definition of it.
- Custom  : Policy created or drafted by customer is custom policy
- Static  : Regulatory compliance policy Define and managed by both customer and the Microsoft. ITs kind of shared policy on known regulatory compliance .


#### Policy Definition
Policy is defined using JSON format. The policy definition contains elements for 
- Display name
- Description
- Mode
- Metadata
- Parameters
- Policy Rule
  - logical evaluation
  - Effect
- e.g. of policy rule.
{
  "if": {
    <accessor>, <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
  }

  Reference for Azure policy sample: https://docs.microsoft.com/en-us/azure/governance/policy/samples/
  
