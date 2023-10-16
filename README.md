# im lazy to finish this but i will

```yaml
type: object
properties:
  id:
    description: The customer identifier string
    readOnly: true
    allOf:
      - $ref: ./ResourceId.yaml
  websiteId:
    description: The website's ID
    allOf:
      - $ref: ./ResourceId.yaml
  paymentToken:
    type: string
    writeOnly: true
    description: |
      A write-only payment token; if supplied, it will be converted into a
      payment instrument and be set as the `defaultPaymentInstrument`. The
      value of this property will override the `defaultPaymentInstrument`
      in the case that both are supplied. The token may only be used once
      before it is expired.
  defaultPaymentInstrument:
    $ref: ./PaymentInstrument.yaml
  createdTime:
    description: The customer created time
    allOf:
      - $ref: ./ServerTimestamp.yaml
  updatedTime:
    description: The customer updated time
    allOf:
      - $ref: ./ServerTimestamp.yaml
  tags:
    description: A list of customer's tags
    readOnly: true
    type: array
    items:
      $ref: ./Tags/Tag.yaml
  revision:
    description: >
      The number of times the customer data has been modified.

      The revision is useful when analyzing webhook data to determine if the
      change takes precedence over the current representation.
    type: integer
    readOnly: true
  _links:
    type: array
    description: The links related to resource
    readOnly: true
    minItems: 3
    items:
      anyOf:
        - $ref: ./Links/SelfLink.yaml
        - $ref: ./Links/NotesLink.yaml
        - $ref: ./Links/DefaultPaymentInstrumentLink.yaml
        - $ref: ./Links/LeadSourceLink.yaml
        - $ref: ./Links/WebsiteLink.yaml
  _embedded:
    type: array
    description: >-
      Any embedded objects available that are requested by the `expand`
      querystring parameter.
    readOnly: true
    minItems: 1
    items:
      anyOf:
        - $ref: ./Embeds/LeadSourceEmbed.yaml

```

```yaml
get:
  tags:
    - Customers
  summary: Retrieve a list of customers
  operationId: GetCustomerCollection
  description: |
    You can have a markdown description here.
  parameters:
    - $ref: ../components/parameters/collectionLimit.yaml
    - $ref: ../components/parameters/collectionOffset.yaml
    - $ref: ../components/parameters/collectionFilter.yaml
    - $ref: ../components/parameters/collectionQuery.yaml
    - $ref: ../components/parameters/collectionExpand.yaml
    - $ref: ../components/parameters/collectionFields.yaml
  responses:
    '200':
      description: A list of Customers was retrieved successfully
      headers:
        Rate-Limit-Limit:
          $ref: ../components/headers/Rate-Limit-Limit.yaml
        Rate-Limit-Remaining:
          $ref: ../components/headers/Rate-Limit-Remaining.yaml
        Rate-Limit-Reset:
          $ref: ../components/headers/Rate-Limit-Reset.yaml
        Pagination-Total:
          $ref: ../components/headers/Pagination-Total.yaml
        Pagination-Limit:
          $ref: ../components/headers/Pagination-Limit.yaml
        Pagination-Offset:
          $ref: ../components/headers/Pagination-Offset.yaml
      content:
        application/json:
          schema:
            type: array
            items:
              $ref: ../components/schemas/Customer.yaml
        text/csv:
          schema:
            type: array
            items:
              $ref: ../components/schemas/Customer.yaml
    '401':
      $ref: ../components/responses/AccessForbidden.yaml
  x-code-samples:
    - lang: PHP
      source:
        $ref: ../code_samples/PHP/customers/get.php
post:
  tags:
    - Customers
  summary: Create a customer (without an ID)
  operationId: PostCustomer
  description: Another markdown description here.
  requestBody:
    $ref: ../components/requestBodies/Customer.yaml
  responses:
    '201':
      $ref: ../components/responses/Customer.yaml
    '401':
      $ref: ../components/responses/AccessForbidden.yaml
    '409':
      $ref: ../components/responses/Conflict.yaml
    '422':
      $ref: ../components/responses/InvalidDataError.yaml
  x-code-samples:
    - lang: PHP
      source:
        $ref: ../code_samples/PHP/customers/post.php
```
