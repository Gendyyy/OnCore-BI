

# Table of Contents
* [Email Notifications (Package MLDB-EmailNotifications)](#MLDB-EmailNotifications)
  * [Calendar Needs Released (Data Flow Task runCALENDAR_NEEDS_RELEASED)](#runCALENDAR_NEEDS_RELEASED)
* [New Protocols Created (Package NewProtocolEmailNotification)](#NewProtocolEmailNotification)


## MLDB-EmailNotifications
> 1. Any VM Job that used to send notifications
> 2. All Source queries must include formatted columns to contain [To, CC, BCC, Subject, Body] even if they're empty string
> 3. All destination script components loop over the existing rows and execute send email operation based on the prepared columns
---



#### runCALENDAR_NEEDS_RELEASED 
> It uses the same logic existing in the source VM Job, i didnt modify it.
> 
> Notification of calendars that need to be released.
> 1. Pulls data from UACC_ONCORE_RW_UTILS.EN_PROTOCOL_CALENDAR_STATUS_RO
> 2. Pulls data from Staging.dbo.Staff

#### NewProtocolEmailNotification
> Sending email notifications to any newly created protocols
> 1. Pulling data from Staging.dbo.Protocols
> 2. Pulling data from Staging.dbo.Staff
> 3. Pulling data from OnCoreDW.dw.DimPI
