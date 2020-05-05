# Okta Workflows collection

I'm creating a collection of flowpacks that can be used by anyone to import inside of their Okta workflows and use them as preloaded "code"

## Requirments

## List of Workflows

1. Character validation on a new user created by a profile master(example: AD, BambooHR(used for this example), SuccessFactors, ServiceNow etc).

---

- Character validation on a new user created by a profile master(example: AD, BambooHR(used for this example), SuccessFactors, ServiceNow etc).
  - Flow content:
    - EVENT: OKTA > User created.
      - Webhook that will only tigger when a new user is created.
    - ACTIONS:
      - OKTA > Read User card
        - On user creation call "[Okta Read User API](https://developer.okta.com/docs/reference/api/users/#get-user)"
      - If Error Card:
        - Try: Get Assigned user for application
          - The card uses the following Okta API: [Get assigned user  for application](https://developer.okta.com/docs/reference/api/apps/#get-assigned-user-for-application)
          - The reason we need this is because there is no data inside of the GET User API that can provide details on who the Profile master
        - If Error
          - We returned error and Stop the flow and not continue
        - If Else Card:
          - If First Name == "" (blank)
            - Compose Card - Create an email body template
              - Provide information that user has been created
              - Inform that the First Name value is blank
            - Send Email
          - Else (If First Name is not blank)
            - Find Pattern Card
              - We are using this to make sure that no special characters are used
            - Assign If Card
              - Card is used to have a true or false condition
                - True - Valid characters
                - False - Invalid characters
            - Else IF Card
              - If we have Valid characters
                - Send email that user is created and mastered by X app
              - If we have Invalid characters
                - Send email and inform contact that the user has been created but has invalid characters and it needs to be resolved

Test

## Disclaimer

**THIS CODE IS PROVIDED *AS IS* WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING ANY IMPLIED WARRANTIES OF FITNESS FOR A PARTICULAR PURPOSE, MERCHANTABILITY, OR NON-INFRINGEMENT.**
