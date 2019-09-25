## XACML Policy Template

```
<Policy PolicyId="SamplePolicy"
        RuleCombiningAlgId="urn:oasis:names:tc:xacml:1.0:rule-combining-algorithm:permit-overrides">

    <Description>Sample XACML Authorization Policy.</Description>

    <Target>...</Target>

    <Rule RuleId="LoginRule" Effect="Permit">
        <Description>Rule description.</Description>

        <Target>
            <Subjects>...</Subjects>
            <Resources>...</Resources>
            <Actions>...</Actions>
        </Target>

        <Condition>...</Condition>
    </Rule>
</Policy>
```

**PolicyId:** (unique)，声明策略ID  
**RuleCombiningAlgId:** 声明了规则联合算法，规则联合算法的作用是解决安全策略中不同安全规则可能造成的冲突，以保证每个访问请求只得到一个最终授权结果。Ref: <https://docs.wso2.com/display/IS450/Writing+XACML+3+Policies+in+WSO2+Identity+Server+-+1>  
**Description:** (Optional)，提供策略的文字说明  
**Target:** (Policy Target) describes the decision requests to which this policy applies. If the attributes in a decision request do not match the values specified in the policy target, then the remainder of the policy does not need to be evaluated. This target section is useful for creating an index to a set of policies.  
**Rule:** introduces the rule in this policy, includes `RuleId` & `Effect`, `Effect` can be either `Permit` or `Deny`.  
**Target:** (Rule Target) introduces the target of the rule. As described above for the target of a policy, the target of a rule describes the decision requests to which this rule applies.  If the attributes in a decision request do not match the values specified in the rule target, then the remainder of the rule does not need to be evaluated, and a value of “NotApplicable” is returned to the rule evaluation.  
**Condition:** The condition must evaluate to "True" for the rule to be applicable.


```
<Policy PolicyId="SamplePolicy"
        RuleCombiningAlgId="urn:oasis:names:tc:xacml:1.0:rule-combining-algorithm:permit-overrides">
    
    <Description>This Policy only applies to requests on the SampleServer</Description>

    <Target>
      <Subjects>
        <AnySubject/>
      </Subjects>
      
      <Resources>
        <ResourceMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
          <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">SampleServer</AttributeValue>
          <ResourceAttributeDesignator DataType="http://www.w3.org/2001/XMLSchema#string"
                                       AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"/>
        </ResourceMatch>
      </Resources>
      
      <Actions>
        <AnyAction/>
      </Actions>
    </Target>

    <!-- Rule to see if we should allow the Subject to login -->
    
    <Rule RuleId="LoginRule" Effect="Permit">

      <Description>Only use this Rule if the action is login</Description>

      <Target>
        <Subjects>
          <AnySubject/>
        </Subjects>

        <Resources>
          <AnyResource/>
        </Resources>

        <Actions>
          <ActionMatch MatchId="urn:oasis:names:tc:xacml:1.0:function:string-equal">
            <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#string">login</AttributeValue>
            <ActionAttributeDesignator DataType="http://www.w3.org/2001/XMLSchema#string"
                                       AttributeId="ServerAction"/>
          </ActionMatch>
        </Actions>
      </Target>

      <!-- Only allow logins from 9am to 5pm -->
      <Condition FunctionId="urn:oasis:names:tc:xacml:1.0:function:and">
        <Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:time-greater-than-or-equal"
          <Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:time-one-and-only">
            <EnvironmentAttributeSelector DataType="http://www.w3.org/2001/XMLSchema#time"
                                          AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-time"/>
          </Apply>
          <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#time">09:00:00</AttributeValue>
        </Apply>
        <Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:time-less-than-or-equal"
          <Apply FunctionId="urn:oasis:names:tc:xacml:1.0:function:time-one-and-only">
            <EnvironmentAttributeSelector DataType="http://www.w3.org/2001/XMLSchema#time"
                                          AttributeId="urn:oasis:names:tc:xacml:1.0:environment:current-time"/>
          </Apply>
          <AttributeValue DataType="http://www.w3.org/2001/XMLSchema#time">17:00:00</AttributeValue>
        </Apply>
      </Condition>

    </Rule>

    <!-- We could include other Rules for different actions here -->

    <!-- A final, "fall-through" Rule that always Denies -->
    <Rule RuleId="FinalRule" Effect="Deny"/>

  </Policy>
```