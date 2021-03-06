---

copyright:
  years: 2015, 2020
lastupdated: "2020-05-13"

keywords: billing, data centers, MAU, monthly active users, service plans

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Plan information
{: #services-information}

Billing for the use of {{site.data.keyword.conversationshort}} is managed through your {{site.data.keyword.cloud}} account.
{: shortdesc}

The metrics that are used for billing purposes differ based on your plan type. You can be billed based on the number of API calls made to a service instance or on the number of active users who interact with the instance.

For answers to common questions about subscriptions, see [How you're charged](/docs/billing-usage?topic=billing-usage-charges){: external}.

## Plan details
{: #services-information-plan-details}

Explore the {{site.data.keyword.conversationshort}} [service plan options](https://www.ibm.com/cloud/watson-assistant/pricing/){: external}.

Find out more about what is included with the plans.

- [Artifact limits per plan](#services-information-limits)
- [Plus and Premium plan features](#services-information-premium)

### Artifact limits per plan
{: #services-information-limits}

Information about the artifact limits per plan is available from the topics that describe how to create the artifacts, so you can refer to the limits when you need to know them. Here are links to the topics:

- [Assistants](/docs/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Dialog nodes](/docs/assistant?topic=assistant-dialog-tasks#dialog-tasks-node-limits)
- [Entities](/docs/assistant?topic=assistant-entities#entities-limits)
- [Inactivity timeout](/docs/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [Intents](/docs/assistant?topic=assistant-intents#intents-limits)
- [Integrations](/docs/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Logs](/docs/assistant?topic=assistant-logs#logs-limits)
- [Skills](/docs/assistant?topic=assistant-skill-add#skill-add-limits)
- [Versions](/docs/assistant?topic=assistant-versions#versions-limits)

### Plus and Premium plan features ![Plus or Premium plan only](images/plus.png)
{: #services-information-premium}

The following features are available only to users of Plus or Premium plans.

- [Activity tracker](/docs/assistant?topic=assistant-at-events) (Premium plans only)
- [Private endpoints](/docs/assistant?topic=assistant-security#security-private-endpoints)
- [Intent conflict resolution](/docs/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Intent recommendations and intent user example recommendations](/docs/assistant?topic=assistant-intent-recommendations)
- [Intercom integration](/docs/assistant?topic=assistant-deploy-intercom)
- [Search skill](/docs/assistant?topic=assistant-skill-search-add)
- [Web Chat](/docs/assistant?topic=assistant-deploy-web-chat)

The plan type of the service instance you are currently using is displayed in the page header. You can upgrade from one plan type to another. For more information, see [Upgrading](/docs/assistant?topic=assistant-upgrade).

## User-based plans explained
{: #services-information-user-based-plans}

Unlike API-based plans, which measure usage by the number of API calls made during a month, the Plus and Premium plans measure usage by the number of monthly active users.

For example, you can pay $120 for the Plus plan, which covers from 0 to 1,000 monthly active users per service instance per billing period. A Plus plan service instance supports up to 100 assistants. For more details about what is supported by this plan and other plans, see the [artifact limits](#services-information-limits) information.

A monthly active user (MAU) is any unique user who has at least one meaningful interaction with your assistant or custom application over the calendar billing month. A meaningful interaction is an exchange in which a user sends a request to your service and your service responds. Welcome messages that are displayed at the start of a conversation are not charged.

A unique user is recognized by the user ID that is associated with the person that interacts with your assistant. It is your responsibility to pass the user ID information to the service. {{site.data.keyword.conversationshort}} checks for the following information from API requests in this order for billing purposes:

  1.  **user_id**: A property defined in the API that is sent in the context object of a /message API call. Using this property is the best way to ensure that you accurately attribute /message API calls to unique users. For more information about the user ID property, see the API reference documentation:
  
    - `context.global.system.user_id`: [v2 API](https://cloud.ibm.com/apidocs/assistant/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [v1 API](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-response-to-user-input)

  1.  **session_id (v2 only)**: A property defined in the v2 API that identifies a single conversation between a user and the assistant. A session ID is provided in /message API calls that are generated by the built-in integrations. The session ends when a user closes the chat window or after the inactivity time limit is reached.

      If you use the stateless v2 message API, you must specify the session_id with each message in an ongoing conversation (in `context.global.session_id`).
      {: note}

  1.  **conversation_id (v1 only)**: A property defined in the v1 API that is stored in the context object of a /message API call. This property can be used to identify multiple /message API calls that are associated with a single conversational exchange with one user. However, the same ID is only used if you explicitly retain the ID and pass it back with each request that is made as part of the same conversation. Otherwise, a new ID is generated for each new /message API call.

For example, if the same person chats with your assistant on three separate occasions over the same billing period, how you represent that user in the API call impacts how the interactions are billed. If you identify the user interaction with a `user_id`, it counts as one use. If you identify the user interaction with a `session_id`, then it counts as three uses (because there is a separate session that is created for each interaction).

Design any custom applications to capture a unique `user_id` or `session_id` and pass the information to {{site.data.keyword.conversationshort}}. Choose a non-human-identifiable ID that doesn't change throughout the customer lifecycle.

The built-in integrations use a combination of the `session_id` and `user_id` to identify an interaction. For Web Chat, you can specify a `user_id`. For more information, see [Adding user identity information](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-userid).

Billing is managed per monthly active user per service instance. If a single user interacts with assistants that are hosted by different service instances that belong to the same plan, each interaction is treated as a separate use. You are billed for the user's interaction with each service instance separately.

### Test activity charges
{: #services-information-billing-testing}

When testing your skills and assistants, any messages that you submit in the "Try it out" pane are not charged. However, test messages that you send from the preview link integration are charged. For the preview link, a random `user_id` is generated and stored in a cookie. The multiple interactions that a single tester has with the assistant embedded in the preview link are recognized as coming from a single user and are charged accordingly. If you are doing your own test, running a scripted regression test for example, use a single `user_id` for all of the calls within your regression test. Other uses are flagged as abuse.

### Handling anonymous users
{: #services-information-billing-anonymous}

If your custom application or assistant interacts with users who are anonymous, you can generate a randomized universally unique ID to represent each anonymous user. For more information about UUIDs, see [RFC 4122](https://tools.ietf.org/html/rfc4122.html){: external}.

- For Web Chat, if you do not pass an identifier for the user when the session begins, the Web Chat creates one for you. It creates a first-party cookie with a generated anonymous ID. The cookie remains active for 45 days. If the same user returns to your site later in the month and chats with your assistant again, the Web Chat integration recognizes the user. And you are charged only once when the same anonymous user interacts with your assistant multiple times in a single month.

If an anonymous user logs in and later is identified as being the same person who submitted a request with a known ID, you are charged twice. Each message with a unique user ID is charged as an independent active user. To avoid this situation, you can prompt users to log in before you initiate a chat or you can use the anonymous user ID to represent the user consistently.

### Data centers
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} has a network of global data centers that provide performance benefits to its cloud services. See [{{site.data.keyword.cloud_notm}} global data centers](https://www.ibm.com/cloud/data-centers/){: external} for more details.

{{site.data.keyword.cloud_notm}} changed from managing user access with Cloud Foundry to using token-based Identity and Access Management (IAM) authentication. IAM was rolled out in different locations at different times. You can migrate a service instance to move it from its current Cloud Foundry org and space to a resource group. See [Migrating](/docs/watson?topic=watson-migrate) for more details.

You can create {{site.data.keyword.conversationshort}} service instances that are hosted in the following data center locations:

| Location    | Location code | API location |  IAM adoption date | Notes |
|-------------|---------------|--------------|---------------------|-------------------|-------|
| Dallas      | us-south      | N/A          | 30 October 2018 | N/A |
| Frankfurt   | eu-de         | fra          | 30 October 2018 | N/A |
| Seoul       | kr-seo        | seo          | 12 November 2019 | N/A |
| Sydney      | au-syd        | syd          | 7 May 2018 | Instances created before May 7 were syndicated to Dallas |
| Tokyo       | jp-tok        | tok          | 8 November 2018 | N/A |
| London      | eu-gb         | lon          | 13 December 2018 | Instances that were created in the United Kingdom region before December 13 were syndicated to the US South region |
| Washington DC  | us-east    | wdc          | 14 June 2018 | N/A |
{: caption="Data center locations" caption-side="top"}

For an additional cost, you can request that service instances in a Premium plan account be hosted by different data centers.
{: note}

For more information about the data centers in which other {{site.data.keyword.cloud_notm}} services are hosted, see [Services by region](/docs/resources/services_region?topic=resources-services_region#services_region){: external}.

<!--### Premium plan details
{: services-information-premium-slots}

When a Premium plan is provisioned for a customer, one or more service instances are created in a single premium slot. A premium slot is a collection of Kubernetes deployments of a {{site.data.keyword.conversationshort}} service that offers data isolation and some, but not total, compute isolation. 

The following information is true for premium slots that are used to support {{site.data.keyword.conversationshort}} service instances. 

- A premium slot is always owned by one account. An account can have multiple premium slots.
- A premium plam exists in a single region.
- A premium slot can contain one or more resource groups. However, a resource group can belong to only one premium slot. 
- Premium slots are plan-agnostic. A service instance has a plan. A premium slot can support services intances of varying plan types, provided the plans adhere to premium policies. -->