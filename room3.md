**Room 3 = IAM - AWS**

1\. app is using `AWS Cognito id pool` with unathenticated guest access

2\. given URL's source page helds no extra information

3\. `DevTools`, under Network provide actual `API` calls:

&#x20;  - "https://cognito-identity.us-east-1.amazonaws.com/"

&#x20;    -> `IdentityId: us-east-1:4d571309-b072-cf38-77fa-73290947b7e5`

&#x20;  - "https://dynamodb.us-east-1.amazonaws.com/"

&#x20;    -> `Key: {guest\_id: {S: "guest-rb8jhxpy"}}`

&#x20;    -> `TableName: complimentary-GuestWellnessProfiles`

4\. skipping `get-id` and reusing `IdentityId` above, can now ask for temporal access:



```aws cognito-identity get-credentials-for-identity \\\\
--identity-id "us-east-1:4d571309-b072-cf38-77fa-73290947b7e5" \\\\
--region us-east-1```

6. load all 3 credentials in `PowerShell` (AccessKeyId, SecretKey, SessionToken) as `$env`
7. confirming whether credentials are active: `aws sts get-caller-identity`
8. using `aws dynamodb scan` of table from step 4, app gives access to all guest information -> flag: *THM{fr33\_app\_fr33\_d4t4!}*


`DevTools Network recon -> unauthenticated Cognito credentials -> DynamoDB full-table scan -> flag`

