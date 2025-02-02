This section corresponds to transaction ITI-YY Resource Subscription Search of the IHE Technical Framework. Transaction [ITI-YY] Resource Subscription Search is used by the the Resource Notification Subscriber and Resource Notification Broker actors. The Resource Subscription Search [ITI-YY] transaction is used to query Subscription or SubscriptionTopic and get back the results.

### Scope

The Resource Subscription Search [ITI-YY] transaction passes a Resource SubscriptionTopic Search from a Resource Notification Subscriber to a Resource Notification Broker.

### Actors Roles

**Table: Actor Roles**

|Actor | Role |
|-------------------+--------------------------|
| [Resource Notification Subscriber](volume-1.html#subscriber)     | Sends the query request to the Resource Notification Broker |
| [Resource Notification Broker](volume-1.html#broker) | Receives the query and responds |

### Referenced Standards

**FHIR-R4B** [HL7 FHIR Release 4.3.0](https://www.hl7.org/FHIR/R4B)

### Interactions

<figure>
{%include ITI-YY-SubscriptionTopic-seq.svg%}
<figcaption><b>Figure 2:3.YY: ITI-YY interactions</b></figcaption>
</figure>
<br clear="all">

#### Resource SubscriptionTopic Search Request Message
The Resource Notification Subscriber Request Message is a parametrized HTTP GET that allows to search for a list of SubscriptionTopic resources managed by the Resource Notification Broker, based on a set of search parameters.

##### Trigger Events

A Resource Notification Subscriber sends this message to the Resource Notification Broker when it needs to discover SubscriptionTopic resource.  

##### Message Semantics

The Resource Notification Subscriber sends a HTTP GET request to the Resource Notification Broker. This request shall comply with requirements specified in the HL7<sup>®</sup>
FHIR<sup>®</sup> standard <http://hl7.org/fhir/R4/http.html#search>.

The search target URL follows the FHIR HTTP specification <http://hl7.org/fhir/R4B/http.html>, addressing the resource type that the Resource Notification Subscriber is interested to discorver. 

The Resource Notification Subscriber Request Message can be expressed by addressing the  SubscriptionTopic Resource:

> GET \[base\]/SubscriptionTopic?\[Parameters\]

The Resource Notification Subscriber may provide the optional parametr "_format" to specify the desiderd MIME-types in the response message. The Resource Notification Broker should accept "application/fhir+xml" and "application/fhir+json" as _format parameter.  For example, indicating application/fhir+json could result in the response from the Resource Notification Broker being a json FHIR Bundle with all the content encoded as FHIR resources.

##### Query Parameters
The query parameter allowed are the parameters indicated in the [Search parameters](http://hl7.org/fhir/R4B/subscriptionTopic.html#search) section.  

<table class="list">
   <tbody>
      <tr>
         <td>
            <b>Name</b>
         </td>
         <td>
            <b>Type</b>
         </td>
         <td>
            <b>Expression</b>
         </td>
         <td>
            <b>Description</b>
         </td>
      </tr>
      <tr>
         <td>date</td>
         <td>
            <a>date</a>
         </td>
         <td>SubscriptionTopic.date</td>
         <td>Date status first applied</td>
      </tr>
      <tr>
         <td>derived-or-self</td>
         <td>
            <a>uri</a>
         </td>
         <td>SubscriptionTopic.url | SubscriptionTopic.derivedFrom</td>
         <td>A server defined search that matches either the url or derivedFrom</td>
      </tr>
      <tr>
         <td>identifier</td>
         <td>
            <a>token</a>
         </td>
         <td>SubscriptionTopic.identifier</td>
         <td>Business Identifier for SubscriptionTopic</td>
      </tr>
      <tr>
         <td>resource</td>
         <td>
            <a>uri</a>
         </td>
         <td>SubscriptionTopic.resourceTrigger.resource</td>
         <td>Allowed Data type or Resource (reference to definition) for this definition,
            searches resourceTrigger, eventTrigger, and notificationShape for matches.
         </td>
      </tr>
      <tr>
         <td>status</td>
         <td>
            <a>token</a>
         </td>
         <td>SubscriptionTopic.status</td>
         <td>draft | active | retired | unknown</td>
      </tr>
      <tr>
         <td>title</td>
         <td>
            <a>string</a>
         </td>
         <td>SubscriptionTopic.title</td>
         <td>Name for this SubscriptionTopic (Human friendly)</td>
      </tr>
      <tr>
         <td>trigger-description</td>
         <td>
            <a>string</a>
         </td>
         <td>SubscriptionTopic.resourceTrigger.description</td>
         <td>Text representation of the trigger</td>
      </tr>
      <tr>
         <td>url</td>
         <td>
            <a>uri</a>
         </td>
         <td>SubscriptionTopic.url</td>
         <td>Logical canonical URL to reference this SubscriptionTopic (globally unique)</td>
      </tr>
      <tr>
         <td>version</td>
         <td>
            <a>token</a>
         </td>
         <td>SubscriptionTopic.version</td>
         <td>Business version of the SubscriptionTopic</td>
      </tr>
   </tbody>

##### Expected Actions

The Resource Notification Broker received the message shall process the request and respond with the results of the search.

#### Resource SubscriptionTopic Search Response Message
The Resource Notification Broker returns a HTTP status code appropiate to the processing as well a Bundle containing a list of the matching SubscriptionTopic resources.

##### Trigger Events

The Resource Notification Broker completed processing of the request message.

##### Message Semantics

Based on the query results, the Resource Notification Broker will either return an error or success. Guidance on handling Access Denied related to use of 200, 403 and 404 can be found in ITI TF-2x: [Appendix Z.7](https://profiles.ihe.net/ITI/TF/Volume2/ch-Z.html#z.7-guidance-on-access-denied-results).

When the the Resource Notification Broker needs to report an error, it shall use HTTP error response codes and should include a FHIR OperationOutcome with more details on the failure. See FHIR https://hl7.org/fhir/R4/http.html and https://hl7.org/fhir/R4B/operationoutcome.html.

If the Resource SubscriptionTopic Search message is processed successfully, whether or not any SubscriptionTopic resources are found, the HTTP status code shall be 200. The Resource SubscriptionTopic Search Response message shall be a Bundle Resource containing zero or more SubscriptionTopic Resources. If the Document Responder is sending warnings, the Bundle Resource shall also contain an OperationOutcome Resource that contains those warnings.

The response shall adhere to the FHIR Bundle constraints specified in ITI TF-2x: [Appendix Z.7](https://profiles.ihe.net/ITI/TF/Volume2/ch-Z.html#z.1-resource-bundles)

##### Expected Actions

The Resource Notification Subscriber shall process the results according to application-defined rules.


### CapabilityStatement Resource

Server implementing this transaction shall provide a CapabilityStatement Resource as described in ITI TF-2x: Appendix Z.3 indicating the transaction has been implemented. 
* Requirements CapabilityStatement for [Client](CapabilityStatement-IHE.ToDo.client.html)
* Requirements CapabilityStatement for [Server](CapabilityStatement-IHE.ToDo.server.html)

### Security Considerations

See [MHD Security Considerations](volume-1.html#security-considerations)

#### Security Audit Considerations

''TODO: The security audit criteria ''

##### Client Audit 

''TODO: the specifics''

##### Server Audit 

''TODO: the specifics''
