User Story Card

User Story Name:  
REQ-10 | Participant Removal by Administrator

Description:  
The system must allow the Administrator to remove a participant from the room until the randomization starts.

User Story:  
As an Administrator,  
I want to remove a participant from the room before the randomization starts,  
So that I can maintain the correct list of participants and prevent invalid users from joining the gift exchange.

✅ Acceptance Criteria  
AC1 — The Administrator can remove any participant from the room until the randomization starts  

Given the Administrator is logged into the system and is viewing the list of participants in a specific room  
When the Administrator selects a participant and clicks “Remove” before the randomization starts  
Then the system removes the selected participant from the room and displays a confirmation message  

AC2 — Removed participant loses access to the room and their data is deleted

Given a participant has been removed from the room by the Administrator  
When the removed participant tries to access the room again  
Then the system denies access and permanently deletes all data related to that participant  

AC3 — The system updates the number of participants in the room accordingly  

Given a participant has been successfully removed from the room  
When the Administrator views the participant list or room summary  
Then the system displays the updated total number of participants in the room  

User Flow  
Main Scenario

1. The Administrator logs into the system and navigates to the “Room Management” section.

2. The Administrator selects the room from which a participant needs to be removed.

3. The system loads the list of participants.

    3.1 If there are many participants, the list is loaded page by page.
    
    3.2 If loading fails due to a network issue or database problem  
        (e.g., connection timeout, lost server connection, authorization error, or table lock), the system displays:  
        “Failed to load the list of participants. There may be a connection issue with the server or database. Please try again later.”
        
    3.3 The error is logged in the system journal with the type: NetworkError, DatabaseConnectionError, or QueryTimeout.

5. The Administrator locates the desired participant and clicks the “Remove” button next to their name.

6. The system displays a confirmation dialog:
    “Are you sure you want to remove participant <Name>? This action cannot be undone.”

7. The Administrator confirms the action.

Validation and Processing

7. The system performs checks:

    7.1 Has the randomization process already started?
    
    7.2 Does the participant still exist in the database (they might have been removed earlier by another administrator)?

8. If randomization has not started and the participant exists:

    8.1 The system removes the participant and all related data (wishes, messages, matching pairs).
    
    8.2 The participant counter in the interface is updated.
    
    8.3 The system displays:
        “Participant successfully removed.”

9. If randomization has already started:

    9.1 The system cancels the operation.
    
    9.2 The system displays:
        “Participant removal is not available after randomization has started.”

10. If a technical error occurs during removal (e.g., database timeout, transaction conflict, lost connection, or internal server error):

    10.1 The system displays:
         “An error occurred while removing the participant. Please try again later.”
         
    10.2 The event is logged with an error code and details.

Additional Scenarios

11. If the participant being removed is currently active in the room:

    11.1 Their current session is forcibly terminated.
    
    11.2 Upon re-entry attempt, the message appears:
         “Your access to the room has been revoked by the administrator.”

12. After successful removal, the system records an entry in the audit log, including:

    12.1 Administrator’s name
    
    12.2 Removed participant’s name
    
    12.3 Date and time
    
    12.4 Reason (if specified)

13. The Administrator can continue removing other participants or exit the room.

14. If, after removal, the number of participants falls below the minimum required for randomization (e.g., fewer than two):
    The system displays a warning:
    “There are not enough participants in the room to perform randomization.”
