# SSIS Package Documentations

## Table of Contents

1. [StaffMonthlyEmailReminderByProtocol](#StaffMonthlyEmailReminderByProtocol)
2. [ReportsAutomation](#ReportsAutomation)
3. [PackagesErrors](#PackagesErrors)
4. [OracleDB-MLDB-Scripts](#OracleDB-MLDB-Scripts)
5. [NewProtocolEmailNotification](#NewProtocolEmailNotification)
6. [EmailNotification-Username_NetID_Discrepancy](#EmailNotification-Username_NetID_Discrepancy)
7. [EmailNotification-Studies_UAHS_RA_SIGNOFF](#EmailNotification-Studies_UAHS_RA_SIGNOFF)
8. [EmailNotification-Studies_IRB_Expiring_Soon](#EmailNotification-Studies_IRB_Expiring_Soon)
9. [EmailNotification-SRCAccrualReport](#EmailNotification-SRCAccrualReport)
10. [EmailNotification-IITStatusUpdate](#EmailNotification-IITStatusUpdate)
11. [EmailNotification-CRF_SRC_Approval](#EmailNotification-CRF_SRC_Approval)
12. [EmailNotification-CalendarNeedsReleased](#EmailNotification-CalendarNeedsReleased)
13. [CalendarValidationByCRCEmailReminder](#CalendarValidationByCRCEmailReminder)

## StaffMonthlyEmailReminderByProtocol

### Basic Information
- **Package Name:** `StaffMonthlyEmailReminderByProtocol`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2023-11-06`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends monthly email reminders to staff members by protocol, including accrual snapshots and staff role details.

### Description
The `StaffMonthlyEmailReminderByProtocol` SSIS package generates and sends monthly email reminders to staff members by protocol. The email includes details like protocol numbers, staff roles, and accrual snapshots to ensure compliance.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[TransformProtocols]`
  - **Query:** Extracts staff and protocol details using an SQL command.

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `DFT StaffMonthlyEmailReminderByProtocol`
  - Extracts data from `[TransformProtocols]` and stores it in an ADO recordset.

- **Script Component:** `Send Email`
  - Uses a C# script to build and send emails via SMTP.

### Parameters and Configurations
1. **Package Parameters:**
   - **`DBConnectionString`:** `Data Source=COM-ONCORE-SQL;Initial Catalog=OnCoreStaging;Integrated Security=SSPI`
   - **`SMTPClientFromURL`:** `TicketCat-NoReply@arizona.edu`
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL

### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`EmailRecord` Class:** Constructs and formats the email messages using protocol information.
- **`SmtpClient` Class:** Sends the emails via SMTP.

### Known Issues
- None documented.

### Schedule
- `8AM, First Monday of every Month`


## ReportsAutomation

### Basic Information
- **Package Name:** `ReportsAutomation`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-04`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Automates the extraction of data from various sources to generate and send email reports based on predefined queries.

### Description
The `ReportsAutomation` package is designed to automate the data extraction process from various sources and generate email reports based on predefined queries. The reports are configured in an ADO recordset and sent via email.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[config].[reports_mail]`
  - **Query:** Extracts data from `protocols` and other tables.

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::config`

### Control Flow
- **Data Flow Task:** `Prepare Config Object`
  - Extracts data from `[config].[reports_mail]` and stores it in an ADO recordset.
- **Script Component:** `Script Task 2`
  - Uses a C# script to process the recordset and generate email reports.

### Parameters and Configurations
1. **Package Parameters:**
   - **`User::config`:** The recordset containing the report configurations.
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL

### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`DbService` Class:** Executes SQL queries on the OnCore SQL database.
- **`EmailService` Class:** Sends emails via SMTP using the specified credentials.

### Known Issues
- None documented.

### Schedule
- `9:30AM, Daily`


## PackagesErrors

### Basic Information
- **Package Name:** `PackagesErrors`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications when errors occur in packages within the OnCore BI system.

### Description
This SSIS package sends email notifications to staff members regarding errors that occur in specific packages within the OnCore BI system. The emails contain details about the package, error messages, and execution time.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[SSISDB].[catalog].[operations]`
  - **Query:** Extracts operation and error data using SQL commands.

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::PackagesErrors`

### Control Flow
- **Data Flow Task:** `ErrorPackage`
  - Extracts data from `[SSISDB].[catalog].[operations]` and other catalog views to identify errors.
- **Union All:** Combines rows from different data flows to create a consolidated error report.
- **Script Component:** `Script Task`
  - Uses a C# script to send emails via SMTP.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
   - **`PackagesErrors`:** Recordset variable containing email data.
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL

### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages.
- **`SmtpClient` Class:** Sends emails via SMTP using the specified connection string.

### Known Issues
- None documented.

### Schedule
- `11AM, Daily`


## OracleDB-MLDB-Scripts

### Basic Information
- **Package Name:** `OracleDB-MLDB-Scripts`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2023-12-04`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Executes stored procedures that update custom tables, like protocols in the OnCore Oracle database under the `utils` schema.

### Description
This SSIS package executes several stored procedures that update custom tables, including protocol data in the OnCore Oracle database within the `utils` schema.

### Source and Destination Overview
- **Source(s):**  
  - Executes stored procedures directly on the OnCore Oracle database via SQL commands.

### Control Flow
- **Execute SQL Tasks:**
  - **`runspCreate_TB_Oncore_DT4`:** Disabled task to create a table using the `CREATE_TB_ONCORE_DT4` procedure.
  - **`runspUpdate_Protocols`:** Executes the `UPDATE_PROTOCOLS` procedure.
  - **`runspUPDATE_PROTOCOLS_UNQIUE_INFO`:** Executes the `UPDATE_PROTOCOLS_UNIQUE_INFO` procedure.
  - **`runspUpdate_Protocol_Subjects_Info`:** Disabled task to update protocol subjects using `UPDATE_PROTOCOL_SUBJECTS_INFO`.

- **Precedence Constraints:**
  - **`runspUpdate_Protocols` -> `runspUPDATE_PROTOCOLS_UNQIUE_INFO`:** Ensures `UPDATE_PROTOCOLS` runs before `UPDATE_PROTOCOLS_UNIQUE_INFO`.
  - **`runspUPDATE_PROTOCOLS_UNQIUE_INFO` -> `runspUpdate_Protocol_Subjects_Info`:** Ensures `UPDATE_PROTOCOLS_UNIQUE_INFO` runs before `UPDATE_PROTOCOL_SUBJECTS_INFO`.

### Parameters and Configurations
1. **Connection Managers**
 - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Known Issues
- None documented.

### Schedule
- `6AM, Daily`


## NewProtocolEmailNotification

### Basic Information
- **Package Name:** `NewProtocolEmailNotification`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2023-11-06`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications when a new protocol is entered into the system.

### Description
This SSIS package sends email notifications to the relevant staff members when a new protocol is entered into the system, providing key protocol information and ensuring appropriate staff sign-offs.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_NewProtocol]`
  - **Query:** `SELECT * FROM NewProtocolEmailNotification`

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::ListOfEmails`

### Control Flow
- **Data Flow Task:** `Prod`
  - Extracts data from `[automation].[EmailNotification_NewProtocol]` and stores it in an ADO recordset.

- **Script Component:** `Send Email`
  - Uses a custom C# script to send emails via SMTP, using data from the recordset.

### Parameters and Configurations
1. **Package Parameters:**
   - **`SMTPClientFromURL`:** `TicketCat-NoReply@arizona.edu`
   - **`ListOfEmails`:** Recordset containing email data.
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL

### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`EmailRecord` Class:** Builds the email messages using the protocol information.
- **`SmtpClient` Class:** Sends emails via SMTP using the specified connection string.

### Known Issues
- None documented.

### Schedule
- `7AM, Daily`


## EmailNotification-Username_NetID_Discrepancy

### Basic Information
- **Package Name:** `EmailNotification-Username_NetID_Discrepancy`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications to staff members regarding discrepancies between usernames and NetIDs.

### Description
This SSIS package sends email notifications to staff members to notify them of discrepancies between their usernames and NetIDs.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_Username_NetID_Discrepancy]`

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `Prepare Data`
  - Extracts data from `[automation].[EmailNotification_Username_NetID_Discrepancy]` using an OLE DB Source and stores it in an in-memory ADO recordset.

- **Script Component:** `Send Email`
  - Uses a custom C# script to send emails via SMTP, reading data from the recordset.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
   - **`MailObject`:** Variable for the recordset containing email data
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages.
- **`SmtpClient` Class:** Sends emails via SMTP using the specified connection string.

### Known Issues
- None documented.

### Schedule
- `One time`


## EmailNotification-Studies_UAHS_RA_SIGNOFF

### Basic Information
- **Package Name:** `EmailNotification-Studies_UAHS_RA_SIGNOFF`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications to remind relevant staff members to sign off on studies conducted under UAHS RA.

### Description
This SSIS package sends email notifications to relevant staff reminding them to sign off on studies under UAHS RA.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_Studies_UAHS_RA_SIGNOFF]`

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `runUAHS_RA_SIGNOFF_10`
  - Extracts data from `[automation].[EmailNotification_Studies_UAHS_RA_SIGNOFF]` to populate an in-memory ADO recordset.

- **Script Component:** `Send Email`
  - Uses a C# script to send emails via SMTP using data from the recordset.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
   - **`MailObject`:** Variable for the recordset containing email data
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages.
- **`SmtpClient` Class:** Sends the emails via SMTP using the specified connection string.

### Known Issues
- None documented.

### Schedule
- `Termindated`


## EmailNotification-Studies_IRB_Expiring_Soon

### Basic Information
- **Package Name:** `EmailNotification-Studies_IRB_Expiring_Soon`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications to relevant staff when studies are nearing their IRB expiration date.

### Description
This SSIS package sends email notifications to staff members when studies are approaching their Institutional Review Board (IRB) expiration dates.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_Studies_IRB_Expiring_Soon]`
  - **Columns:** `To`, `From`, `BCC`, `CC`, `PROTOCOL_NO`, `IRB_EXPIRATION_DATE`, `Subject`, and `Body`.

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `Prepare Data`
  - Extracts data from `[automation].[EmailNotification_Studies_IRB_Expiring_Soon]` using an OLE DB source and stores it in a recordset.

- **Script Component:** `Send Email`
  - Reads from the recordset to send emails using a custom C# script.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
   - **`MailObject`:** Variable for the recordset containing email data
2. **Project Parameters:**
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages.
- **`SmtpClient` Class:** Sends emails via SMTP using the provided connection string.

### Known Issues
- None documented.

### Schedule
- `Not Active yet`


## EmailNotification-SRCAccrualReport

### Basic Information
- **Package Name:** `EmailNotification-SRCAccrualReport`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications to relevant staff with accrual reports from the Study Review Committee (SRC).

### Description
This SSIS package sends email notifications containing accrual reports from the Study Review Committee (SRC) to relevant staff members.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification-SRCAccrualReport]`

- **Destination(s):**
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `runExecuteSpoolSRCAccrualRpt`
  - Extracts data from `[automation].[EmailNotification-SRCAccrualReport]` to populate an in-memory ADO recordset.

- **Script Component:** `Send Email`
  - Custom C# script task sends emails via SMTP using the ADO recordset data.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
   - **`MailObject`:** Variable for the recordset containing email data
2. **Project Parameters:** (if applicable)
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages.
- **`SmtpClient` Class:** Sends emails via SMTP with the specified credentials.

### Known Issues
- None documented.

### Schedule
- `9:30AM, First day of every month`


## EmailNotification-IITStatusUpdate

### Basic Information
- **Package Name:** `EmailNotification-IITStatusUpdate`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications to provide status updates for Investigator-Initiated Trials (IIT).

### Description
This SSIS package sends email notifications containing status updates for Investigator-Initiated Trials (IIT) to relevant staff members. It retrieves email data from the `automation` schema.

### Source and Destination Overview
- **Source(s):**  
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_IITStatusUpdate]`

- **Destination(s):**  
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `runIIT_STATUS_UPDATE`
  - Extracts data from `[automation].[EmailNotification_IITStatusUpdate]` and stores it in an ADO recordset.

- **Script Component:** `Send Email`
  - Reads the data from the recordset and uses a custom C# script to send emails via SMTP.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
2. **Project Parameters:** (if applicable)
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages.
- **`SmtpClient` Class:** Sends the emails via SMTP.

### Known Issues
- None documented.

### Schedule
- `9:30AM, Daily`


## EmailNotification-CRF_SRC_Approval

### Basic Information
- **Package Name:** `EmailNotification-CRF_SRC_Approval`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications when a case report form (CRF) needs approval from the Study Review Committee (SRC).

### Description
This SSIS package sends email notifications to relevant staff members when a case report form requires approval from the Study Review Committee.

### Source and Destination Overview
- **Source(s):**  
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_CRF_SRC_Approval]`
  - **Query:** The query retrieves relevant email data from the `automation` schema.

- **Destination(s):**  
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `runCRF_SRC_APPROVAL`
  - Extracts data from `[automation].[EmailNotification_CRF_SRC_Approval]` and stores it in an in-memory ADO recordset.
- **Script Component:** `Send Email`
  - This Script Task reads from the recordset and sends emails using the `MailObject` class.
  - The SMTP client uses the `ConnectionString` parameter to connect to the email server.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
2. **Project Parameters:** (if applicable)
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages to be sent.
- **`SmtpClient` Class:** Sends emails through the specified SMTP server.

### Known Issues
- None documented.

### Schedule
- `Not Active yet`


## EmailNotification-CalendarNeedsReleased
### Basic Information
- **Package Name:** `EmailNotification-CalendarNeedsReleased`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2024-03-25`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email notifications to study staff members when a study calendar needs to be released.

### Description
This package is designed to notify study staff members via email when a study calendar needs to be released. The email provides details regarding the calendar status and protocol number.

### Source and Destination Overview
- **Source(s):**
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_CalendarNeedsReleased]`
  - **Query:** `select * from automation.EmailNotification_CalendarNeedsReleased`
- **Destination(s):**  
  - **Type:** Recordset Destination
  - **Variable Name:** `User::MailObject`

### Control Flow
- **Data Flow Task:** `runCALENDAR_NEEDS_RELEASED`
  - Extracts data from `[automation].[EmailNotification_CalendarNeedsReleased]` to populate an in-memory ADO recordset.
- **Script Component:** `Send Email`
  - Uses a custom script to read the recordset data and send emails via SMTP.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
2. **Project Parameters:** (if applicable)
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL


### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`MailMessage` Class:** Constructs and formats the email messages to be sent.
- **`SmtpClient` Class:** Sends emails through the specified SMTP server.

### Known Issues
- None documented.

### Schedule
- `9:45AM, Every Tuesday`

## CalendarValidationByCRCEmailReminder
### Basic Information
- **Package Name:** `CalendarValidationByCRCEmailReminder`
- **Author:** `BLUECAT\gendy`
- **Creation Date:** `2023-11-06`
- **Last Modified Version:** `16.0.5270.0`
- **Purpose:** Sends email reminders to study staff members when a study calendar is released, informing them that it needs to be signed by the CRC.

### Description
This package sends email reminders to study staff members to inform them that the study calendar has been released and requires a CRC signature.

### Source and Destination Overview
- **Source(s):**  
  - **Type:** OLE DB Source
  - **Name:** `[automation].[EmailNotification_CalendarValidationByCRC]`
  - **Query:** `SELECT * FROM automation.EmailNotification_CalendarValidationByCRC`
- **Destination(s):**  
  - Emails are generated and sent using the `EmailService` class.

### Control Flow
- **Data Flow Task:** `DFT CalendarValidationByCRCEmailReminder`
  - Extracts data from `[automation].[EmailNotification_CalendarValidationByCRC]`
- **Script Component:**
  - Processes extracted data and builds emails using the `EmailRecord` class.
- **Error Handling:**
  - Errors redirect to an error output.

### Parameters and Configurations
1. **Package Parameters:**
   - **`MailFrom`:** `TicketCat-NoReply@arizona.edu`
2. **Project Parameters:** (if applicable)
   - **`ConnectionString`:** usb-smtp-outbound-1.mimecast.com
   - **`Login`:** Sensetive
   - **`PW`:** Sensetive
3. **Connection Managers**
   - **`Project Connection Manager`** : COM-ONCORE-SQL

### Logging and Monitoring
- **Logging Provider:** Not explicitly stated.

### Key Classes and Components
- **`EmailRecord` Class:** Structures email messages.
- **`EmailService` Class:** Sends email messages via SMTP.

### Known Issues
- None documented.

### Schedule
- `7AM, Daily`
