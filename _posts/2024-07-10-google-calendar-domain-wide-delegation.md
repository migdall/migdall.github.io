---
layout: post
author: Jes&uacute;s Noland
title: "Resolving Google Calendar API Issues with Service Accounts and Domain-Wide Delegation"
date: 2024-07-10
categories: [Programming]
---

Working with Google Calendar API can sometimes be tricky, especially when using service accounts to manage events. One common issue is the error encountered when trying to invite attendees using a service account without the proper permissions. In this post, we’ll walk through the steps to resolve the error:

```
<HttpError 403 when requesting https://www.googleapis.com/calendar/v3/calendars/jesusn%40mysholder.com/events?alt=json returned "Service accounts cannot invite attendees without Domain-Wide Delegation of Authority.". Details: "[{'domain': 'calendar', 'reason': 'forbiddenForServiceAccounts', 'message': 'Service accounts cannot invite attendees without Domain-Wide Delegation of Authority.'}]">
```

## Understanding the Problem

The error occurs because service accounts by default do not have the necessary permissions to invite attendees to calendar events. This is a security measure by Google to ensure that service accounts cannot perform actions on behalf of users without explicit authorization.

To fix this, you need to enable Domain-Wide Delegation of Authority (DWD) for your service account and properly configure your code to use these permissions.

## Step-by-Step Solution

### Step 1: Enable Domain-Wide Delegation for Your Service Account

1. Create a Service Account with Domain-Wide Delegation:

-   Go to the Google Cloud Console.
-   Navigate to IAM & Admin > Service accounts.
-   Select your project and create a new service account if you don’t already have one.
-   When creating the service account, check the box for Enable G Suite Domain-wide Delegation.

2. Authorize API Scopes:

-   Go to the Google Admin Console.
-   Navigate to Security > API Controls > Manage Domain Wide Delegation.
-   Click on Add new.
-   Enter the Client ID of your service account.
-   Add the necessary API scopes. For Google Calendar, you might need:

```
https://www.googleapis.com/auth/calendar
https://www.googleapis.com/auth/calendar.events
```

### Step 2: Modify Your Code to Use the Service Account with Proper Scopes

Using google.oauth2.service_account.Credentials, you can configure your code to handle impersonation and create events with attendees. Here's an example function to set up an event:

```python
from google.oauth2.service_account import Credentials
from googleapiclient.discovery import build
import datetime

def set_event(event_datetime, event_name, calendar_id, sharer_email):
    """
    Create a new event in the user's calendar. In order to create an event,
    the service account must have access to the calendar and insert the
    calendar ID to the calendar list.
    """
    SCOPES = ['https://www.googleapis.com/auth/calendar']
    SERVICE_ACCOUNT_KEY_FILE = 'path/to/your/service-account-key.json'

    # Load the service account credentials
    credentials = Credentials.from_service_account_file(
        SERVICE_ACCOUNT_KEY_FILE, scopes=SCOPES)

    # Impersonate a user in your domain
    credentials = credentials.with_subject('foo@bar.com')

    try:
        service = build("calendar", "v3", credentials=credentials)

        end_time = datetime.datetime.fromisoformat(event_datetime) + datetime.timedelta(hours=1)

        # Call the Calendar API
        event = {
            "summary": event_name,
            "start": {"dateTime": event_datetime},
            "end": {"dateTime": end_time.isoformat()},
            "attendees": [{"email": sharer_email}],
        }
        event = service.events().insert(calendarId=calendar_id, body=event).execute()
        print("Event created: %s" % (event.get("htmlLink")))
    except HttpError as error:
        print(f"An error occurred: {error}")
        return None

    return event

```

Make sure you have the necessary packages installed:

```bash
pip install google-auth google-auth-oauthlib google-api-python-client
```

## Conclusion

By enabling Domain-Wide Delegation and properly configuring your service account, you can resolve the 403 error and successfully create calendar events with attendees. This approach ensures that your service account has the necessary permissions to perform actions on behalf of users, making your application more robust and secure.

If you have any questions or run into issues, feel free to reach out. Happy coding!
