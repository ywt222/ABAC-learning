## XACML Attribute Category
Ref: <http://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-cd-1-en.html#_Toc229296624>

In XACML 2.0, attributes were organized into subject, resource, environment, or action categories using XML element tags:

```
<?xml version="1.0" encoding="UTF-8"?>
<Request  xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os  access_control-xacml-2.0-context-schema-os.xsd">
        <Subject>
            <Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                <AttributeValue>Julius Hibbert</AttributeValue>
            </Attribute>
        </Subject>
        <Resource>
            <Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
                  DataType="http://www.w3.org/2001/XMLSchema#anyURI">
                <AttributeValue>http://medico.com/record/patient/BartSimpson</AttributeValue>
            </Attribute>
        </Resource>
        <Action>
            <Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                <AttributeValue>read</AttributeValue>
            </Attribute>
        </Action>
        <Environment/>
</Request>
```

In XACML 3.0, these categories are indicated using XML attributes instead of XML element tags:

```
<?xml version="1.0" encoding="utf-8"?>
<Request xsi:schemaLocation="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17 http://docs.oasis-open.org/xacml/3.0/xacml-core-v3-schema-wd-17.xsd" ReturnPolicyIdList="false" CombinedDecision="false" xmlns="urn:oasis:names:tc:xacml:3.0:core:schema:wd-17" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Attributes Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">
    <Attribute IncludeInResult="false" AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id">
      <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">Julius Hibbert</AttributeValue>
    </Attribute>
  </Attributes>
  <Attributes Category="urn:oasis:names:tc:xacml:3.0:attribute-category:resource">
    <Attribute IncludeInResult="false" AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
      <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#anyURI">http://medico.com/record/patient/BartSimpson</AttributeValue>
    </Attribute>
  </Attributes>
  <Attributes Category="urn:oasis:names:tc:xacml:3.0:attribute-category:action">
    <Attribute IncludeInResult="false" AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id">
      <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">read</AttributeValue>
    </Attribute>
  </Attributes>
  <Attributes Category="urn:oasis:names:tc:xacml:3.0:attribute-category:environment" />
</Request>
```

The `<Subject>` element in XACML 2.0 becomes `<Attributes Category="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">` in XACML 3.0, for example. Ditto for the resource, environment, and action categories.

The following attribute category identifiers MUST be used when an XACML 2.0 or earlier policy or request is translated into XACML 3.0.

Attributes previously placed in the Resource, Action, and Environment sections of a request are placed in an attribute category with the following identifiers respectively. It is RECOMMENDED that they are used to list attributes of resources, actions, and the environment respectively when authoring XACML 3.0 policies or requests.

```
urn:oasis:names:tc:xacml:3.0:attribute-category:resource
urn:oasis:names:tc:xacml:3.0:attribute-category:action
urn:oasis:names:tc:xacml:3.0:attribute-category:environment
```

Attributes previously placed in the Subject section of a request are placed in an attribute category which is identical of the subject category in XACML 2.0, as defined below. It is RECOMMENDED that they are used to list attributes of subjects when authoring XACML 3.0 policies or requests.

This identifier indicates the system entity that initiated the access request. That is, the initial entity in a request chain. If subject category is not specified in XACML 2.0, this is the default translation value.

```
urn:oasis:names:tc:xacml:1.0:subject-category:access-subject
```

This identifier indicates the system entity that will receive the results of the request (used when it is distinct from the access-subject).

```
urn:oasis:names:tc:xacml:1.0:subject-category:recipient-subject
```

This identifier indicates a system entity through which the access request was passed. There may be more than one. No means is provided to specify the order in which they passed the message.

```
urn:oasis:names:tc:xacml:1.0:subject-category:intermediary-subject
```

This identifier indicates a system entity associated with a local or remote codebase that generated the request. Corresponding subject attributes might include the URL from which it was loaded and/or the identity of the code-signer. There may be more than one. No means is provided to specify the order in which they processed the request.

```
urn:oasis:names:tc:xacml:1.0:subject-category:codebase
```

This identifier indicates a system entity associated with the computer that initiated the access request. An example would be an IPsec identity.

```
urn:oasis:names:tc:xacml:1.0:subject-category:requesting-machine
```
