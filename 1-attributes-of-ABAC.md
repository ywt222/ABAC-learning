## ABAC (Attribute-based access control)
As known as `policy-based access control(PBAC)` or `claims-based access control(CBAC)`, defines an access control paradigm whereby access rights are granted to users through the use of policies which combine attributes together. The policies can use any type of attributes (user attributes, resource attributes, object, environment attributes etc.).

Attribute values can be set-valued or atomic-valued.  
**Set-valued** attributes contain more than one atomic value. Examples are role and project.  
**Atomic-valued** attributes contain only one atomic value. Examples are clearance and sensitivity.  
Attributes can be compared to static values or to one another, thus enabling relation-based access control.


### Attributes
Attributes can be about anything and anyone. They tend to fall into 4 different categories or functions (as in grammatical function)

- Subject attributes: attributes that describe the user attempting the access e.g. age, clearance, department, role, job title...
- Action attributes: attributes that describe the action being attempted e.g. read, delete, view, approve...
- Object attributes: attributes that describe the object (or resource) being accessed e.g. the object type (medical record, bank account...), the department, the classification or sensitivity, the location...
- Contextual (environment) attributes: attributes that deal with time, location or dynamic aspects of the access control scenario

### Policies
Policies are statements that bring together attributes to express what can happen and is not allowed.  
Policies in ABAC can be granting or denying policies.  
Policies can also be local or global and can be written in a way that they override other policies.  
Examples include:

1. A user can view a document if the document is in the same department as the user
2. A user can edit a document if they are the owner and if the document is in draft mode
3. Deny access before 9am  

With ABAC you can have as many policies as you like that cater to many different scenarios and technologies.