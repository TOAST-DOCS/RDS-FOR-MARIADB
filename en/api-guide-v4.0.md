## Database > RDS for MariaDB > API v4.0 Guide

## RDS for MariaDB API Common Information

### API Endpoint

| Region | Endpoint |
|------|----------|
| Korea (Pangyo) region | https://kr1-rds-mariadb.api.nhncloudservice.com |


### Authentication and Authorization

RDS for MariaDB uses User Access Key tokens for authentication/authorization when calling APIs. User Access Key tokens are Bearer-type temporary access tokens issued based on User Access Keys. For more information about issuing and using User Access Key tokens, see [User Access Key token](/nhncloud/en/public-api/user-access-key-token).
The issued token must be included in the request header along with the Appkey.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| X-TC-APP-KEY | Header | String | O | Appkey of RDS for MariaDB service or project integrated Appkey |
| X-NHN-AUTHORIZATION | Header | String | O | Bearer type token issued by Public API |

Additionally, the APIs that can be called are limited according to project permissions. The `RDS for MariaDB ADMIN` and `RDS for MariaDB VIEWER` roles have default permissions as shown below, and you can grant only the necessary permissions in the role group management menu within the project.

* The `RDS for MariaDB ADMIN` role is granted all permissions required for API execution.
* The `RDS for MariaDB VIEWER` role is granted only permissions to view information.
    * You cannot create, modify, or delete DB instances, or use any features that target DB instances.
    * However, you can use features related to notification groups and user groups.

When API requests fail authentication or lack permissions, the following errors occur:

| resultCode | resultMessage | Description |
|------------|---------------|-----|
| 80401 | Unauthorized | Authentication failed. |
| 80403 | Forbidden | No permission. |

### Common Response Information

All API requests respond with '200 OK'. For detailed response results, refer to the header in the response body.

#### Response Body
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

#### Fields
| Name | Format | Description |
|-----|-----|-----|
| resultCode | Number | Result code<br/>- Success: `0`<br/>- Failure: Non-zero value |
| resultMessage | String | Result message |
| isSuccessful | Boolean | Success status |

### DB Engine Type

| DB Engine Type | Creation Available | Restoration from OBS Available | Authentication Plugin Support |
|------------|----------|------------------|------------|
| MARIADB_V10330 | X | X | NATIVE, ED25519 |
| MARIADB_V10611 | X | X | NATIVE, ED25519 |
| MARIADB_V10612 | X | X | NATIVE, ED25519 |
| MARIADB_V10616 | X | X | NATIVE, ED25519 |
| MARIADB_V10622 | X | X | NATIVE, ED25519 |
| MARIADB_V10625 | X | X | NATIVE, ED25519 |
| MARIADB_V101107 | O | O | NATIVE, ED25519 |
| MARIADB_V101108 | O | O | NATIVE, ED25519 |
| MARIADB_V101113 | O | O | NATIVE, ED25519 |
| MARIADB_V101116 | O | O | NATIVE, ED25519 |
| MARIADB_V11407 | O | O | NATIVE, ED25519 |
| MARIADB_V11410 | O | O | NATIVE, ED25519 |
| MARIADB_V11806 | O | O | NATIVE, ED25519 |

* These values can be used for the dbVersion field of ENUM type.
* Creation or restoration may not be possible depending on the version.

## Project Information

### View Project Member List

```http
GET /v4.0/project/members
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Project.Get | View project member list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| members | Body | Array | Project member list |
| members.memberId | Body | UUID | Project member identifier |
| members.memberName | Body | String | Project member name |
| members.emailAddress | Body | String | Project member email address |
| members.phoneNumber | Body | String | Project member phone number |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "members": [
        {
            "memberId": "550e8400-e29b-41d4-a716-446655440000",
            "memberName": "memberName-example",
            "emailAddress": "user@example.com",
            "phoneNumber": "010-1234-5678"
        }
    ]
}
```

</p>
</details>

---

### View Region List

```http
GET /v4.0/project/regions
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Project.Get | View region list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| regions | Body | Array | Region list |
| regions.regionCode | Body | Enum | Region code<br/>- KR1: `Korea (Pangyo)` |
| regions.isEnabled | Body | Boolean | Region enablement status |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "regions": [
        {
            "regionCode": "KR1",
            "isEnabled": false
        }
    ]
}
```

</p>
</details>

---

## DB Instance Specifications

### View DB Instance Specifications

```http
GET /v4.0/db-flavors
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbFlavor.List | View the list of DB instance specifications |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbFlavors | Body | Array | List of DB instance specifications |
| dbFlavors.dbFlavorId | Body | UUID | Identifier of the DB instance specification |
| dbFlavors.dbFlavorName | Body | String | DB instance specification name |
| dbFlavors.ram | Body | Number | Memory capacity (MB) |
| dbFlavors.vcpus | Body | Number | Number of CPU cores |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbFlavors": [
        {
            "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
            "dbFlavorName": "dbFlavorName-example",
            "ram": 1,
            "vcpus": 1
        }
    ]
}
```

</p>
</details>

---

## Network

### View Subnet List

```http
GET /v4.0/network/subnets
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Network.List | View subnet list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| subnets | Body | Array | List of subnets |
| subnets.subnetId | Body | UUID | Identifier of the subnet |
| subnets.subnetName | Body | String | Name that can identify the subnet |
| subnets.subnetCidr | Body | String | CIDR of the subnet |
| subnets.usingGateway | Body | Boolean | Whether gateway is used |
| subnets.availableIpCount | Body | Number | Number of available IPs |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "subnets": [
        {
            "subnetId": "550e8400-e29b-41d4-a716-446655440000",
            "subnetName": "subnetName-example",
            "subnetCidr": "192.168.0.0/24",
            "usingGateway": false,
            "availableIpCount": 1
        }
    ]
}
```

</p>
</details>

---

## DB Engine

### View DB Engine List

```http
GET /v4.0/db-versions
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbVersion.List | View DB engine list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbVersions | Body | Array | DB engine list |
| dbVersions.dbVersion | Body | String | DB engine type |
| dbVersions.dbVersionName | Body | String | DB engine name |
| dbVersions.restorableFromObs | Body | Boolean | Indicates whether restoration from Object Storage is possible |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbVersions": [
        {
            "dbVersion": "MYSQL_V8036",
            "dbVersionName": "dbVersionName-example",
            "restorableFromObs": false
        }
    ]
}
```

</p>
</details>

---

## Data Storage

### View Storage Type List

```http
GET /v4.0/storage-types
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Storage.List | View storage type list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| storageTypes | Body | Array | Storage type list |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "storageTypes": [
        "General SSD",
        "General HDD"
    ]
}
```

</p>
</details>

---

## Job Information

### Job Status

| Status Name        | Description                           |
|--------------------|---------------------------------------|
| `PREPARING`        | The job is being prepared             |
| `READY`            | The job is ready                      |
| `RUNNING`          | The job is running                    |
| `COMPLETED`        | The job is completed                  |
| `REGISTERED`       | The job is registered                 |
| `WAIT_TO_REGISTER` | The job is waiting to be registered   |
| `INTERRUPTED`      | The job was interrupted while running |
| `CANCELED`         | The job is canceled                   |
| `FAILED`           | The job failed                        |
| `ERROR`            | An error occurred while running the job |
| `DELETED`          | The job is deleted                    |
| `FAIL_TO_READY`    | Failed to prepare the job             |

### View Job Details

```http
GET /v4.0/jobs/{jobId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Job.Get | View job details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| jobId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Job identifier |
| jobStatus | Body | Enum | Current status of the job<br/>- DELETED<br/>- CANNOT_PROGRESS<br/>- FAILED<br/>- ERROR<br/>- CANCELED<br/>- INTERRUPTED<br/>- COMPLETED<br/>- RUNNING<br/>- PREPARING<br/>- READY<br/>- CREATED<br/>- FAIL_TO_READY<br/>- REGISTERED<br/>- FAIL_TO_REGISTER<br/>- WAIT_TO_REGISTER |
| resourceRelations | Body | Array | List of related resources |
| resourceRelations.resourceType | Body | String | Related resource type |
| resourceRelations.resourceId | Body | String | Related resource identifier |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000",
    "jobStatus": "DELETED",
    "resourceRelations": [
        {
            "resourceType": "resourceType-example",
            "resourceId": "resourceId-example"
        }
    ],
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

## DB Instance Groups

### View DB Instance Group List

```http
GET /v4.0/db-instance-groups
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceGroup.List | View DB instance group list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbInstanceGroups | Body | Array | DB instance group list |
| dbInstanceGroups.dbInstanceGroupId | Body | UUID | DB instance group identifier |
| dbInstanceGroups.replicationType | Body | Enum | DB instance group replication type<br/>- STANDALONE: `High availability disabled`<br/>- HIGH_AVAILABILITY: `High availability enabled` |
| dbInstanceGroups.createdYmdt | Body | DateTime | Creation date and time |
| dbInstanceGroups.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstanceGroups": [
        {
            "dbInstanceGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "replicationType": "STANDALONE",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### View DB Instance Group Details

```http
GET /v4.0/db-instance-groups/{dbInstanceGroupId}
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceGroup.Get | View DB instance group details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceGroupId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbInstanceGroupId | Body | UUID | DB instance group identifier |
| replicationType | Body | Enum | DB instance group replication type<br/>- STANDALONE: `High availability disabled`<br/>- HIGH_AVAILABILITY: `High availability enabled` |
| dbInstances | Body | Array | List of DB instances in the DB instance group |
| dbInstances.dbInstanceId | Body | UUID | DB instance identifier |
| dbInstances.dbInstanceType | Body | Enum | DB instance role type<br/>- MASTER: `Master`<br/>- FAILED_MASTER: `Failed master`<br/>- CANDIDATE_MASTER: `Candidate master`<br/>- READ_ONLY_SLAVE: `Read replica` |
| dbInstances.dbInstanceStatus | Body | Enum | Current status of the DB instance<br/>- BEFORE_CREATE: `Before creation (gray)`<br/>- AVAILABLE: `Available (green)`<br/>- STORAGE_FULL: `Storage full (red)`<br/>- FAIL_TO_CREATE: `Failed to create (red)`<br/>- FAIL_TO_CONNECT: `Failed to connect (red)`<br/>- REPLICATION_STOP: `Replication down (red)`<br/>- REPLICATION_DELAY: `Replication delay (yellow)`<br/>- FAILOVER: `Failover completed (red)`<br/>- SHUTDOWN: `Stopped (gray)`<br/>- DELETED: `Deleted (gray)` |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstanceGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "replicationType": "STANDALONE",
    "dbInstances": [
        {
            "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
            "dbInstanceType": "MASTER",
            "dbInstanceStatus": "BEFORE_CREATE"
        }
    ],
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

## DB Instances

### DB Instance Status

| Status | Description |
|--------|-------------|
| `AVAILABLE` | DB instance is available |
| `BEFORE_CREATE` | DB instance not yet created |
| `STORAGE_FULL` | DB instance storage is full |
| `FAIL_TO_CREATE` | DB instance creation failed |
| `FAIL_TO_CONNECT` | DB instance connection failed |
| `REPLICATION_STOP` | DB instance replication is down |
| `FAILOVER` | DB instance failover occurred |
| `SHUTDOWN` | DB instance is shut down |
| `DELETED` | DB instance is deleted |

### DB Instance Progress Status

| Status                     | Description           |
|----------------------------|-----------------------|
| `APPLYING_PARAMETER_GROUP` | Applying parameter group |
| `BACKING_UP`               | Backing up            |
| `CANCELING`                | Canceling             |
| `CREATING`                 | Creating              |
| `CREATING_SCHEMA`          | Creating DB schema    |
| `CREATING_USER`            | Creating user         |
| `DELETING`                 | Deleting              |
| `DELETING_SCHEMA`          | Deleting DB schema    |
| `DELETING_USER`            | Deleting user         |
| `EXPORTING_BACKUP`         | Exporting backup      |
| `FAILING_OVER`             | Failing over          |
| `MIGRATING`                | Migrating             |
| `MODIFYING`                | Modifying             |
| `PREPARING`                | Preparing             |
| `PROMOTING`                | Promoting             |
| `REBUILDING`               | Rebuilding            |
| `REPAIRING`                | Repairing             |
| `REPLICATING`              | Replicating           |
| `RESTARTING`               | Restarting            |
| `RESTARTING_FORCIBLY`      | Restarting forcibly   |
| `RESTORING`                | Restoring             |
| `STARTING`                 | Starting              |
| `STOPPING`                 | Stopping              |
| `SYNCING_SCHEMA`           | Syncing DB schema     |
| `SYNCING_USER`             | Syncing user          |
| `UPDATING_USER`            | Updating user         |

### View DB Instance List

```http
GET /v4.0/db-instances
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.List | View DB instance list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbInstances | Body | Array | DB instance list |
| dbInstances.dbInstanceId | Body | UUID | DB instance identifier |
| dbInstances.dbInstanceGroupId | Body | UUID | DB instance group identifier |
| dbInstances.dbInstanceName | Body | String | Name that can identify the DB instance |
| dbInstances.description | Body | String | Additional information about the DB instance |
| dbInstances.dbVersion | Body | Enum | DB engine type |
| dbInstances.dbPort | Body | Number | DB port |
| dbInstances.dbInstanceType | Body | Enum | DB instance role type<br/>- MASTER: `Master`<br/>- FAILED_MASTER: `Failed master`<br/>- CANDIDATE_MASTER: `Candidate master`<br/>- READ_ONLY_SLAVE: `Read replica` |
| dbInstances.dbInstanceStatus | Body | Enum | Current status of the DB instance<br/>- BEFORE_CREATE: `Before creation (gray)`<br/>- AVAILABLE: `Available (green)`<br/>- STORAGE_FULL: `Storage full (red)`<br/>- FAIL_TO_CREATE: `Failed to create (red)`<br/>- FAIL_TO_CONNECT: `Failed to connect (red)`<br/>- REPLICATION_STOP: `Replication down (red)`<br/>- REPLICATION_DELAY: `Replication delay (yellow)`<br/>- FAILOVER: `Failover completed (red)`<br/>- SHUTDOWN: `Shutdown (gray)`<br/>- DELETED: `Deleted (gray)` |
| dbInstances.progressStatus | Body | Enum | Current progress status of the DB instance<br/>- NONE<br/>- APPLYING_PARAMETER_GROUP<br/>- BACKING_UP<br/>- CANCELING<br/>- CREATING<br/>- CREATING_SCHEMA<br/>- CREATING_USER<br/>- DELETING<br/>- DELETING_SCHEMA<br/>- DELETING_USER<br/>- EXPORTING_BACKUP<br/>- FAILING_OVER<br/>- MIGRATING<br/>- MODIFYING<br/>- PREPARING<br/>- PROMOTING<br/>- PROMOTING_FORCIBLY<br/>- REBUILDING<br/>- REPAIRING<br/>- REPLICATING<br/>- RESTARTING<br/>- RESTARTING_FORCIBLY<br/>- RESTORING<br/>- STARTING<br/>- STOPPING<br/>- SYNCING_SCHEMA<br/>- SYNCING_USER<br/>- UPDATING_USER<br/>- WAIT_MANUAL_CONTROL |
| dbInstances.createdYmdt | Body | DateTime | Creation date and time |
| dbInstances.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstances": [
        {
            "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
            "dbInstanceGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "dbInstanceName": "dbInstanceName-example",
            "description": "description-example",
            "dbVersion": "MYSQL_V8036",
            "dbPort": 1,
            "dbInstanceType": "MASTER",
            "dbInstanceStatus": "BEFORE_CREATE",
            "progressStatus": "NONE",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create DB Instance

```http
POST /v4.0/db-instances
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Create | Create DB instance |

#### Common Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceName | Body | String | O | Name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB instance<br/>- Maximum length: `100` |
| dbFlavorId | Body | UUID | O | Identifier for DB instance specifications |
| dbVersion | Body | Enum | O | DB engine type |
| dbPort | Body | Number | O | DB port<br/>- Minimum value: 3306, Maximum value: 43306 |
| dbUserName | Body | String | O | DB user account name<br/>- Minimum length: `1`<br/>- Maximum length: `32` |
| dbPassword | Body | String | O | DB user account password<br/>- Minimum length: `4`<br/>- Maximum length: `256` |
| parameterGroupId | Body | UUID | O | Identifier for parameter group |
| dbSecurityGroupIds | Body | Array | X | List of DB security group identifiers |
| userGroupIds | Body | Array | X | List of user group identifiers |
| useHighAvailability | Body | Boolean | X | Whether to use high availability<br/>- Default: `false` |
| pingInterval | Body | Number | X | Ping interval in seconds when using high availability<br/>- Default: `3`<br/>- Minimum value: `1`<br/>- Maximum value: `600` |
| useDefaultNotification | Body | Boolean | X | Whether to use default notifications<br/>- Default: `false` |
| useDeletionProtection | Body | Boolean | X | Whether to enable deletion protection<br/>- Default: `false` |
| useSlowQueryAnalysis | Body | Boolean | X | Whether to analyze slow queries<br/>- Default: `true` |
| authenticationPlugin | Body | Enum | X | Authentication Plugin<br/>- NATIVE: `mysql_native_password authentication`<br/>- ED25519: `ed25519 authentication (MariaDB only)` |
| tlsOption | Body | Enum | X | TLS Option<br/>- Default: `NONE`<br/>- NONE: `TLS not used`<br/>- SSL: `SSL authentication`<br/>- X509: `X509 certificate authentication` |
| network | Body | Object | O | Network information object |
| network.subnetId | Body | UUID | O | Subnet identifier |
| network.usePublicAccess | Body | Boolean | X | Whether external access is allowed<br/>- Default: `false` |
| network.availabilityZone | Body | Enum | O | Availability zone to create the DB instance |
| storage | Body | Object | O | Storage information object |
| storage.storageType | Body | Enum | O | Data storage type |
| storage.storageSize | Body | Number | O | Data storage size (GB)<br/>- Minimum value: `20` |
| storage.storageAutoscale | Body | Object | X | Data storage auto-scaling object |
| storage.storageAutoscale.useStorageAutoscale | Body | Boolean | X | Whether to enable storage auto-scaling<br/>- Default: `false` |
| backup | Body | Object | O | Backup information object |
| backup.backupPeriod | Body | Number | O | Backup retention period (days)<br/>- Minimum value: `0`<br/>- Maximum value: `730` |
| backup.backupRetryCount | Body | Number | X | Number of backup retry attempts<br/>- Minimum value: `0`<br/>- Maximum value: `10` |
| backup.ftwrlWaitTimeout | Body | Number | X | Query delay wait time (seconds)<br/>- Minimum value: `0`<br/>- Maximum value: `21600` |
| backup.replicationRegion | Body | Enum | X | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| backup.useBackupLock | Body | Boolean | X | Whether to use table lock<br/>- Default: `true` |
| backup.backupSchedules | Body | Array | O | List of backup schedules |
| backup.backupSchedules.backupWndBgnTime | Body | Time | O | Backup start time |
| backup.backupSchedules.backupWndDuration | Body | Enum | O | Backup Duration<br/>- HALF_AN_HOUR: `30 minutes`<br/>- ONE_HOUR: `1 hour`<br/>- ONE_HOUR_AND_HALF: `1 hour 30 minutes`<br/>- TWO_HOURS: `2 hours`<br/>- TWO_HOURS_AND_HALF: `2 hours 30 minutes`<br/>- THREE_HOURS: `3 hours` |

#### When Using High Availability

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceCandidateName | Body | String | O | Standby master name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |

#### When Using Storage Auto-scaling

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| storage.storageAutoscale.threshold | Body | Number | O | Auto-scaling condition (%)<br/>- Minimum value: `50`<br/>- Maximum value: `95` |
| storage.storageAutoscale.maxStorageSize | Body | Number | O | Maximum auto-scaling size (GB)<br/>- Maximum value: `4096` |
| storage.storageAutoscale.cooldownTime | Body | Number | O | Auto-scaling cooldown time (minutes)<br/>- Minimum value: `10`<br/>- Maximum value: `1440` |

<details><summary>Example</summary>
<p>

```json
{
    "dbInstanceName": "dbInstanceName",
    "description": "description-example",
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "dbVersion": "MYSQL_V8036",
    "dbPort": 1,
    "dbUserName": "dbUserName",
    "dbPassword": "dbPassword",
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupIds": [],
    "userGroupIds": [],
    "useHighAvailability": false,
    "pingInterval": 3,
    "useDefaultNotification": false,
    "useDeletionProtection": false,
    "useSlowQueryAnalysis": true,
    "authenticationPlugin": "NATIVE",
    "tlsOption": "NONE",
    "network": {
        "subnetId": "550e8400-e29b-41d4-a716-446655440000",
        "usePublicAccess": false,
        "availabilityZone": "kr-pub-a"
    },
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20,
        "storageAutoscale": {
            "useStorageAutoscale": false
        }
    },
    "backup": {
        "backupPeriod": 0,
        "backupRetryCount": 0,
        "ftwrlWaitTimeout": 1800,
        "replicationRegion": "KR1",
        "useBackupLock": true,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "HALF_AN_HOUR"
            }
        ]
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

### Restore DB Instance Using Object Storage

```http
POST /v4.0/db-instances/restore-from-obs
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.RestoreFromObs | Restore DB instance using Object Storage |

#### Common request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceName | Body | String | O | Master name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB instance<br/>- Maximum length: `100` |
| dbFlavorId | Body | UUID | O | DB instance specification identifier |
| dbPort | Body | Number | O | DB port |
| dbVersion | Body | Enum | O | DB engine type |
| useHighAvailability | Body | Boolean | X | Whether to use high availability<br/>- Default: `false` |
| pingInterval | Body | Number | X | Ping interval (seconds) when using high availability<br/>- Minimum value: `1`<br/>- Maximum value: `600` |
| storage | Body | Object | O | Storage information object |
| storage.storageType | Body | Enum | O | Storage type |
| storage.storageSize | Body | Number | O | Data storage size (GB)<br/>- Minimum value: `20` |
| storage.storageAutoscale | Body | Object | X | Data storage auto-expansion object |
| storage.storageAutoscale.useStorageAutoscale | Body | Boolean | X | Whether to use storage auto-expansion<br/>- Default: `false` |
| network | Body | Object | O | Network information object |
| network.subnetId | Body | UUID | O | Subnet identifier |
| network.usePublicAccess | Body | Boolean | X | Whether external access is possible<br/>- Default: `false` |
| network.availabilityZone | Body | Enum | O | Availability zone where the DB instance will be created |
| backup | Body | Object | O | Backup information object |
| backup.backupPeriod | Body | Number | O | Backup retention period (days)<br/>- Minimum value: `0`<br/>- Maximum value: `730` |
| backup.ftwrlWaitTimeout | Body | Number | X | Query delay wait time (seconds)<br/>- Minimum value: `0`<br/>- Maximum value: `21600` |
| backup.backupRetryCount | Body | Number | X | Number of backup retries<br/>- Minimum value: `0`<br/>- Maximum value: `10` |
| backup.replicationRegion | Body | Enum | X | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| backup.useBackupLock | Body | Boolean | X | Whether to use table lock<br/>- Default: `true` |
| backup.backupSchedules | Body | Array | O | Backup schedule list |
| backup.backupSchedules.backupWndBgnTime | Body | Time | O | Backup start time |
| backup.backupSchedules.backupWndDuration | Body | Enum | O | Backup Duration<br/>- HALF_AN_HOUR: `30 minutes`<br/>- ONE_HOUR: `1 hour`<br/>- ONE_HOUR_AND_HALF: `1 hour 30 minutes`<br/>- TWO_HOURS: `2 hours`<br/>- TWO_HOURS_AND_HALF: `2 hours 30 minutes`<br/>- THREE_HOURS: `3 hours` |
| restore | Body | Object | O | Restore information object |
| restore.tenantId | Body | String | O | Tenant ID of Object Storage where the backup is stored |
| restore.username | Body | String | O | NHN Cloud account or IAM member ID |
| restore.password | Body | String | O | API password for Object Storage where the backup is stored |
| restore.targetContainer | Body | String | O | Container in Object Storage where the backup is stored |
| restore.objectPath | Body | String | O | Path of the backup stored in the container |
| useDefaultNotification | Body | Boolean | X | Whether to use default notifications<br/>- Default: `false` |
| useSlowQueryAnalysis | Body | Boolean | X | Whether to analyze slow queries<br/>- Default: `true` |
| parameterGroupId | Body | UUID | O | Parameter group identifier |
| dbSecurityGroupIds | Body | Array | X | DB security group identifier list |
| userGroupIds | Body | Array | X | User group identifier list |
| useDeletionProtection | Body | Boolean | X | Whether to use deletion protection<br/>- Default: `false` |

#### When using high availability

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceCandidateName | Body | String | O | Candidate master name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |

#### When using storage auto-expansion

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| storage.storageAutoscale.threshold | Body | Number | O | Auto-expansion condition (%)<br/>- Minimum value: `50`<br/>- Maximum value: `95` |
| storage.storageAutoscale.maxStorageSize | Body | Number | O | Auto-expansion maximum size (GB)<br/>- Maximum value: `4096` |
| storage.storageAutoscale.cooldownTime | Body | Number | O | Auto-expansion cooldown time (minutes)<br/>- Minimum value: `10`<br/>- Maximum value: `1440` |

<details><summary>Example</summary>
<p>

```json
{
    "dbInstanceName": "dbInstanceName",
    "description": "description-example",
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "dbPort": 1,
    "dbVersion": "MYSQL_V8036",
    "useHighAvailability": false,
    "pingInterval": 3,
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20,
        "storageAutoscale": {
            "useStorageAutoscale": false
        }
    },
    "network": {
        "subnetId": "550e8400-e29b-41d4-a716-446655440000",
        "usePublicAccess": false,
        "availabilityZone": "kr-pub-a"
    },
    "backup": {
        "backupPeriod": 0,
        "ftwrlWaitTimeout": 1800,
        "backupRetryCount": 0,
        "replicationRegion": "KR1",
        "useBackupLock": true,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "HALF_AN_HOUR"
            }
        ]
    },
    "restore": {
        "tenantId": "0123456789abcdef0123456789abcdef",
        "username": "username-example",
        "password": "password-example",
        "targetContainer": "targetContainer-example",
        "objectPath": "objectPath-example"
    },
    "useDefaultNotification": false,
    "useSlowQueryAnalysis": true,
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupIds": [],
    "userGroupIds": [],
    "useDeletionProtection": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete DB Instance

```http
DELETE /v4.0/db-instances/{dbInstanceId}
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Delete | Delete DB instance |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | The identifier of the DB instance |
| deleteAutoBackup | Body | Boolean | X | Whether to delete auto backup<br/>- Default: `false` |

<details><summary>Example</summary>
<p>

```json
{
    "deleteAutoBackup": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | The identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View DB Instance Details

```http
GET /v4.0/db-instances/{dbInstanceId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | View DB instance details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbInstanceId | Body | UUID | DB instance identifier |
| dbInstanceGroupId | Body | UUID | DB instance group identifier |
| dbInstanceName | Body | String | Name that identifies the DB instance |
| description | Body | String | Additional information about the DB instance |
| dbVersion | Body | Enum | DB engine type |
| dbPort | Body | Number | DB port |
| dbInstanceType | Body | Enum | DB instance role type<br/>- MASTER: `Master`<br/>- FAILED_MASTER: `Failed master`<br/>- CANDIDATE_MASTER: `Candidate master`<br/>- READ_ONLY_SLAVE: `Read replica` |
| dbInstanceStatus | Body | Enum | Current status of the DB instance<br/>- BEFORE_CREATE: `Before creation (gray)`<br/>- AVAILABLE: `Available (green)`<br/>- STORAGE_FULL: `Storage full (red)`<br/>- FAIL_TO_CREATE: `Failed to create (red)`<br/>- FAIL_TO_CONNECT: `Failed to connect (red)`<br/>- REPLICATION_STOP: `Replication down (red)`<br/>- REPLICATION_DELAY: `Replication delay (yellow)`<br/>- FAILOVER: `Failover completed (red)`<br/>- SHUTDOWN: `Stopped (gray)`<br/>- DELETED: `Deleted (gray)` |
| progressStatus | Body | Enum | Current progress status of the DB instance<br/>- NONE<br/>- APPLYING_PARAMETER_GROUP<br/>- BACKING_UP<br/>- CANCELING<br/>- CREATING<br/>- CREATING_SCHEMA<br/>- CREATING_USER<br/>- DELETING<br/>- DELETING_SCHEMA<br/>- DELETING_USER<br/>- EXPORTING_BACKUP<br/>- FAILING_OVER<br/>- MIGRATING<br/>- MODIFYING<br/>- PREPARING<br/>- PROMOTING<br/>- PROMOTING_FORCIBLY<br/>- REBUILDING<br/>- REPAIRING<br/>- REPLICATING<br/>- RESTARTING<br/>- RESTARTING_FORCIBLY<br/>- RESTORING<br/>- STARTING<br/>- STOPPING<br/>- SYNCING_SCHEMA<br/>- SYNCING_USER<br/>- UPDATING_USER<br/>- WAIT_MANUAL_CONTROL |
| dbFlavorId | Body | UUID | DB instance specification identifier |
| parameterGroupId | Body | UUID | Identifier of the parameter group applied to the DB instance |
| dbSecurityGroupIds | Body | Array | List of DB security group identifiers applied to the DB instance |
| notificationGroupIds | Body | Array | List of notification group identifiers applied to the DB instance |
| useDeletionProtection | Body | Boolean | Whether DB instance deletion protection is enabled |
| useSlowQueryAnalysis | Body | Boolean | Whether slow query analysis is enabled |
| supportAuthenticationPlugin | Body | Boolean | Whether authentication plugin is supported |
| needToApplyParameterGroup | Body | Boolean | Whether the latest parameter group needs to be applied |
| needMigration | Body | Boolean | Whether migration is required |
| supportDbVersionUpgrade | Body | Boolean | Whether DB version upgrade is supported |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
    "dbInstanceGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbInstanceName": "dbInstanceName-example",
    "description": "description-example",
    "dbVersion": "MYSQL_V8036",
    "dbPort": 1,
    "dbInstanceType": "MASTER",
    "dbInstanceStatus": "BEFORE_CREATE",
    "progressStatus": "NONE",
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupIds": [
        "550e8400-e29b-41d4-a716-446655440000"
    ],
    "notificationGroupIds": [
        "550e8400-e29b-41d4-a716-446655440000"
    ],
    "useDeletionProtection": false,
    "useSlowQueryAnalysis": false,
    "supportAuthenticationPlugin": false,
    "needToApplyParameterGroup": false,
    "needMigration": false,
    "supportDbVersionUpgrade": false,
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

### Modify DB Instance

```http
PUT /v4.0/db-instances/{dbInstanceId}
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Modify | Modify DB Instance |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier for the DB instance |
| dbInstanceName | Body | String | X | Name that can identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| dbInstanceCandidateName | Body | String | X | Candidate master name that can identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB instance<br/>- Maximum length: `100` |
| dbPort | Body | Number | X | DB port<br/>- Minimum: 3306, Maximum: 43306 |
| dbFlavorId | Body | UUID | X | Identifier for the DB instance specification |
| parameterGroupId | Body | UUID | X | Identifier for the parameter group |
| dbVersion | Body | Enum | X | DB engine version code |
| useSlowQueryAnalysis | Body | Boolean | X | Whether to analyze slow queries |
| useDummy | Body | Boolean | X | Whether to use dummy when upgrading the DB version of a single DB instance<br/>- Default: `false` |
| dbSecurityGroupIds | Body | Array | X | List of DB security group identifiers |
| executeBackup | Body | Boolean | X | Whether to proceed with current point backup<br/>- Default: `false` |
| useOnlineFailover | Body | Boolean | X | Whether to restart using failover<br/>- Default: `false` |
| waitReplicationDelay | Body | Boolean | X | Wait for replication delay resolution<br/>- Default: `false` |
| useReadOnly | Body | Boolean | X | Block write load<br/>- Default: `false` |

<details><summary>Example</summary>
<p>

```json
{
    "dbInstanceName": "dbInstanceName",
    "dbInstanceCandidateName": "dbInstanceCandidateName",
    "description": "description-example",
    "dbPort": 1,
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbVersion": "MYSQL_V8036",
    "useSlowQueryAnalysis": false,
    "useDummy": false,
    "dbSecurityGroupIds": [],
    "executeBackup": false,
    "useOnlineFailover": false,
    "waitReplicationDelay": false,
    "useReadOnly": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier for the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View Backup Information

```http
GET /v4.0/db-instances/{dbInstanceId}/backup-info
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | View backup information |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| backupPeriod | Body | Number | Backup retention period (days) |
| ftwrlWaitTimeout | Body | Number | Query delay wait time (seconds) |
| backupRetryCount | Body | Number | Number of backup retries |
| replicationRegion | Body | Enum | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| useBackupLock | Body | Boolean | Whether to use table locking |
| backupSchedules | Body | Array | Backup schedule list |
| backupSchedules.backupWndBgnTime | Body | Time | Backup start time |
| backupSchedules.backupWndDuration | Body | Enum | Backup duration<br/>- HALF_AN_HOUR<br/>- ONE_HOUR<br/>- ONE_HOUR_AND_HALF<br/>- TWO_HOURS<br/>- TWO_HOURS_AND_HALF<br/>- THREE_HOURS |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "backupPeriod": 1,
    "ftwrlWaitTimeout": 1,
    "backupRetryCount": 1,
    "replicationRegion": "KR1",
    "useBackupLock": false,
    "backupSchedules": [
        {
            "backupWndBgnTime": "00:00:00",
            "backupWndDuration": "HALF_AN_HOUR"
        }
    ]
}
```

</p>
</details>

---

### Modify Backup Information

```http
PUT /v4.0/db-instances/{dbInstanceId}/backup-info
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Modify | Modify backup information |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| backupPeriod | Body | Number | X | Backup retention period (days)<br/>- Minimum value: `0`<br/>- Maximum value: `730` |
| ftwrlWaitTimeout | Body | Number | X | Query delay wait time (seconds)<br/>- Minimum value: `0`<br/>- Maximum value: `21600` |
| backupRetryCount | Body | Number | X | Backup retry count<br/>- Minimum value: `0`<br/>- Maximum value: `10` |
| replicationRegion | Body | Enum | X | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| useBackupLock | Body | Boolean | X | Whether to use table lock |
| backupSchedules | Body | Array | X | Backup schedule list |
| backupSchedules.backupWndBgnTime | Body | Time | O | Backup start time |
| backupSchedules.backupWndDuration | Body | Enum | O | Backup duration<br/>- HALF_AN_HOUR: `30 minutes`<br/>- ONE_HOUR: `1 hour`<br/>- ONE_HOUR_AND_HALF: `1 hour 30 minutes`<br/>- TWO_HOURS: `2 hours`<br/>- TWO_HOURS_AND_HALF: `2 hours 30 minutes`<br/>- THREE_HOURS: `3 hours` |

<details><summary>Example</summary>
<p>

```json
{
    "backupPeriod": 0,
    "ftwrlWaitTimeout": 0,
    "backupRetryCount": 0,
    "replicationRegion": "KR1",
    "useBackupLock": false,
    "backupSchedules": [
        {
            "backupWndBgnTime": "00:00:00",
            "backupWndDuration": "HALF_AN_HOUR"
        }
    ]
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View Binary Log List

```http
GET /v4.0/db-instances/{dbInstanceId}/binlogs
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceBinLog.List | View binary log list |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| binLogs | Body | Array | BinLog file list |
| binLogs.binLogFileName | Body | String | BinLog file name |
| binLogs.binLogFileSize | Body | Number | BinLog file size (bytes) |
| binLogs.createdYmdt | Body | DateTime | Created date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "binLogs": [
        {
            "binLogFileName": "binLogFileName-example",
            "binLogFileSize": 1,
            "createdYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Delete Binary Log

```http
POST /v4.0/db-instances/{dbInstanceId}/binlogs/purge
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceBinLog.Purge | Delete binary log |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O |  |
| lastBinLogFileName | Body | String | O | The name of the last BinLog file to delete (files before this file are deleted) |

<details><summary>Example</summary>
<p>

```json
{
    "lastBinLogFileName": "mysql-bin.000010"
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

### View Certificate File List

```http
GET /v4.0/db-instances/{dbInstanceId}/certificates
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceCertificate.List | View certificate file list |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| certificates | Body | Array | Certificate file list |
| certificates.fileName | Body | String | Certificate file name |
| certificates.certificateType | Body | Enum | Certificate type<br/>- CA_FILE<br/>- CERT_FILE<br/>- KEY_FILE |
| certificates.fileSize | Body | Number | Certificate file size (bytes) |
| certificates.createdYmdt | Body | DateTime | Created date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "certificates": [
        {
            "fileName": "fileName-example",
            "certificateType": "CA_FILE",
            "fileSize": 1,
            "createdYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Export Certificate File

```http
POST /v4.0/db-instances/{dbInstanceId}/certificates/upload
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceCertificate.Export | Export certificate file |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |
| certificateTypes | Body | Array | O | List of certificate types to upload |
| tenantId | Body | String | O | Tenant ID of Object Storage where the certificate file will be stored<br/>- Minimum length: `32`<br/>- Maximum length: `32` |
| username | Body | String | O | NHN Cloud member or IAM member ID |
| password | Body | String | O | API password of Object Storage where the certificate file will be stored |
| targetContainer | Body | String | O | Container of Object Storage where the certificate file will be stored |
| objectPath | Body | String | O | Path of the certificate file to be stored in the container |

<details><summary>Example</summary>
<p>

```json
{
    "certificateTypes": [],
    "tenantId": "0123456789abcdef0123456789abcdef",
    "username": "username-example",
    "password": "password-example",
    "targetContainer": "targetContainer-example",
    "objectPath": "objectPath-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### List DB Schemas

```http
GET /v4.0/db-instances/{dbInstanceId}/db-schemas
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceSchema.List | List DB schemas |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | Yes | The identifier of the DB instance |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbSchemas | Body | Array | List of DB schemas |
| dbSchemas.dbSchemaId | Body | UUID | The identifier of the DB schema |
| dbSchemas.dbSchemaName | Body | String | DB schema name |
| dbSchemas.dbSchemaStatus | Body | Enum | Current status of the DB schema<br/>- STABLE<br/>- CREATING<br/>- SYNCING<br/>- DELETING<br/>- DELETED |
| dbSchemas.createdYmdt | Body | DateTime | Creation date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbSchemas": [
        {
            "dbSchemaId": "550e8400-e29b-41d4-a716-446655440000",
            "dbSchemaName": "dbSchemaName-example",
            "dbSchemaStatus": "STABLE",
            "createdYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create DB Schema

```http
POST /v4.0/db-instances/{dbInstanceId}/db-schemas
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceSchema.Create | Create DB schema |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |
| dbSchemaName | Body | String | O | DB schema name<br/>- Maximum length: `64`<br/>- Begin with an English letter, contain only English letters, numbers, and underscore (_), be between 1 and 64 characters long, and must not use MySQL reserved words |

<details><summary>Example</summary>
<p>

```json
{
    "dbSchemaName": "dbSchemaName-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete DB Schema

```http
DELETE /v4.0/db-instances/{dbInstanceId}/db-schemas/{dbSchemaId}
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceSchema.Delete | Delete DB schema |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| dbSchemaId | URL | UUID | O | DB schema identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View DB User List

```http
GET /v4.0/db-instances/{dbInstanceId}/db-users
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceUser.List | View DB user list |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbUsers | Body | Array | DB user list |
| dbUsers.dbUserId | Body | UUID | DB user identifier |
| dbUsers.dbUserName | Body | String | DB user account name |
| dbUsers.host | Body | String | Host name of the DB user account |
| dbUsers.authorityType | Body | Enum | DB user permission type<br/>- CUSTOM: `Custom permissions`<br/>- READ: `Read permissions`<br/>- CRUD: `CRUD permissions`<br/>- DDL: `DDL permissions`<br/>- ALL: `All permissions` |
| dbUsers.dbUserStatus | Body | Enum | Current status of the DB user<br/>- STABLE<br/>- CREATING<br/>- UPDATING<br/>- SYNCING<br/>- DELETING<br/>- DELETED |
| dbUsers.createdYmdt | Body | DateTime | Creation date and time |
| dbUsers.updatedYmdt | Body | DateTime | Modification date and time |
| dbUsers.authenticationPlugin | Body | Enum | User authentication plugin<br/>- NATIVE: `mysql_native_password authentication`<br/>- ED25519: `ed25519 authentication (MariaDB only)` |
| dbUsers.tlsOption | Body | Enum | Certificate options<br/>- NONE: `No TLS`<br/>- SSL: `SSL authentication`<br/>- X509: `X509 certificate authentication` |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbUsers": [
        {
            "dbUserId": "550e8400-e29b-41d4-a716-446655440000",
            "dbUserName": "dbUserName-example",
            "host": "192.168.0.1",
            "authorityType": "CUSTOM",
            "dbUserStatus": "STABLE",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00",
            "authenticationPlugin": "NATIVE",
            "tlsOption": "NONE"
        }
    ]
}
```

</p>
</details>

---

### Create DB User

```http
POST /v4.0/db-instances/{dbInstanceId}/db-users
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceUser.Create | Create DB user |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| dbUserName | Body | String | O | DB user account name<br/>- Minimum length: `1`<br/>- Maximum length: `32` |
| dbPassword | Body | String | O | DB user account password<br/>- Minimum length: `4`<br/>- Maximum length: `256` |
| host | Body | String | O | DB user account host name<br/>- Maximum length: `45` |
| authorityType | Body | Enum | O | DB user authority type<br/>- CUSTOM: `Custom authority`<br/>- READ: `Read authority`<br/>- CRUD: `CRUD authority`<br/>- DDL: `DDL authority`<br/>- ALL: `All authority` |
| authenticationPlugin | Body | Enum | X | User authentication plugin<br/>- NATIVE: `mysql_native_password authentication`<br/>- ED25519: `ed25519 authentication (MariaDB only)` |
| tlsOption | Body | Enum | X | Certificate option<br/>- Default: `NONE`<br/>- NONE: `TLS not used`<br/>- SSL: `SSL authentication`<br/>- X509: `X509 certificate authentication` |

<details><summary>Example</summary>
<p>

```json
{
    "dbUserName": "dbUserName",
    "dbPassword": "dbPassword",
    "host": "192.168.0.1",
    "authorityType": "CUSTOM",
    "authenticationPlugin": "NATIVE",
    "tlsOption": "NONE"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete DB User

```http
DELETE /v4.0/db-instances/{dbInstanceId}/db-users/{dbUserId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceUser.Delete | Delete DB user |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier for the DB instance |
| dbUserId | URL | UUID | O | Identifier for the DB user |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier for the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Modify DB User

```http
PUT /v4.0/db-instances/{dbInstanceId}/db-users/{dbUserId}
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceUser.Modify | Modify DB User |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| dbUserId | URL | UUID | O | DB user identifier |
| dbPassword | Body | String | X | DB user account password<br/>- Minimum length: `4`<br/>- Maximum length: `256` |
| authorityType | Body | Enum | X | DB user permission type<br/>- CUSTOM: `Custom permissions`<br/>- READ: `Read permissions`<br/>- CRUD: `CRUD permissions`<br/>- DDL: `DDL permissions`<br/>- ALL: `All permissions` |
| authenticationPlugin | Body | Enum | X | User authentication plugin<br/>- NATIVE: `mysql_native_password authentication`<br/>- ED25519: `ed25519 authentication (MariaDB only)` |
| tlsOption | Body | Enum | X | Certificate options<br/>- NONE: `TLS not used`<br/>- SSL: `SSL authentication`<br/>- X509: `X509 certificate authentication` |

<details><summary>Example</summary>
<p>

```json
{
    "dbPassword": "dbPassword",
    "authorityType": "CUSTOM",
    "authenticationPlugin": "NATIVE",
    "tlsOption": "NONE"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Change DB Instance Deletion Protection Settings

```http
PUT /v4.0/db-instances/{dbInstanceId}/deletion-protection
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Modify | Change DB instance deletion protection settings |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| useDeletionProtection | Body | Boolean | O | Whether to use deletion protection |

<details><summary>Example</summary>
<p>

```json
{
    "useDeletionProtection": false
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

### Force Restart DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/force-restart
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.ForceRestart | Force restart DB instance |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

This API does not return a response body.

---

### View High Availability Information

```http
GET /v4.0/db-instances/{dbInstanceId}/high-availability
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | View high availability information |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| useHighAvailability | Body | Boolean | Whether to use high availability<br/>- Default value: `false` |
| haStatus | Body | Enum | High availability status<br/>- CREATED: `Created`<br/>- STABLE: `Stable`<br/>- PAUSING: `Pausing`<br/>- DISABLE: `Disabled`<br/>- DISABLE_MASTER_IN_REPLICATION: `High availability stopped due to abnormal master replication detection`<br/>- DISABLE_MHA_PROCESS: `High availability process stopped`<br/>- DISABLE_REPLICATION_STOP: `High availability stopped due to replication down`<br/>- DISABLE_REPLICATION_DELAY: `High availability stopped due to replication delay`<br/>- FAILOVER_STARTED: `Failover started`<br/>- FAILOVER_FAILED: `Failover failed`<br/>- FAILOVER_COMPLETED: `Failover completed`<br/>- DELETED: `Deleted`<br/>- PAUSED: `Paused`<br/>- PAUSED_DUE_TO_TASK: `Paused due to task`<br/>- MASTER_FAILURE_DETECTION: `Master failure detection` |
| pingInterval | Body | Number | Ping interval (seconds) |
| pingType | Body | String | Ping method |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "useHighAvailability": false,
    "haStatus": "CREATED",
    "pingInterval": 1,
    "pingType": "pingType-example"
}
```

</p>
</details>

---

### Modify High Availability

```http
PUT /v4.0/db-instances/{dbInstanceId}/high-availability
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:HighAvailability.Modify | Modify high availability |

#### Common Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| useHighAvailability | Body | Boolean | O | Whether to use high availability |
| pingInterval | Body | Number | X | Ping interval when using high availability (seconds)<br/>- Minimum value: `1`<br/>- Maximum value: `600` |

#### When Using High Availability

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceCandidateName | Body | String | O | Standby master name that can identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |

<details><summary>Example</summary>
<p>

```json
{
    "useHighAvailability": false,
    "pingInterval": 1
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Pause high availability

```http
POST /v4.0/db-instances/{dbInstanceId}/high-availability/pause
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:HighAvailability.Pause | Pause high availability |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Repair High Availability

```http
POST /v4.0/db-instances/{dbInstanceId}/high-availability/repair
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:HighAvailability.Repair | Repair high availability |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Resume High Availability

```http
POST /v4.0/db-instances/{dbInstanceId}/high-availability/resume
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:HighAvailability.Resume | Resume high availability |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier for the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Separate High Availability

```http
POST /v4.0/db-instances/{dbInstanceId}/high-availability/split
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:HighAvailability.Split | Separate high availability |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | Y | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View Log File List

```http
GET /v4.0/db-instances/{dbInstanceId}/log-files
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceLog.List | View log file list |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| logFiles | Body | Array | Log file list |
| logFiles.logFileName | Body | String | Log file name |
| logFiles.logFileType | Body | Enum | Log file type<br/>- ERROR<br/>- BINLOG<br/>- GENERAL<br/>- SLOW_QUERY<br/>- AUDIT<br/>- BACKUP |
| logFiles.logFileSize | Body | Number | Log file size (bytes) |
| logFiles.createdYmdt | Body | DateTime | Created date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "logFiles": [
        {
            "logFileName": "logFileName-example",
            "logFileType": "ERROR",
            "logFileSize": 1,
            "createdYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

### Export Log Files

```http
POST /v4.0/db-instances/{dbInstanceId}/log-files/export
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceLog.Export | Export log files |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O |  |
| logFileNames | Body | Array | O | List of log file names |
| tenantId | Body | String | O | Tenant ID of the Object Storage where log files will be stored<br/>- Minimum length: `32`<br/>- Maximum length: `32` |
| username | Body | String | O | NHN Cloud member or IAM member ID |
| password | Body | String | O | API password for the Object Storage where log files will be stored |
| targetContainer | Body | String | O | Container of the Object Storage where log files will be stored |
| objectPath | Body | String | O | Path of the log file to be stored in the container |

<details><summary>Example</summary>
<p>

```json
{
    "logFileNames": [],
    "tenantId": "0123456789abcdef0123456789abcdef",
    "username": "username-example",
    "password": "password-example",
    "targetContainer": "targetContainer-example",
    "objectPath": "objectPath-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View Log File Content

```http
GET /v4.0/db-instances/{dbInstanceId}/log-files/{logFileName}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstanceLog.Get | View log file content |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| logFileName | URL | UUID | O | Log file name |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| content | Body | String | Log file content (up to 65533 bytes) |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "content": "content-example"
}
```

</p>
</details>

---

### View DB Instance Maintenance List

```http
GET /v4.0/db-instances/{dbInstanceId}/maintenances
```

#### Required Permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Maintenance.List | View DB instance maintenance list |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier for the DB instance |
| type | Query | String | X |  |
| statuses | Query | String | X |  |
| category | Query | String | X |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Number of maintenance items |
| maintenances | Body | Array | Maintenance list |
| maintenances.maintenanceId | Body | UUID | Maintenance ID |
| maintenances.dbInstanceId | Body | UUID | DB instance ID |
| maintenances.category | Body | Enum | Maintenance category<br/>- USER: `User maintenance category`<br/>- PROVIDER: `Provider maintenance category`<br/>- AUTO: `Automatic maintenance category` |
| maintenances.description | Body | String | Maintenance description |
| maintenances.type | Body | Enum | Maintenance type<br/>- UPDATE_DB_INSTANCE: `DB instance modification (specification change, port change, parameter group change)`<br/>- UPGRADE_ENGINE_VERSION: `Engine version upgrade`<br/>- APPLY_CHANGE_PARAMETER: `Parameter change in parameter group`<br/>- UPGRADE_OS: `Operating system version upgrade`<br/>- PATCH_SECURITY: `Security update`<br/>- MIGRATION: `Migration for hypervisor inspection`<br/>- CLEANUP_STORAGE: `Storage cleanup` |
| maintenances.payload | Body | Object | Payload according to maintenance type |
| maintenances.required | Body | Boolean | Whether maintenance is required |
| maintenances.deadlineYmdt | Body | DateTime | Forced maintenance application date and time |
| maintenances.status | Body | Enum | Maintenance status<br/>- PENDING: `Pending`<br/>- READY: `Ready`<br/>- RUNNING: `Running`<br/>- COMPLETED: `Completed`<br/>- FAILED: `Failed`<br/>- EXCLUDED: `Excluded`<br/>- DELETED: `Deleted`<br/>- UNKNOWN |
| maintenances.executionType | Body | Enum | Maintenance execution type<br/>- SCHEDULED: `Scheduled execution (automatic execution during maintenance period)`<br/>- MANUAL: `Manual execution (immediate execution)`<br/>- FORCED: `Forced execution (automatic execution after deadline exceeded)` |
| maintenances.addedYmdt | Body | DateTime | Maintenance schedule registration date and time |
| maintenances.executionStartedYmdt | Body | DateTime | Maintenance start date and time |
| maintenances.executionCompletedYmdt | Body | DateTime | Maintenance end date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "maintenances": [
        {
            "maintenanceId": "550e8400-e29b-41d4-a716-446655440000",
            "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
            "category": "USER",
            "description": "description-example",
            "type": "UPDATE_DB_INSTANCE",
            "payload": {
            },
            "required": false,
            "deadlineYmdt": "2023-12-31T15:00:00+09:00",
            "status": "PENDING",
            "executionType": "SCHEDULED",
            "addedYmdt": "2023-12-31T15:00:00+09:00",
            "executionStartedYmdt": "2023-12-31T15:00:00+09:00",
            "executionCompletedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Execute DB Instance Maintenance Immediately

```http
POST /v4.0/db-instances/{dbInstanceId}/maintenances/execute-now
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Maintenance.Execute | Execute DB instance maintenance immediately |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | Yes | DB instance identifier |
| configId | Body | String | Yes | Configuration ID |
| category | Body | Enum | Yes | Maintenance category<br/>- USER: `User maintenance category`<br/>- PROVIDER: `Provider maintenance category`<br/>- AUTO: `Auto maintenance category` |
| description | Body | String | No | Maintenance description |
| type | Body | Enum | Yes | Maintenance type<br/>- UPDATE_DB_INSTANCE: `Modify DB instance (specification change, port change, parameter group change)`<br/>- UPGRADE_ENGINE_VERSION: `Engine version upgrade`<br/>- APPLY_CHANGE_PARAMETER: `Parameter change in parameter group`<br/>- UPGRADE_OS: `Operating system version upgrade`<br/>- PATCH_SECURITY: `Security update`<br/>- MIGRATION: `Migration for hypervisor maintenance`<br/>- CLEANUP_STORAGE: `Storage cleanup` |
| payload | Body | String | Yes | Payload according to maintenance type |

<details><summary>Example</summary>
<p>

```json
{
    "configId": "configId-example",
    "category": "USER",
    "description": "description-example",
    "type": "UPDATE_DB_INSTANCE",
    "payload": "payload-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Requested job identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Schedule DB Instance Maintenance

```http
POST /v4.0/db-instances/{dbInstanceId}/maintenances/schedule
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Maintenance.Update | Schedule DB instance maintenance |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |
| configId | Body | String | O | Configuration ID |
| category | Body | Enum | O | Maintenance category<br/>- USER: `User maintenance category`<br/>- PROVIDER: `Provider maintenance category`<br/>- AUTO: `Auto maintenance category` |
| description | Body | String | X | Maintenance description |
| type | Body | Enum | O | Maintenance type<br/>- UPDATE_DB_INSTANCE: `DB instance modification (specification change, port change, parameter group change)`<br/>- UPGRADE_ENGINE_VERSION: `Engine version upgrade`<br/>- APPLY_CHANGE_PARAMETER: `Parameter change in parameter group`<br/>- UPGRADE_OS: `Operating system version upgrade`<br/>- PATCH_SECURITY: `Security update`<br/>- MIGRATION: `Migration for hypervisor maintenance`<br/>- CLEANUP_STORAGE: `Storage cleanup` |
| payload | Body | String | O | Payload according to maintenance type |

<details><summary>Example</summary>
<p>

```json
{
    "configId": "configId-example",
    "category": "USER",
    "description": "description-example",
    "type": "UPDATE_DB_INSTANCE",
    "payload": "payload-example"
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

### Delete DB Instance Maintenance

```http
DELETE /v4.0/db-instances/{dbInstanceId}/maintenances/{maintenanceId}
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Maintenance.Delete | Delete DB instance maintenance |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |
| maintenanceId | URL | UUID | O | Maintenance ID |

#### Response

This API does not return a response body.

---

### View Network Information

```http
GET /v4.0/db-instances/{dbInstanceId}/network-info
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | View network information |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| availabilityZone | Body | String | Availability zone where the DB instance is created |
| subnet | Body | Object | Subnet object |
| subnet.subnetId | Body | UUID | Identifier of the subnet |
| subnet.subnetName | Body | String | Name that can identify the subnet |
| subnet.subnetCidr | Body | String | CIDR of the subnet |
| endPoints | Body | Array | List of connection information |
| endPoints.domain | Body | String | Domain |
| endPoints.ipAddress | Body | String | IP address |
| endPoints.endPointType | Body | String | Connection information type |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "availabilityZone": "kr-pub-a",
    "subnet": {
        "subnetId": "550e8400-e29b-41d4-a716-446655440000",
        "subnetName": "subnetName-example",
        "subnetCidr": "192.168.0.0/24"
    },
    "endPoints": [
        {
            "domain": "domain-example",
            "ipAddress": "192.168.0.1",
            "endPointType": "https://example.com"
        }
    ]
}
```

</p>
</details>

---

### Modify Network Information

```http
PUT /v4.0/db-instances/{dbInstanceId}/network-info
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Modify | Modify Network Information |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier for the DB instance |
| usePublicAccess | Body | Boolean | O | Whether external access is enabled |

<details><summary>Example</summary>
<p>

```json
{
    "usePublicAccess": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Promote DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/promote
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Promote | Promote DB instance |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Rebuild DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/rebuild
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Rebuild | Rebuild DB instance |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Replicate DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/replicate
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Replicate | Replicate DB instance |

#### Common Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |
| dbInstanceName | Body | String | O | Name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB instance<br/>- Maximum length: `100` |
| dbFlavorId | Body | UUID | X | Identifier of the DB instance specification |
| dbPort | Body | Number | X | DB port<br/>- Minimum value: 3306, Maximum value: 43306 |
| parameterGroupId | Body | UUID | X | Identifier of the parameter group |
| dbSecurityGroupIds | Body | Array | X | List of DB security group identifiers |
| userGroupIds | Body | Array | X | List of user group identifiers |
| useDefaultNotification | Body | Boolean | X | Whether to use default notifications<br/>- Default value: `false` |
| useDeletionProtection | Body | Boolean | X | Whether to use deletion protection<br/>- Default value: `false` |
| useSlowQueryAnalysis | Body | Boolean | X | Whether to use slow query analysis<br/>- Default value: `true` |
| network | Body | Object | O | Network information object |
| network.usePublicAccess | Body | Boolean | X | Whether to allow external access |
| network.availabilityZone | Body | Enum | O | Availability zone to create the DB instance |
| storage | Body | Object | X | Storage information object |
| storage.storageType | Body | Enum | X | Data storage type |
| storage.storageSize | Body | Number | X | Data storage size (GB)<br/>- Minimum value: `20` |
| storage.storageAutoscale | Body | Object | X | Data storage autoscaling object |
| storage.storageAutoscale.useStorageAutoscale | Body | Boolean | X | Whether to use storage autoscaling<br/>- Default value: `false` |
| backup | Body | Object | X | Backup information object |
| backup.backupPeriod | Body | Number | X | Backup retention period (days)<br/>- Minimum value: `0`<br/>- Maximum value: `730` |
| backup.backupRetryCount | Body | Number | X | Number of backup retries<br/>- Minimum value: `0`<br/>- Maximum value: `10` |
| backup.ftwrlWaitTimeout | Body | Number | X | Query delay wait time (seconds)<br/>- Minimum value: `0`<br/>- Maximum value: `21600` |
| backup.replicationRegion | Body | Enum | X | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| backup.useBackupLock | Body | Boolean | X | Whether to use table lock |
| backup.backupSchedules | Body | Array | X | List of backup schedules |
| backup.backupSchedules.backupWndBgnTime | Body | Time | X | Backup start time |
| backup.backupSchedules.backupWndDuration | Body | Enum | X | Backup duration<br/>- HALF_AN_HOUR: `30 minutes`<br/>- ONE_HOUR: `1 hour`<br/>- ONE_HOUR_AND_HALF: `1 hour 30 minutes`<br/>- TWO_HOURS: `2 hours`<br/>- TWO_HOURS_AND_HALF: `2 hours 30 minutes`<br/>- THREE_HOURS: `3 hours` |

#### When Using Storage Autoscaling

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| storage.storageAutoscale.threshold | Body | Number | O | Autoscaling condition (%)<br/>- Minimum value: `50`<br/>- Maximum value: `95` |
| storage.storageAutoscale.maxStorageSize | Body | Number | O | Maximum autoscaling size (GB)<br/>- Maximum value: `4096` |
| storage.storageAutoscale.cooldownTime | Body | Number | O | Autoscaling cooldown time (minutes)<br/>- Minimum value: `10`<br/>- Maximum value: `1440` |

<details><summary>Example</summary>
<p>

```json
{
    "dbInstanceName": "dbInstanceName",
    "description": "description-example",
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "dbPort": 1,
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupIds": [],
    "userGroupIds": [],
    "useDefaultNotification": false,
    "useDeletionProtection": false,
    "useSlowQueryAnalysis": true,
    "network": {
        "usePublicAccess": false,
        "availabilityZone": "kr-pub-a"
    },
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20,
        "storageAutoscale": {
            "useStorageAutoscale": false
        }
    },
    "backup": {
        "backupPeriod": 0,
        "backupRetryCount": 0,
        "ftwrlWaitTimeout": 0,
        "replicationRegion": "KR1",
        "useBackupLock": false,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "HALF_AN_HOUR"
            }
        ]
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

### Restart DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/restart
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Restart | Restart DB instance |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | The identifier of the DB instance |
| useOnlineFailover | Body | Boolean | X | Whether to restart using failover<br/>- Default: `false` |
| executeBackup | Body | Boolean | X | Whether to proceed with current point-in-time backup<br/>- Default: `false` |
| waitReplicationDelay | Body | Boolean | X | Wait for replication delay resolution<br/>- Default: `false` |
| useReadOnly | Body | Boolean | X | Block write load<br/>- Default: `false` |

<details><summary>Example</summary>
<p>

```json
{
    "useOnlineFailover": false,
    "executeBackup": false,
    "waitReplicationDelay": false,
    "useReadOnly": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | The identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Get DB Instance Restoration Information

```http
GET /v4.0/db-instances/{dbInstanceId}/restoration-info
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | Get DB instance restoration information |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| oldestRestorableYmdt | Body | DateTime | Oldest restorable time |
| latestRestorableYmdt | Body | DateTime | Latest restorable time |
| restorableBackups | Body | Array | List of restorable backups |
| restorableBackups.backup | Body | Object | Backup information object |
| restorableBackups.backup.backupId | Body | UUID | Backup identifier |
| restorableBackups.backup.backupName | Body | String | Backup name |
| restorableBackups.backup.backupStatus | Body | Enum | Backup status<br/>- BACKING_UP: `Backing up (spinner)`<br/>- VERIFYING: `Verifying (spinner)`<br/>- COMPLETED: `Available (green icon)`<br/>- DELETING: `Deleting (spinner)`<br/>- DELETED: `Deleted (gray icon)`<br/>- ERROR: `Error (red icon)` |
| restorableBackups.backup.dbInstanceId | Body | UUID | Original DB instance identifier |
| restorableBackups.backup.dbInstanceName | Body | String | Original DB instance name |
| restorableBackups.backup.dbVersion | Body | Enum | DB engine type |
| restorableBackups.backup.backupType | Body | Enum | Backup type<br/>- AUTO<br/>- MANUAL |
| restorableBackups.backup.backupSize | Body | Number | Backup size |
| restorableBackups.backup.useBackupLock | Body | Boolean | Whether to use table lock |
| restorableBackups.backup.failoverCount | Body | Number | Failover count |
| restorableBackups.backup.binLogFileName | Body | String | Binary log file name |
| restorableBackups.backup.binLogPosition | Body | Object | Binary log file location |
| restorableBackups.backup.createdYmdt | Body | DateTime | Backup creation date and time |
| restorableBackups.backup.updatedYmdt | Body | DateTime | Backup update date and time |
| restorableBackups.restorableBinLogs | Body | Array | List of binary log names that can be restored using the corresponding backup |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "oldestRestorableYmdt": "2023-12-31T15:00:00+09:00",
    "latestRestorableYmdt": "2023-12-31T15:00:00+09:00",
    "restorableBackups": [
        {
            "backup": {
                "backupId": "550e8400-e29b-41d4-a716-446655440000",
                "backupName": "backupName-example",
                "backupStatus": "BACKING_UP",
                "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
                "dbInstanceName": "dbInstanceName-example",
                "dbVersion": "MYSQL_V8036",
                "backupType": "AUTO",
                "backupSize": 1,
                "useBackupLock": false,
                "failoverCount": 1,
                "binLogFileName": "binLogFileName-example",
                "binLogPosition": {
                },
                "createdYmdt": "2023-12-31T15:00:00+09:00",
                "updatedYmdt": "2023-12-31T15:00:00+09:00"
            },
            "restorableBinLogs": []
        }
    ]
}
```

</p>
</details>

---

### Retrieve Last Query for Restoration

```http
GET /v4.0/db-instances/{dbInstanceId}/restoration-info/last-query
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | Retrieve last query for restoration |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| executedYmdt | Body | DateTime | Query execution date and time |
| lastQuery | Body | String | Last executed query |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "executedYmdt": "2023-12-31T15:00:00+09:00",
    "lastQuery": "lastQuery-example"
}
```

</p>
</details>

---

### Restore DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/restore
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Restore | Restore DB instance |

#### Common Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |
| dbInstanceName | Body | String | O | Master name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB instance<br/>- Maximum length: `100` |
| dbFlavorId | Body | UUID | X | Identifier of the DB instance specification. If not entered, the specification of the original instance is applied. |
| dbPort | Body | Number | X | DB port |
| useHighAvailability | Body | Boolean | X | Whether to use high availability<br/>- Default value: `false` |
| pingInterval | Body | Number | X | Ping interval (seconds) when using high availability<br/>- Minimum value: `1`<br/>- Maximum value: `600` |
| storage | Body | Object | X | Storage information object. If not entered, the storage settings of the original instance are applied. |
| storage.storageType | Body | Enum | X | Storage type. If not entered, the storage type of the original instance is applied. |
| storage.storageSize | Body | Number | X | Data storage size (GB). If not entered, the storage size of the original instance is applied.<br/>- Minimum value: `20` |
| storage.storageAutoscale | Body | Object | X | Data storage autoscale object |
| storage.storageAutoscale.useStorageAutoscale | Body | Boolean | X | Whether to use storage autoscale<br/>- Default value: `false` |
| network | Body | Object | X | Network information object. If not entered, the network settings of the original instance are applied. |
| network.subnetId | Body | UUID | X | Identifier of the subnet. If not entered, the original instance value is used |
| network.usePublicAccess | Body | Boolean | X | Whether external access is possible<br/>- Default value: `false` |
| network.availabilityZone | Body | Enum | X | Availability zone to create the DB instance. If not entered, randomly selected |
| backup | Body | Object | X | Backup information object. If not entered, the backup settings of the original instance are applied. |
| backup.backupPeriod | Body | Number | X | Backup retention period (days). If not entered, the backup retention period of the original instance is applied.<br/>- Minimum value: `0`<br/>- Maximum value: `730` |
| backup.ftwrlWaitTimeout | Body | Number | X | Query delay wait time (seconds)<br/>- Minimum value: `0`<br/>- Maximum value: `21600` |
| backup.backupRetryCount | Body | Number | X | Number of backup retries<br/>- Minimum value: `0`<br/>- Maximum value: `10` |
| backup.replicationRegion | Body | Enum | X | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| backup.useBackupLock | Body | Boolean | X | Whether to use table lock<br/>- Default value: `true` |
| backup.backupSchedules | Body | Array | X | Backup schedule list. If not entered, the backup schedule of the original instance is applied. |
| backup.backupSchedules.backupWndBgnTime | Body | Time | X | Backup start time |
| backup.backupSchedules.backupWndDuration | Body | Enum | X | Backup Duration<br/>- HALF_AN_HOUR: `30 minutes`<br/>- ONE_HOUR: `1 hour`<br/>- ONE_HOUR_AND_HALF: `1 hour 30 minutes`<br/>- TWO_HOURS: `2 hours`<br/>- TWO_HOURS_AND_HALF: `2 hours 30 minutes`<br/>- THREE_HOURS: `3 hours` |
| restore | Body | Object | O | Restore information object |
| restore.restoreType | Body | Enum | O | Restore type<br/>- TIMESTAMP: `Restore to a certain point in time using a time within the restorable time range`<br/>- BINLOG: `Restore to a certain point in time using a restorable binary log position`<br/>- BACKUP: `Snapshot backup restore using a previously created backup` |
| restore.binLog.binLogFileName | Body | String | X | Binary log name to use for restoration |
| restore.binLog.binLogPosition | Body | Object | X | Binary log position to use for restoration |
| useDefaultNotification | Body | Boolean | X | Whether to use default notification<br/>- Default value: `false` |
| useSlowQueryAnalysis | Body | Boolean | X | Whether to analyze slow query<br/>- Default value: `true` |
| parameterGroupId | Body | UUID | X | Identifier of the parameter group. If not entered, the parameter group of the original instance is applied. |
| dbSecurityGroupIds | Body | Array | X | List of DB security group identifiers. If not entered, the security groups of the original instance are applied. |
| userGroupIds | Body | Array | X | List of user group identifiers |
| useDeletionProtection | Body | Boolean | X | Whether to use deletion protection<br/>- Default value: `false` |

#### When Using High Availability

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceCandidateName | Body | String | O | Candidate master name to identify the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |

#### When Using Storage Autoscale

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| storage.storageAutoscale.threshold | Body | Number | O | Autoscale condition (%)<br/>- Minimum value: `50`<br/>- Maximum value: `95` |
| storage.storageAutoscale.maxStorageSize | Body | Number | O | Maximum autoscale size (GB)<br/>- Maximum value: `4096` |
| storage.storageAutoscale.cooldownTime | Body | Number | O | Autoscale cooldown time (minutes)<br/>- Minimum value: `10`<br/>- Maximum value: `1440` |

#### Request for Restore to a Certain Point in Time Using Timestamp (when restoreType is `TIMESTAMP`)

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| restore.restoreYmdt | Body | DateTime | X | DB instance restore date and time (YYYY-MM-DDThh:mm:ss.SSSTZD) |

Restoration is only possible for times before the most recent restorable time retrieved through restore information inquiry.

#### Request for Restore to a Certain Point in Time Using Binary Log (when restoreType is `BINLOG`)

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| restore.backupId | Body | UUID | X | Identifier of the backup to use for restoration |
| restore.binLog | Body | Object | X | Binary log information object to use for restoration |

When using binary log for restore to a certain point in time, restoration is possible for logs recorded after the binary log file and position of the baseline backup.

#### Request for Restore Using Backup (when restoreType is `BACKUP`)

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| restore.backupId | Body | UUID | X | Identifier of the backup to use for restoration |

<details><summary>Example</summary>
<p>

```json
{
    "dbInstanceName": "dbInstanceName",
    "description": "description-example",
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "dbPort": 1,
    "useHighAvailability": false,
    "pingInterval": 3,
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20,
        "storageAutoscale": {
            "useStorageAutoscale": false
        }
    },
    "network": {
        "subnetId": "550e8400-e29b-41d4-a716-446655440000",
        "usePublicAccess": false,
        "availabilityZone": "kr-pub-a"
    },
    "backup": {
        "backupPeriod": 0,
        "ftwrlWaitTimeout": 1800,
        "backupRetryCount": 0,
        "replicationRegion": "KR1",
        "useBackupLock": true,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "HALF_AN_HOUR"
            }
        ]
    },
    "restore": {
        "restoreType": "TIMESTAMP",
        "binLog": {
            "binLogFileName": "binLogFileName-example",
            "binLogPosition": {
            }
        }
    },
    "useDefaultNotification": false,
    "useSlowQueryAnalysis": true,
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupIds": [],
    "userGroupIds": [],
    "useDeletionProtection": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Start DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/start
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Start | Start DB instance |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | Identifier of the DB instance |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Stop DB Instance

```http
POST /v4.0/db-instances/{dbInstanceId}/stop
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Stop | Stop DB instance |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### View Storage Information

```http
GET /v4.0/db-instances/{dbInstanceId}/storage-info
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Get | View storage information |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| storageType | Body | String | Data storage type |
| storageSize | Body | Number | Data storage size (GB) |
| storageStatus | Body | Enum | Current status of data storage<br/>- DELETED: `Deleted`<br/>- PENDING_DELETION: `Deletion deferred`<br/>- DELETION_RESERVED: `Deletion reserved (waiting for snapshot cleanup)`<br/>- DETACHED: `Detached`<br/>- ATTACHED: `Attached` |
| storageAutoscale | Body | Object | Data storage auto scaling object |
| storageAutoscale.useStorageAutoscale | Body | Boolean | Whether storage auto scaling is enabled |
| storageAutoscale.threshold | Body | Number | Auto scaling condition (%) |
| storageAutoscale.maxStorageSize | Body | Number | Auto scaling maximum size (GB) |
| storageAutoscale.cooldownTime | Body | Number | Auto scaling cooldown time (minutes) |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "storageType": "General SSD",
    "storageSize": 1,
    "storageStatus": "DELETED",
    "storageAutoscale": {
        "useStorageAutoscale": false,
        "threshold": 1,
        "maxStorageSize": 1,
        "cooldownTime": 1
    }
}
```

</p>
</details>

---

### Modify Storage Information

```http
PUT /v4.0/db-instances/{dbInstanceId}/storage-info
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:DbInstance.Modify | Modify storage information |

#### Common Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceId | URL | UUID | O | DB instance identifier |
| storageSize | Body | Number | O | Data storage size (GB)<br/>- Maximum value: `2048` |
| storageAutoscale | Body | Object | X | Data storage autoscaling object |
| storageAutoscale.useStorageAutoscale | Body | Boolean | X | Whether to enable storage autoscaling |

#### When Using Storage Autoscaling

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| storageAutoscale.threshold | Body | Number | O | Autoscaling condition (%)<br/>- Minimum value: `50`<br/>- Maximum value: `95` |
| storageAutoscale.maxStorageSize | Body | Number | O | Maximum autoscaling size (GB)<br/>- Maximum value: `4096` |
| storageAutoscale.cooldownTime | Body | Number | O | Autoscaling cooldown time (minutes)<br/>- Minimum value: `10`<br/>- Maximum value: `1440` |

<details><summary>Example</summary>
<p>

```json
{
    "storageSize": 1,
    "storageAutoscale": {
        "useStorageAutoscale": false
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

## Backup

### Backup Status

| Status           | Description           |
|--------------|--------------|
| `BACKING_UP` | When backing up     |
| `COMPLETED`  | When backup is completed   |
| `DELETING`   | When backup is being deleted |
| `DELETED`    | When backup is deleted   |
| `ERROR`      | When an error occurs   |

### Get Backup List

```http
GET /v4.0/backups
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Backup.List | Get backup list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Total number of backup lists |
| backups | Body | Array | Backup list |
| backups.backupId | Body | UUID | Backup identifier |
| backups.backupName | Body | String | Name that can identify the backup |
| backups.backupStatus | Body | Enum | Current status of the backup<br/>- BACKING_UP: `Backing up (spinner)`<br/>- VERIFYING: `Verifying (spinner)`<br/>- COMPLETED: `Available (green icon)`<br/>- DELETING: `Deleting (spinner)`<br/>- DELETED: `Deleted (gray icon)`<br/>- ERROR: `Error (red icon)` |
| backups.dbInstanceId | Body | UUID | Identifier of the source DB instance |
| backups.dbVersion | Body | Enum | DB engine type |
| backups.utilVersion | Body | String | Utility version |
| backups.backupType | Body | Enum | Backup type<br/>- AUTO<br/>- MANUAL |
| backups.backupSize | Body | Number | Backup size (bytes) |
| backups.createdYmdt | Body | DateTime | Creation date and time |
| backups.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "backups": [
        {
            "backupId": "550e8400-e29b-41d4-a716-446655440000",
            "backupName": "backupName-example",
            "backupStatus": "BACKING_UP",
            "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
            "dbVersion": "MYSQL_V8036",
            "utilVersion": "utilVersion-example",
            "backupType": "AUTO",
            "backupSize": 1,
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create Backup

```http
POST /v4.0/backups
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Backup.Create | Create backup |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| backupName | Body | String | O | A name to identify the backup<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| baseBackupId | Body | UUID | X | Identifier of the original backup |
| dbInstanceId | Body | UUID | X | DB instance identifier |
| backupMethodType | Body | Enum | O | Backup method type<br/>- FULL: `Full backup`<br/>- INCREMENTAL: `Incremental backup`<br/>- SNAPSHOT: `Snapshot backup` |

<details><summary>Example</summary>
<p>

```json
{
    "backupName": "backupName",
    "baseBackupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
    "backupMethodType": "FULL"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete Backup

```http
DELETE /v4.0/backups/{backupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Backup.Delete | Delete backup |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| backupId | URL | UUID | Yes |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Get Single Backup

```http
GET /v4.0/backups/{backupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Backup.Get | Get single backup |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| backupId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| backup | Body | Object | Backup details |
| backup.backupId | Body | UUID | Backup identifier |
| backup.regionCode | Body | Enum | Region code<br/>- KR1: `Korea (Pangyo)` |
| backup.backupName | Body | String | Name to identify the backup |
| backup.backupStatus | Body | Enum | Current status of the backup<br/>- BACKING_UP: `Backing up (spinner)`<br/>- VERIFYING: `Verifying (spinner)`<br/>- COMPLETED: `Available (green icon)`<br/>- DELETING: `Deleting (spinner)`<br/>- DELETED: `Deleted (gray icon)`<br/>- ERROR: `Error (red icon)` |
| backup.dbInstanceId | Body | UUID | Original DB instance identifier |
| backup.dbInstanceName | Body | String | Original DB instance name |
| backup.dbVersion | Body | Enum | DB engine version |
| backup.utilVersion | Body | String | Utility version |
| backup.backupType | Body | Enum | Backup type (AUTO, MANUAL)<br/>- AUTO<br/>- MANUAL |
| backup.backupMethodType | Body | Enum | Backup method (FULL, SNAPSHOT, INCREMENTAL)<br/>- FULL<br/>- INCREMENTAL<br/>- SNAPSHOT |
| backup.backupFileType | Body | Enum | Backup file type<br/>- XBSTREAM<br/>- TAR_ZSTD<br/>- TAR_LZ4<br/>- TAR_GZIP<br/>- SNAPSHOT |
| backup.backupSize | Body | Number | Backup size (Byte) |
| backup.isReplicable | Body | Boolean | Whether replication is possible |
| backup.binLogFileName | Body | String | Binary log file name |
| backup.binLogPosition | Body | Object | Binary log position |
| backup.createdYmdt | Body | DateTime | Creation date and time |
| backup.updatedYmdt | Body | DateTime | Update date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "backup": {
        "backupId": "550e8400-e29b-41d4-a716-446655440000",
        "regionCode": "KR1",
        "backupName": "backupName-example",
        "backupStatus": "BACKING_UP",
        "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
        "dbInstanceName": "dbInstanceName-example",
        "dbVersion": "MYSQL_V8036",
        "utilVersion": "utilVersion-example",
        "backupType": "AUTO",
        "backupMethodType": "FULL",
        "backupFileType": "XBSTREAM",
        "backupSize": 1,
        "isReplicable": false,
        "binLogFileName": "binLogFileName-example",
        "binLogPosition": {
        },
        "createdYmdt": "2023-12-31T15:00:00+09:00",
        "updatedYmdt": "2023-12-31T15:00:00+09:00"
    }
}
```

</p>
</details>

---

### Export Backup

```http
POST /v4.0/backups/{backupId}/export
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Backup.Export | Export backup |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| backupId | URL | UUID | O |  |
| tenantId | Body | String | O | Tenant ID of the Object Storage where the backup is stored<br/>- Minimum length: `32`<br/>- Maximum length: `32` |
| username | Body | String | O | NHN Cloud account or IAM member ID |
| password | Body | String | O | API password of the Object Storage where the backup is stored |
| targetContainer | Body | String | O | Container of the Object Storage where the backup is stored |
| objectPath | Body | String | O | Path of the backup to be stored in the container |

<details><summary>Example</summary>
<p>

```json
{
    "tenantId": "0123456789abcdef0123456789abcdef",
    "username": "username-example",
    "password": "password-example",
    "targetContainer": "targetContainer-example",
    "objectPath": "objectPath-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Restore Backup

```http
POST /v4.0/backups/{backupId}/restore
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Backup.Restore | Restore backup |

#### Common Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| backupId | URL | UUID | O |  |
| dbInstanceName | Body | String | O | Master name that identifies the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB instance<br/>- Maximum length: `100` |
| dbFlavorId | Body | UUID | X | Identifier of the DB instance specification. If not specified, uses the original instance value |
| dbPort | Body | Number | X | DB port. If not specified, uses the original instance value<br/>- Minimum value: 3306, Maximum value: 43306 |
| parameterGroupId | Body | UUID | X | Identifier of the parameter group. If not specified, uses the original instance value |
| dbSecurityGroupIds | Body | Array | X | List of DB security group identifiers |
| userGroupIds | Body | Array | X | List of user group identifiers |
| useHighAvailability | Body | Boolean | X | Whether to use high availability<br/>- Default: `false` |
| pingInterval | Body | Number | X | Ping interval in seconds when using high availability<br/>- Default: `3`<br/>- Minimum value: `1`<br/>- Maximum value: `600` |
| useDefaultNotification | Body | Boolean | X | Whether to use default notifications<br/>- Default: `false` |
| useDeletionProtection | Body | Boolean | X | Whether to enable deletion protection<br/>- Default: `false` |
| useSlowQueryAnalysis | Body | Boolean | X | Whether to enable slow query analysis<br/>- Default: `true` |
| network | Body | Object | X | Network information object. If not specified, uses the original instance value |
| network.subnetId | Body | UUID | X | Identifier of the subnet. If not specified, uses the original instance value |
| network.usePublicAccess | Body | Boolean | X | Whether to allow external access<br/>- Default: `false` |
| network.availabilityZone | Body | Enum | X | Availability zone where the DB instance will be created. If not specified, randomly selected |
| storage | Body | Object | X | Storage information object. If not specified, uses the original instance value |
| storage.storageType | Body | Enum | X | Storage type. If not specified, uses the original instance value |
| storage.storageSize | Body | Number | X | Data storage size in GB. If not specified, uses the original instance value<br/>- Minimum value: `20` |
| storage.storageAutoscale | Body | Object | X | Data storage auto-scaling object. If not specified, uses the original instance value |
| storage.storageAutoscale.useStorageAutoscale | Body | Boolean | X | Whether to enable storage auto-scaling<br/>- Default: `false` |
| backup | Body | Object | X | Backup information object. If not specified, uses the original instance backup settings |
| backup.backupPeriod | Body | Number | X | Backup retention period in days. If not specified, uses the original instance value<br/>- Minimum value: `0`<br/>- Maximum value: `730` |
| backup.backupRetryCount | Body | Number | X | Number of backup retries. If not specified, uses the original instance value<br/>- Minimum value: `0`<br/>- Maximum value: `10` |
| backup.ftwrlWaitTimeout | Body | Number | X | Query delay wait time in seconds. If not specified, uses the original instance value<br/>- Minimum value: `0`<br/>- Maximum value: `21600` |
| backup.replicationRegion | Body | Enum | X | Backup replication region<br/>- KR1: `Korea (Pangyo)` |
| backup.useBackupLock | Body | Boolean | X | Whether to use table lock. If not specified, uses the original instance value |
| backup.backupSchedules | Body | Array | X | List of backup schedules. If not specified, uses the original instance value |
| backup.backupSchedules.backupWndBgnTime | Body | Time | O | Backup start time |
| backup.backupSchedules.backupWndDuration | Body | Enum | O | Backup duration<br/>- HALF_AN_HOUR: `30 minutes`<br/>- ONE_HOUR: `1 hour`<br/>- ONE_HOUR_AND_HALF: `1 hour 30 minutes`<br/>- TWO_HOURS: `2 hours`<br/>- TWO_HOURS_AND_HALF: `2 hours 30 minutes`<br/>- THREE_HOURS: `3 hours` |

#### When Using High Availability

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbInstanceCandidateName | Body | String | O | Candidate master name that identifies the DB instance<br/>- Minimum length: `1`<br/>- Maximum length: `100` |

#### When Using Storage Auto-scaling

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| storage.storageAutoscale.threshold | Body | Number | O | Auto-scaling threshold (%)<br/>- Minimum value: `50`<br/>- Maximum value: `95` |
| storage.storageAutoscale.maxStorageSize | Body | Number | O | Maximum auto-scaling size in GB<br/>- Maximum value: `4096` |
| storage.storageAutoscale.cooldownTime | Body | Number | O | Auto-scaling cooldown time in minutes<br/>- Minimum value: `10`<br/>- Maximum value: `1440` |

<details><summary>Example</summary>
<p>

```json
{
    "dbInstanceName": "dbInstanceName",
    "description": "description-example",
    "dbFlavorId": "550e8400-e29b-41d4-a716-446655440000",
    "dbPort": 1,
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupIds": [],
    "userGroupIds": [],
    "useHighAvailability": false,
    "pingInterval": 3,
    "useDefaultNotification": false,
    "useDeletionProtection": false,
    "useSlowQueryAnalysis": true,
    "network": {
        "subnetId": "550e8400-e29b-41d4-a716-446655440000",
        "usePublicAccess": false,
        "availabilityZone": "kr-pub-a"
    },
    "storage": {
        "storageType": "General SSD",
        "storageSize": 20,
        "storageAutoscale": {
            "useStorageAutoscale": false
        }
    },
    "backup": {
        "backupPeriod": 0,
        "backupRetryCount": 0,
        "ftwrlWaitTimeout": 0,
        "replicationRegion": "KR1",
        "useBackupLock": false,
        "backupSchedules": [
            {
                "backupWndBgnTime": "00:00:00",
                "backupWndDuration": "HALF_AN_HOUR"
            }
        ]
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Identifier of the requested job |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

## DB Security Group

### DB Security Group Progress Status

| Status              | Description           |
|-----------------|--------------|
| `NONE`          | No job in progress |
| `CREATING_RULE` | Creating rule policy   |
| `UPDATING_RULE` | Updating rule policy   |
| `DELETING_RULE` | Deleting rule policy   |

### View DB security group list

```http
GET /v4.0/db-security-groups
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroup.List | View DB security group list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Total count of DB security groups |
| dbSecurityGroups | Body | Array | DB security group list |
| dbSecurityGroups.dbSecurityGroupId | Body | UUID | Identifier of the DB security group |
| dbSecurityGroups.dbSecurityGroupName | Body | String | Name to identify the DB security group |
| dbSecurityGroups.description | Body | String | Additional information about the DB security group |
| dbSecurityGroups.progressStatus | Body | Enum | Current progress status of the DB security group<br/>- NONE: `None`<br/>- CREATING_RULE: `Creating rule`<br/>- UPDATING_RULE: `Updating rule`<br/>- DELETING_RULE: `Deleting rule`<br/>- APPLYING_DEFAULT_RULE: `Applying default rule` |
| dbSecurityGroups.createdYmdt | Body | DateTime | Creation date and time |
| dbSecurityGroups.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "dbSecurityGroups": [
        {
            "dbSecurityGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "dbSecurityGroupName": "dbSecurityGroupName-example",
            "description": "description-example",
            "progressStatus": "NONE",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create DB Security Group

```http
POST /v4.0/db-security-groups
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroup.Create | Create DB security group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupName | Body | String | O | A name that can identify the DB security group<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB security group<br/>- Maximum length: `100` |
| rules | Body | Array | O | List of DB security group rules |
| rules.direction | Body | Enum | O | Communication direction<br/>- INGRESS: `Inbound`<br/>- EGRESS: `Outbound` |
| rules.etherType | Body | Enum | O | Ether type<br/>- IPV4: `IPv4 format`<br/>- IPV6: `IPv6 format` |
| rules.port | Body | Object | O | Port object |
| rules.port.portType | Body | Enum | O | Port type<br/>- ALL: `All port ranges (not used in user console)`<br/>- PORT: `Specific port`<br/>- DB_PORT: `DB inbound port`<br/>- PORT_RANGE: `Port range` |
| rules.port.minPort | Body | Number | X | Minimum port range<br/>- Minimum value: `3306` |
| rules.port.maxPort | Body | Number | X | Maximum port range<br/>- Maximum value: `65535` |
| rules.cidr | Body | String | O | CIDR |
| rules.description | Body | String | X | Additional information about the security group rule |

<details><summary>Example</summary>
<p>

```json
{
    "dbSecurityGroupName": "dbSecurityGroupName",
    "description": "description-example",
    "rules": [
        {
            "direction": "INGRESS",
            "etherType": "IPV4",
            "port": {
                "portType": "ALL",
                "minPort": 3306,
                "maxPort": 1
            },
            "cidr": "192.168.0.0/24",
            "description": "description-example"
        }
    ]
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbSecurityGroupId | Body | UUID | DB security group identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbSecurityGroupId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete DB Security Group

```http
DELETE /v4.0/db-security-groups/{dbSecurityGroupId}
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroup.Delete | Delete DB security group |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupId | URL | UUID | O |  |

#### Response

This API does not return a response body.

---

### View DB Security Group Details

```http
GET /v4.0/db-security-groups/{dbSecurityGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroup.Get | View DB security group details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| dbSecurityGroupId | Body | UUID | DB security group identifier |
| dbSecurityGroupName | Body | String | Name that can identify the DB security group |
| description | Body | String | Additional information about the DB security group |
| progressStatus | Body | Enum | Current progress status of the DB security group<br/>- NONE: `None`<br/>- CREATING_RULE: `Creating rule`<br/>- UPDATING_RULE: `Updating rule`<br/>- DELETING_RULE: `Deleting rule`<br/>- APPLYING_DEFAULT_RULE: `Applying default rule` |
| rules | Body | Array | DB security group rule list |
| rules.ruleId | Body | UUID | DB security group rule identifier |
| rules.description | Body | String | Additional information about the DB security group rule |
| rules.direction | Body | Enum | Communication direction<br/>- INGRESS: `Inbound`<br/>- EGRESS: `Outbound` |
| rules.etherType | Body | Enum | Ether type<br/>- IPV4: `IPv4 format`<br/>- IPV6: `IPv6 format` |
| rules.port | Body | Object | Port object |
| rules.port.portType | Body | Enum | Port type<br/>- ALL: `All port ranges (not used in user console)`<br/>- PORT: `Specific port`<br/>- DB_PORT: `DB inbound port`<br/>- PORT_RANGE: `Port range` |
| rules.port.minPort | Body | Number | Minimum port range |
| rules.port.maxPort | Body | Number | Maximum port range |
| rules.cidr | Body | String | CIDR |
| rules.createdYmdt | Body | DateTime | Creation date and time |
| rules.updatedYmdt | Body | DateTime | Modification date and time |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "dbSecurityGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "dbSecurityGroupName": "dbSecurityGroupName-example",
    "description": "description-example",
    "progressStatus": "NONE",
    "rules": [
        {
            "ruleId": "550e8400-e29b-41d4-a716-446655440000",
            "description": "description-example",
            "direction": "INGRESS",
            "etherType": "IPV4",
            "port": {
                "portType": "ALL",
                "minPort": 1,
                "maxPort": 1
            },
            "cidr": "192.168.0.0/24",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ],
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

### Modify DB Security Group

```http
PUT /v4.0/db-security-groups/{dbSecurityGroupId}
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroup.Modify | Modify DB security group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupId | URL | UUID | O |  |
| dbSecurityGroupName | Body | String | X | A name that identifies the DB security group<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the DB security group<br/>- Maximum length: `100` |

<details><summary>Example</summary>
<p>

```json
{
    "dbSecurityGroupName": "dbSecurityGroupName",
    "description": "description-example"
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

### Delete DB Security Group Rules

```http
DELETE /v4.0/db-security-groups/{dbSecurityGroupId}/rules
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroupRule.Delete | Delete DB security group rules |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupId | URL | UUID | O |  |
| ruleIds | Query | String | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Job identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Create DB Security Group Rule

```http
POST /v4.0/db-security-groups/{dbSecurityGroupId}/rules
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroupRule.Create | Create DB security group rule |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupId | URL | UUID | O |  |
| direction | Body | Enum | O | Communication direction<br/>- INGRESS: `Inbound`<br/>- EGRESS: `Outbound` |
| etherType | Body | Enum | O | Ether type<br/>- IPV4: `IPv4 format`<br/>- IPV6: `IPv6 format` |
| port | Body | Object | O | Port object |
| port.portType | Body | Enum | O | Port type<br/>- ALL: `All port ranges (not used in user console)`<br/>- PORT: `Specific port`<br/>- DB_PORT: `DB inbound port`<br/>- PORT_RANGE: `Port range` |
| port.minPort | Body | Number | X | Minimum port range<br/>- Minimum value: `3306` |
| port.maxPort | Body | Number | X | Maximum port range<br/>- Maximum value: `65535` |
| cidr | Body | String | O | CIDR |
| description | Body | String | X | Additional information about the DB security group rule<br/>- Maximum length: `200` |

<details><summary>Example</summary>
<p>

```json
{
    "direction": "INGRESS",
    "etherType": "IPV4",
    "port": {
        "portType": "ALL",
        "minPort": 3306,
        "maxPort": 1
    },
    "cidr": "192.168.0.0/24",
    "description": "description-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Job identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Modify DB Security Group Rules

```http
PUT /v4.0/db-security-groups/{dbSecurityGroupId}/rules/{ruleId}
```

#### Required permissions

| Permission name | Description |
|-----|-----|
| RDSforMariaDB:DbSecurityGroupRule.Modify | Modify DB security group rules |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| dbSecurityGroupId | URL | UUID | O |  |
| ruleId | URL | UUID | O |  |
| direction | Body | Enum | O | Communication direction<br/>- INGRESS: `Inbound`<br/>- EGRESS: `Outbound` |
| etherType | Body | Enum | O | Ether type<br/>- IPV4: `IPv4 format`<br/>- IPV6: `IPv6 format` |
| port | Body | Object | O | Port object |
| port.portType | Body | Enum | O | Port type<br/>- ALL: `All port ranges (not used in user console)`<br/>- PORT: `Specific port`<br/>- DB_PORT: `DB inbound port`<br/>- PORT_RANGE: `Port range` |
| port.minPort | Body | Number | X | Minimum port range<br/>- Minimum value: `3306` |
| port.maxPort | Body | Number | X | Maximum port range<br/>- Maximum value: `65535` |
| cidr | Body | String | O | CIDR |
| description | Body | String | X | Additional information about DB security group rules<br/>- Maximum length: `200` |

<details><summary>Example</summary>
<p>

```json
{
    "direction": "INGRESS",
    "etherType": "IPV4",
    "port": {
        "portType": "ALL",
        "minPort": 3306,
        "maxPort": 1
    },
    "cidr": "192.168.0.0/24",
    "description": "description-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| jobId | Body | UUID | Job identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "jobId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

## Parameter Groups

### View Parameter Group List

```http
GET /v4.0/parameter-groups
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.List | View parameter group list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Total number of parameter groups |
| parameterGroups | Body | Array | Parameter group list |
| parameterGroups.parameterGroupId | Body | UUID | Parameter group identifier |
| parameterGroups.parameterGroupName | Body | String | Name that can identify the parameter group |
| parameterGroups.description | Body | String | Additional information about the parameter group |
| parameterGroups.dbVersion | Body | Enum | DB engine type |
| parameterGroups.parameterGroupType | Body | Enum | Parameter group type<br/>- USER<br/>- ADMIN<br/>- DEFAULT<br/>- CLUSTER_USER |
| parameterGroups.parameterGroupStatus | Body | Enum | Current status of the parameter group<br/>- STABLE: `Applied`<br/>- NEED_TO_APPLY: `Need to apply`<br/>- DELETED: `Deleted` |
| parameterGroups.createdYmdt | Body | DateTime | Creation date and time |
| parameterGroups.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "parameterGroups": [
        {
            "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "parameterGroupName": "parameterGroupName-example",
            "description": "description-example",
            "dbVersion": "MYSQL_V8036",
            "parameterGroupType": "USER",
            "parameterGroupStatus": "STABLE",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create Parameter Group

```http
POST /v4.0/parameter-groups
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Create | Create parameter group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupName | Body | String | O | Name that can identify the parameter group<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the parameter group<br/>- Maximum length: `100` |
| dbVersion | Body | Enum | O | DB engine type |

<details><summary>Example</summary>
<p>

```json
{
    "parameterGroupName": "parameterGroupName",
    "description": "description-example",
    "dbVersion": "MYSQL_V8036"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| parameterGroupId | Body | UUID | Parameter group identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete Parameter Group

```http
DELETE /v4.0/parameter-groups/{parameterGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Delete | Delete parameter group |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupId | URL | UUID | O |  |

#### Response

This API does not return a response body.

---

### View Parameter Group Details

```http
GET /v4.0/parameter-groups/{parameterGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Get | View parameter group details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| parameterGroupId | Body | UUID | Parameter group identifier |
| parameterGroupName | Body | String | Name that can identify the parameter group |
| description | Body | String | Additional information about the parameter group |
| dbVersion | Body | Enum | DB engine type |
| parameterGroupStatus | Body | Enum | Current status of the parameter group<br/>- STABLE: `Applied`<br/>- NEED_TO_APPLY: `Need to apply`<br/>- DELETED: `Deleted` |
| parameters | Body | Array | Parameter list |
| parameters.parameterId | Body | UUID | Parameter identifier |
| parameters.parameterFileGroup | Body | Enum | Parameter file group type<br/>- CLIENT<br/>- MYSQL<br/>- MYSQLD |
| parameters.parameterName | Body | String | Parameter name |
| parameters.fileParameterName | Body | String | Parameter file name |
| parameters.value | Body | String | Currently configured value |
| parameters.defaultValue | Body | String | Default value |
| parameters.allowedValue | Body | String | Allowed values |
| parameters.updateType | Body | Enum | Update type<br/>- VARIABLE<br/>- CONSTANT<br/>- INIT_VARIABLE |
| parameters.applyType | Body | Enum | Application type<br/>- BOTH<br/>- SESSION<br/>- FILE |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "parameterGroupName": "parameterGroupName-example",
    "description": "description-example",
    "dbVersion": "MYSQL_V8036",
    "parameterGroupStatus": "STABLE",
    "parameters": [
        {
            "parameterId": "550e8400-e29b-41d4-a716-446655440000",
            "parameterFileGroup": "CLIENT",
            "parameterName": "parameterName-example",
            "fileParameterName": "fileParameterName-example",
            "value": "value-example",
            "defaultValue": "defaultValue-example",
            "allowedValue": "allowedValue-example",
            "updateType": "VARIABLE",
            "applyType": "BOTH"
        }
    ],
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

### Modify Parameter Group

```http
PUT /v4.0/parameter-groups/{parameterGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Modify | Modify parameter group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupId | URL | UUID | O |  |
| parameterGroupName | Body | String | X | Name that can identify the parameter group<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the parameter group<br/>- Maximum length: `100` |

<details><summary>Example</summary>
<p>

```json
{
    "parameterGroupName": "parameterGroupName",
    "description": "description-example"
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

### Copy Parameter Group

```http
POST /v4.0/parameter-groups/{parameterGroupId}/copy
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Copy | Copy parameter group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupId | URL | UUID | O |  |
| parameterGroupName | Body | String | O | Name that can identify the parameter group<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| description | Body | String | X | Additional information about the parameter group<br/>- Maximum length: `100` |

<details><summary>Example</summary>
<p>

```json
{
    "parameterGroupName": "parameterGroupName",
    "description": "description-example"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| parameterGroupId | Body | UUID | Parameter group identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "parameterGroupId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Modify Parameters

```http
PUT /v4.0/parameter-groups/{parameterGroupId}/parameters
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Modify | Modify parameters |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupId | URL | UUID | O |  |
| modifiedParameters | Body | Array | O | List of parameters to change |
| modifiedParameters.parameterId | Body | UUID | O | Parameter identifier |
| modifiedParameters.value | Body | String | O | Parameter value to change |

<details><summary>Example</summary>
<p>

```json
{
    "modifiedParameters": [
        {
            "parameterId": "550e8400-e29b-41d4-a716-446655440000",
            "value": "value-example"
        }
    ]
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

### Reset Parameter Group

```http
PUT /v4.0/parameter-groups/{parameterGroupId}/reset
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:ParameterGroup.Reset | Reset parameter group |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| parameterGroupId | URL | UUID | O |  |

#### Response

This API does not return a response body.

---

## User Groups

### View User Group List

```http
GET /v4.0/user-groups
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:UserGroup.List | View user group list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Total number of user groups |
| userGroups | Body | Array | User group list |
| userGroups.userGroupId | Body | UUID | User group identifier |
| userGroups.userGroupName | Body | String | Name that can identify the user group |
| userGroups.createdYmdt | Body | DateTime | Creation date and time |
| userGroups.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "userGroups": [
        {
            "userGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "userGroupName": "userGroupName-example",
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create User Group

```http
POST /v4.0/user-groups
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:UserGroup.Create | Create user group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| userGroupName | Body | String | Yes | Name that can identify the user group |
| memberIds | Body | Array | Yes | List of project member identifiers |
| selectAll | Body | Boolean | No | Whether to include all project members<br/>- Default: `false` |

<details><summary>Example</summary>
<p>

```json
{
    "userGroupName": "userGroupName-example",
    "memberIds": [],
    "selectAll": false
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| userGroupId | Body | UUID | User group identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "userGroupId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete User Group

```http
DELETE /v4.0/user-groups/{userGroupId}
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:UserGroup.Delete | Delete user group |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| userGroupId | URL | UUID | Yes |  |

#### Response

This API does not return a response body.

---

### View User Group Details

```http
GET /v4.0/user-groups/{userGroupId}
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:UserGroup.Get | View user group details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| userGroupId | URL | UUID | Yes |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| userGroupId | Body | UUID | User group identifier |
| userGroupName | Body | String | Name that can identify the user group |
| userGroupTypeCode | Body | Enum | User group type<br/>- ENTIRE<br/>- INDIVIDUAL_MEMBER |
| members | Body | Array | Project member list |
| members.memberId | Body | UUID | Project member identifier |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "userGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "userGroupName": "userGroupName-example",
    "userGroupTypeCode": "ENTIRE",
    "members": [
        {
            "memberId": "550e8400-e29b-41d4-a716-446655440000"
        }
    ],
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

### Modify User Group

```http
PUT /v4.0/user-groups/{userGroupId}
```

#### Required Permissions

| Permission | Description |
|-----|-----|
| RDSforMariaDB:UserGroup.Modify | Modify user group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| userGroupId | URL | UUID | Yes |  |
| userGroupName | Body | String | Yes | Name that can identify the user group |
| memberIds | Body | Array | No | List of project member identifiers |
| selectAll | Body | Boolean | No | Whether to include all project members<br/>- Default: `false` |

<details><summary>Example</summary>
<p>

```json
{
    "userGroupName": "userGroupName-example",
    "memberIds": [],
    "selectAll": false
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

## Notification Groups

### View Notification Group List

```http
GET /v4.0/notification-groups
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:NotificationGroup.List | View notification group list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| notificationGroups | Body | Array | Notification group list |
| notificationGroups.notificationGroupId | Body | UUID | Notification group identifier |
| notificationGroups.notificationGroupName | Body | String | Name that can identify the notification group |
| notificationGroups.notifyEmail | Body | Boolean | Whether to send email notifications |
| notificationGroups.notifySms | Body | Boolean | Whether to send SMS notifications |
| notificationGroups.isEnabled | Body | Boolean | Whether enabled |
| notificationGroups.createdYmdt | Body | DateTime | Creation date and time |
| notificationGroups.updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "notificationGroups": [
        {
            "notificationGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "notificationGroupName": "notificationGroupName-example",
            "notifyEmail": false,
            "notifySms": false,
            "isEnabled": false,
            "createdYmdt": "2023-12-31T15:00:00+09:00",
            "updatedYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create Notification Group

```http
POST /v4.0/notification-groups
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:NotificationGroup.Create | Create notification group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| notificationGroupName | Body | String | O | Name that can identify the notification group<br/>- Minimum length: `1`<br/>- Maximum length: `100` |
| notifyEmail | Body | Boolean | X | Whether to send email notifications<br/>- Default: `true` |
| notifySms | Body | Boolean | X | Whether to send SMS notifications<br/>- Default: `true` |
| isEnabled | Body | Boolean | X | Whether enabled<br/>- Default: `true` |
| dbInstanceIds | Body | Array | O | List of identifiers for DB instances to monitor |
| userGroupIds | Body | Array | O | List of user group identifiers |

<details><summary>Example</summary>
<p>

```json
{
    "notificationGroupName": "notificationGroupName",
    "notifyEmail": true,
    "notifySms": true,
    "isEnabled": true,
    "dbInstanceIds": [],
    "userGroupIds": []
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| notificationGroupId | Body | UUID | Notification group identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "notificationGroupId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete Notification Group

```http
DELETE /v4.0/notification-groups/{notificationGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:NotificationGroup.Delete | Delete notification group |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| notificationGroupId | URL | UUID | O |  |

#### Response

This API does not return a response body.

---

### View Notification Group Details

```http
GET /v4.0/notification-groups/{notificationGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:NotificationGroup.Get | View notification group details |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| notificationGroupId | URL | UUID | O |  |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| notificationGroupId | Body | UUID | Notification group identifier |
| notificationGroupName | Body | String | Name that can identify the notification group |
| notifyEmail | Body | Boolean | Whether to send email notifications |
| notifySms | Body | Boolean | Whether to send SMS notifications |
| isEnabled | Body | Boolean | Whether enabled |
| dbInstances | Body | Array | List of DB instances to monitor |
| dbInstances.dbInstanceId | Body | UUID | DB instance identifier |
| dbInstances.dbInstanceName | Body | String | Name that can identify the DB instance |
| userGroups | Body | Array | User group list |
| userGroups.userGroupId | Body | UUID | User group identifier |
| userGroups.userGroupName | Body | String | Name that can identify the user group |
| createdYmdt | Body | DateTime | Creation date and time |
| updatedYmdt | Body | DateTime | Modification date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "notificationGroupId": "550e8400-e29b-41d4-a716-446655440000",
    "notificationGroupName": "notificationGroupName-example",
    "notifyEmail": false,
    "notifySms": false,
    "isEnabled": false,
    "dbInstances": [
        {
            "dbInstanceId": "550e8400-e29b-41d4-a716-446655440000",
            "dbInstanceName": "dbInstanceName-example"
        }
    ],
    "userGroups": [
        {
            "userGroupId": "550e8400-e29b-41d4-a716-446655440000",
            "userGroupName": "userGroupName-example"
        }
    ],
    "createdYmdt": "2023-12-31T15:00:00+09:00",
    "updatedYmdt": "2023-12-31T15:00:00+09:00"
}
```

</p>
</details>

---

### Modify Notification Group

```http
PUT /v4.0/notification-groups/{notificationGroupId}
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:NotificationGroup.Modify | Modify notification group |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| notificationGroupId | URL | UUID | O |  |
| notificationGroupName | Body | String | X | Name that can identify the notification group |
| notifyEmail | Body | Boolean | X | Whether to send email notifications<br/>- Default: `false` |
| notifySms | Body | Boolean | X | Whether to send SMS notifications<br/>- Default: `false` |
| isEnabled | Body | Boolean | X | Whether enabled<br/>- Default: `false` |
| dbInstanceIds | Body | Array | X | List of identifiers for DB instances to monitor |
| userGroupIds | Body | Array | X | List of user group identifiers |

<details><summary>Example</summary>
<p>

```json
{
    "notificationGroupName": "notificationGroupName-example",
    "notifyEmail": false,
    "notifySms": false,
    "isEnabled": false,
    "dbInstanceIds": [],
    "userGroupIds": []
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

## Monitoring

### View Statistical Information

```http
GET /v4.0/metric-statistics
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Metric.List | View statistical information |

#### Request

This API does not require a request body.

#### Response

This API does not return a response body.

---

### View Metric List

```http
GET /v4.0/metrics
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Metric.List | View metric list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| metrics | Body | Array | Metric list |
| metrics.measureName | Body | String | Query metric type |
| metrics.unit | Body | String | Measurement unit |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "metrics": [
        {
            "measureName": "measureName-example",
            "unit": "unit-example"
        }
    ]
}
```

</p>
</details>

---

## Events

### Event Categories

Events can be categorized as follows:

| Event Category | Description |
|-------------|---------|
| ALL         | All      |
| BACKUP      | Backup      |
| DB_INSTANCE | DB Instance |
| JOB         | Job      |
| TENANT      | Tenant     |
| MONITORING  | Monitoring    |

### View Subscribable Event Codes List

```http
GET /v4.0/event-codes
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Event.List | View subscribable event codes list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| eventCodes | Body | Array | Event codes list |
| eventCodes.eventCode | Body | Enum | Event code |
| eventCodes.eventCategoryType | Body | Enum | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "eventCodes": [
        {
            "eventCode": "ENUM_VALUE",
            "eventCategoryType": "ALL"
        }
    ]
}
```

</p>
</details>

---

### View Event List

```http
GET /v4.0/events
```

#### Required Permissions

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:Event.List | View event list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Total number of event list |
| events | Body | Array | Event list |
| events.eventCategoryType | Body | Enum | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| events.eventCode | Body | Enum | Type of event that occurred |
| events.sourceId | Body | UUID | Event source identifier |
| events.sourceName | Body | String | Name that can identify the event source |
| events.messages | Body | Array | Event message list |
| events.messages.langCode | Body | Enum | Language code<br/>- KO<br/>- EN<br/>- JA<br/>- ZH |
| events.messages.message | Body | String | Event message |
| events.eventYmdt | Body | DateTime | Event occurrence date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "events": [
        {
            "eventCategoryType": "ALL",
            "eventCode": "ENUM_VALUE",
            "sourceId": "550e8400-e29b-41d4-a716-446655440000",
            "sourceName": "sourceName-example",
            "messages": [
                {
                    "langCode": "KO",
                    "message": "message-example"
                }
            ],
            "eventYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

## Event Subscription

### Get Event Subscription List

```http
GET /v4.0/event-subscriptions
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:EventSubscription.List | Get event subscription list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| totalCounts | Body | Number | Total number of event subscription lists |
| eventSubscriptions | Body | Array | Event subscription list |
| eventSubscriptions.eventSubscriptionId | Body | UUID | Event subscription identifier |
| eventSubscriptions.eventCategoryType | Body | Enum | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| eventSubscriptions.eventSubscriptionName | Body | String | Identifiable name of event subscription |
| eventSubscriptions.enabled | Body | Boolean | Whether enabled |
| eventSubscriptions.notifyEmail | Body | Boolean | Whether to send email |
| eventSubscriptions.notifySms | Body | Boolean | Whether to send SMS |
| eventSubscriptions.eventCodes | Body | Array | List of event codes to subscribe |
| eventSubscriptions.sources | Body | Array | List of event sources to subscribe |
| eventSubscriptions.sources.sourceId | Body | UUID | Event source identifier |
| eventSubscriptions.sources.eventCategoryType | Body | Enum | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| eventSubscriptions.userGroupIds | Body | Array | List of user group identifiers subscribed to events |
| eventSubscriptions.createdYmdt | Body | DateTime | Creation date and time |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "totalCounts": 1,
    "eventSubscriptions": [
        {
            "eventSubscriptionId": "550e8400-e29b-41d4-a716-446655440000",
            "eventCategoryType": "ALL",
            "eventSubscriptionName": "eventSubscriptionName-example",
            "enabled": false,
            "notifyEmail": false,
            "notifySms": false,
            "eventCodes": [],
            "sources": [
                {
                    "sourceId": "550e8400-e29b-41d4-a716-446655440000",
                    "eventCategoryType": "ALL"
                }
            ],
            "userGroupIds": [
                "550e8400-e29b-41d4-a716-446655440000"
            ],
            "createdYmdt": "2023-12-31T15:00:00+09:00"
        }
    ]
}
```

</p>
</details>

---

### Create Event Subscription

```http
POST /v4.0/event-subscriptions
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:EventSubscription.Create | Create event subscription |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| eventCategoryType | Body | Enum | O | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| eventSubscriptionName | Body | String | O | Identifiable name for event subscription |
| enabled | Body | Boolean | O | Whether enabled |
| notifyEmail | Body | Boolean | O | Whether to send email |
| notifySms | Body | Boolean | O | Whether to send SMS |
| eventCodes | Body | Array | O | List of event codes to subscribe |
| sources | Body | Array | O | List of event sources to subscribe |
| sources.sourceId | Body | UUID | O | Event source identifier |
| sources.eventCategoryType | Body | Enum | O | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| userGroupIds | Body | Array | O | List of user group identifiers to subscribe to events |

<details><summary>Example</summary>
<p>

```json
{
    "eventCategoryType": "ALL",
    "eventSubscriptionName": "eventSubscriptionName-example",
    "enabled": false,
    "notifyEmail": false,
    "notifySms": false,
    "eventCodes": [],
    "sources": [
        {
            "sourceId": "550e8400-e29b-41d4-a716-446655440000",
            "eventCategoryType": "ALL"
        }
    ],
    "userGroupIds": []
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| eventSubscriptionId | Body | UUID | Event subscription identifier |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "eventSubscriptionId": "550e8400-e29b-41d4-a716-446655440000"
}
```

</p>
</details>

---

### Delete Event Subscription

```http
DELETE /v4.0/event-subscriptions/{eventSubscriptionId}
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:EventSubscription.Delete | Delete event subscription |

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| eventSubscriptionId | URL | UUID | O |  |

#### Response

This API does not return a response body.

---

### Modify Event Subscription

```http
PUT /v4.0/event-subscriptions/{eventSubscriptionId}
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:EventSubscription.Modify | Modify event subscription |

#### Request

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| eventSubscriptionId | URL | UUID | O |  |
| eventCategoryType | Body | Enum | X | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| eventSubscriptionName | Body | String | X | Identifiable name for event subscription |
| enabled | Body | Boolean | X | Whether enabled |
| notifyEmail | Body | Boolean | X | Whether to send email |
| notifySms | Body | Boolean | X | Whether to send SMS |
| eventCodes | Body | Array | X | List of event codes to subscribe |
| sources | Body | Array | X | List of event sources to subscribe |
| sources.sourceId | Body | UUID | O | Event source identifier |
| sources.eventCategoryType | Body | Enum | O | Event category type<br/>- ALL<br/>- INSTANCE<br/>- DB_SECURITY_GROUP<br/>- MONITORING<br/>- JOB<br/>- BACKUP<br/>- TENANT |
| userGroupIds | Body | Array | X | List of user group identifiers to subscribe to events |

<details><summary>Example</summary>
<p>

```json
{
    "eventCategoryType": "ALL",
    "eventSubscriptionName": "eventSubscriptionName-example",
    "enabled": false,
    "notifyEmail": false,
    "notifySms": false,
    "eventCodes": [],
    "sources": [
        {
            "sourceId": "550e8400-e29b-41d4-a716-446655440000",
            "eventCategoryType": "ALL"
        }
    ],
    "userGroupIds": []
}
```

</p>
</details>

#### Response

This API does not return a response body.

---

## Availability Zone

### View Availability Zone List

```http
GET /v4.0/availability-zones
```

#### Required Permission

| Permission Name | Description |
|-----|-----|
| RDSforMariaDB:AvailabilityZone.List | View availability zone list |

#### Request

This API does not require a request body.

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| availabilityZones | Body | Array | Availability zone list |
| availabilityZones.availabilityZoneName | Body | String | Availability zone name |
| availabilityZones.zoneState | Body | Object | Availability zone state |
| availabilityZones.zoneState.available | Body | Boolean | Whether the availability zone is available |

<details><summary>Example</summary>
<p>

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "availabilityZones": [
        {
            "availabilityZoneName": "availabilityZoneName-example",
            "zoneState": {
                "available": false
            }
        }
    ]
}
```

</p>
</details>

---