# Requirements Document

## Introduction

A single-page HTML application that serves as a BJJ (Brazilian Jiu-Jitsu) training timer. The timer displays round and rest countdowns, cycles through rounds automatically, and is fully controlled via a numerical keyboard. It is optimized for large TV displays with a dark background and responsive layout, and provides audio and visual alerts during transitions.

## Glossary

- **Timer_App**: The single-page HTML application that manages and displays BJJ round and rest timers
- **Round_Timer**: The countdown timer that tracks the duration of an active sparring round
- **Rest_Timer**: The countdown timer that tracks the duration of a rest period between rounds
- **Numpad**: The numerical keyboard (0–9, *, and other numpad keys) used to control the Timer_App
- **Cycle**: One complete sequence of a round period followed by a rest period
- **Session**: A series of Cycles that repeats until the user stops the timer
- **Customization_Overlay**: A modal prompt displayed over the main screen for entering new time values
- **Advanced_Menu**: A submenu accessed via the * key that provides additional configuration options
- **Prep_Countdown**: A short countdown displayed before the first round begins
- **Competition_Preset**: A predefined set of round time, rest time, and number of rounds based on IBJJF rules
- **Class_Schedule**: A JSON array of Class_Entry objects representing the weekly class timetable
- **Class_Entry**: A JSON object with properties: dayOfWeek (string), startTime (HH:MM), endTime (HH:MM), title (string), and classType (one of: kids, adult_basics, adult_advanced, marathon_roll, open_mat)
- **Class_Type**: One of five predefined class categories: kids, adult_basics, adult_advanced, marathon_roll, or open_mat
- **Active_Class**: The Class_Entry whose dayOfWeek matches the current day and whose startTime–endTime range includes the current time
- **Timer_Preset**: A set of default round duration, rest duration, and round count values associated with a specific Class_Type
- **Schedule_Display**: A UI section that shows information about the Active_Class or the next upcoming Class_Entry

## Requirements

### Requirement 1: Display Current Time

**User Story:** As a gym user, I want to see the current wall-clock time, so that I can keep track of the actual time during training.

#### Acceptance Criteria

1. THE Timer_App SHALL display the current local time in HH:MM:SS format in the top-right corner of the screen
2. THE Timer_App SHALL update the displayed current time every second

### Requirement 2: Display Round Timer

**User Story:** As a gym user, I want to see a large round countdown timer, so that I can easily read the remaining round time from across the gym.

#### Acceptance Criteria

1. THE Timer_App SHALL display the Round_Timer in MM:SS format in a large, prominent section in the center of the screen
2. THE Timer_App SHALL label the Round_Timer section with the text "ROUND"
3. WHILE the Round_Timer is active, THE Timer_App SHALL count down from the configured round duration to 00:00

### Requirement 3: Display Rest Timer

**User Story:** As a gym user, I want to see a large rest countdown timer below the round timer, so that I can track rest periods between rounds.

#### Acceptance Criteria

1. THE Timer_App SHALL display the Rest_Timer in MM:SS format in a large section below the Round_Timer
2. THE Timer_App SHALL label the Rest_Timer section with the text "REST"
3. WHILE the Rest_Timer is active, THE Timer_App SHALL count down from the configured rest duration to 00:00

### Requirement 4: Start and Stop Timer

**User Story:** As a gym user, I want to start and stop the timer with a single key press, so that I can control training flow without leaving the mat area.

#### Acceptance Criteria

1. WHEN the user presses the 0 key on the Numpad and no timer is running, THE Timer_App SHALL start the Round_Timer countdown
2. WHEN the user presses the 0 key on the Numpad and a timer is running, THE Timer_App SHALL stop the currently active timer and reset the Session
3. WHEN the timer is stopped, THE Timer_App SHALL display the configured round duration on the Round_Timer and the configured rest duration on the Rest_Timer

### Requirement 5: Automatic Round-Rest Cycling

**User Story:** As a gym user, I want the timer to automatically transition between round and rest periods and loop continuously, so that I do not need to manually restart each period.

#### Acceptance Criteria

1. WHEN the Round_Timer reaches 00:00, THE Timer_App SHALL automatically start the Rest_Timer
2. WHEN the Rest_Timer reaches 00:00, THE Timer_App SHALL automatically start the Round_Timer for the next round
3. THE Timer_App SHALL continue cycling between round and rest periods until the user presses the 0 key to stop
4. WHEN a configured number of rounds is set and all rounds are completed, THE Timer_App SHALL stop the Session automatically

### Requirement 6: Customize Round Time

**User Story:** As a gym user, I want to customize the round duration, so that I can adjust training to different formats.

#### Acceptance Criteria

1. WHEN the user presses the 1 key on the Numpad, THE Timer_App SHALL display a Customization_Overlay prompting for minutes
2. WHEN the user submits the minutes value, THE Timer_App SHALL prompt for seconds in the same Customization_Overlay
3. WHEN the user submits the seconds value, THE Timer_App SHALL update the configured round duration to the entered value
4. THE Timer_App SHALL persist the last configured round duration across page reloads using local storage
5. IF the user enters a non-numeric value in the Customization_Overlay, THEN THE Timer_App SHALL display an error message and re-prompt for the value

### Requirement 7: Customize Rest Time

**User Story:** As a gym user, I want to customize the rest duration, so that I can control recovery periods.

#### Acceptance Criteria

1. WHEN the user presses the 2 key on the Numpad, THE Timer_App SHALL display a Customization_Overlay prompting for minutes
2. WHEN the user submits the minutes value, THE Timer_App SHALL prompt for seconds in the same Customization_Overlay
3. WHEN the user submits the seconds value, THE Timer_App SHALL update the configured rest duration to the entered value
4. THE Timer_App SHALL persist the last configured rest duration across page reloads using local storage
5. IF the user enters a non-numeric value in the Customization_Overlay, THEN THE Timer_App SHALL display an error message and re-prompt for the value

### Requirement 8: Default Timer Values

**User Story:** As a gym user, I want sensible default round and rest times, so that I can start training immediately without configuration.

#### Acceptance Criteria

1. THE Timer_App SHALL default the round duration to 5 minutes and 0 seconds when no prior configuration exists
2. THE Timer_App SHALL default the rest duration to 1 minute and 0 seconds when no prior configuration exists
3. WHEN previously configured values exist in local storage, THE Timer_App SHALL load and use those values as the current configuration

### Requirement 9: Audio Alerts

**User Story:** As a gym user, I want audible alerts near the end of each period, so that I am warned before a transition even when not looking at the screen.

#### Acceptance Criteria

1. WHILE the Round_Timer has 5 or fewer seconds remaining, THE Timer_App SHALL play a short beep sound each second
2. WHILE the Rest_Timer has 5 or fewer seconds remaining, THE Timer_App SHALL play a short beep sound each second
3. WHEN the Round_Timer reaches 00:00, THE Timer_App SHALL play a distinct end-of-round beep
4. WHEN the Rest_Timer reaches 00:00, THE Timer_App SHALL play a distinct end-of-rest beep

### Requirement 10: Visual Alerts

**User Story:** As a gym user, I want visual alerts when a period is about to end, so that I can see the transition even in a noisy gym.

#### Acceptance Criteria

1. WHILE the Round_Timer has 5 or fewer seconds remaining, THE Timer_App SHALL flash the Round_Timer display
2. WHILE the Rest_Timer has 5 or fewer seconds remaining, THE Timer_App SHALL flash the Rest_Timer display
3. WHEN the Round_Timer reaches 00:00, THE Timer_App SHALL briefly flash the entire screen
4. WHEN the Rest_Timer reaches 00:00, THE Timer_App SHALL briefly flash the entire screen

### Requirement 11: Help Menu Display

**User Story:** As a gym user, I want to see a help menu on screen, so that I can quickly reference the keyboard controls.

#### Acceptance Criteria

1. THE Timer_App SHALL display a help menu in the bottom-right corner of the screen
2. THE Timer_App SHALL include the following entries in the help menu: "0 → Start/Stop Timer", "1 → Customize Round Time", "2 → Customize Rest Time", "* → Advanced Features"

### Requirement 12: Advanced Features Menu

**User Story:** As a gym user, I want access to advanced configuration options, so that I can set up the timer for specific training scenarios.

#### Acceptance Criteria

1. WHEN the user presses the * key on the Numpad, THE Timer_App SHALL display the Advanced_Menu overlay
2. THE Advanced_Menu SHALL provide an option to set the number of rounds per Session (0 for unlimited)
3. THE Advanced_Menu SHALL provide an option to set a Prep_Countdown duration in seconds before the first round starts
4. THE Advanced_Menu SHALL provide Competition_Preset options based on IBJJF rules (e.g., White Belt 5 min, Blue/Purple Belt 6 min, Brown/Black Belt 8 min with appropriate rest times)
5. WHEN the user selects a Competition_Preset, THE Timer_App SHALL update the round duration, rest duration, and number of rounds to match the selected preset
6. THE Timer_App SHALL persist all Advanced_Menu settings across page reloads using local storage

### Requirement 13: Preparation Countdown

**User Story:** As a gym user, I want a preparation countdown before the first round, so that I have time to get ready after pressing start.

#### Acceptance Criteria

1. WHEN the user starts a Session and a Prep_Countdown duration greater than 0 is configured, THE Timer_App SHALL display and count down the Prep_Countdown before starting the first Round_Timer
2. WHILE the Prep_Countdown is active, THE Timer_App SHALL display the prep countdown value prominently on screen
3. WHEN the Prep_Countdown reaches 0, THE Timer_App SHALL automatically start the first Round_Timer

### Requirement 14: Dark Theme and Responsive Layout

**User Story:** As a gym user, I want the timer to have a dark background with bright text and be readable on a large TV, so that it is easy to see in a gym environment.

#### Acceptance Criteria

1. THE Timer_App SHALL use a dark background color with high-contrast bright text for all timer displays
2. THE Timer_App SHALL use a responsive layout that scales to fill the available screen size
3. THE Timer_App SHALL render the Round_Timer and Rest_Timer text large enough to be readable from a distance on a large TV display
4. THE Timer_App SHALL be optimized for large screen displays while remaining functional on smaller screens

### Requirement 15: Numpad-Only Control

**User Story:** As a gym user, I want to control the entire application using only a numerical keyboard, so that I can use a simple wireless numpad from across the gym.

#### Acceptance Criteria

1. THE Timer_App SHALL accept all control inputs exclusively from Numpad key events
2. THE Timer_App SHALL respond to Numpad key presses regardless of which element has focus on the page
3. WHILE a Customization_Overlay or Advanced_Menu is open, THE Timer_App SHALL route Numpad input to the active overlay for data entry
4. IF an unrecognized Numpad key is pressed, THEN THE Timer_App SHALL ignore the input without side effects

### Requirement 16: Class Schedule Data Model

**User Story:** As a gym owner, I want to define the weekly class schedule as a JSON array, so that the timer app can be configured for each class automatically.

#### Acceptance Criteria

1. THE Timer_App SHALL accept a Class_Schedule as a JSON array of Class_Entry objects
2. THE Timer_App SHALL validate that each Class_Entry contains the following properties: dayOfWeek (string matching a day of the week), startTime (string in HH:MM format), endTime (string in HH:MM format), title (string), and classType (one of: kids, adult_basics, adult_advanced, marathon_roll, open_mat)
3. IF a Class_Entry contains an invalid or missing property, THEN THE Timer_App SHALL reject the entry and log a descriptive validation error to the browser console
4. THE Timer_App SHALL support multiple Class_Entry objects for the same day of the week

### Requirement 17: Current Class Awareness

**User Story:** As a gym user, I want the app to know the current date and time, so that it can determine which class is active or coming up next.

#### Acceptance Criteria

1. THE Timer_App SHALL track the current local date and time and update the Active_Class determination every 30 seconds
2. WHEN the current day and time fall within a Class_Entry startTime–endTime range, THE Timer_App SHALL identify that Class_Entry as the Active_Class
3. WHEN no Class_Entry matches the current day and time, THE Timer_App SHALL identify the next upcoming Class_Entry as the next scheduled class
4. THE Timer_App SHALL display the Active_Class title and classType in the Schedule_Display section of the screen

### Requirement 18: Class Type Timer Presets

**User Story:** As a gym owner, I want each class type to have default timer settings, so that the timer automatically adjusts to the training format of each class.

#### Acceptance Criteria

1. THE Timer_App SHALL define a Timer_Preset for the kids Class_Type with a round duration of 3 minutes, a rest duration of 30 seconds, and unlimited rounds
2. THE Timer_App SHALL define a Timer_Preset for the adult_basics Class_Type with a round duration of 5 minutes, a rest duration of 1 minute, and unlimited rounds
3. THE Timer_App SHALL define a Timer_Preset for the adult_advanced Class_Type with a round duration of 6 minutes, a rest duration of 1 minute, and unlimited rounds
4. THE Timer_App SHALL define a Timer_Preset for the marathon_roll Class_Type with a round duration of 10 minutes, a rest duration of 30 seconds, and unlimited rounds
5. THE Timer_App SHALL define a Timer_Preset for the open_mat Class_Type with a round duration of 5 minutes, a rest duration of 1 minute, and unlimited rounds

### Requirement 19: Automatic Class Detection and Preset Loading

**User Story:** As a gym user, I want the timer to automatically load the correct settings when a scheduled class begins, so that I do not need to configure the timer manually for each class.

#### Acceptance Criteria

1. WHEN the Timer_App detects a new Active_Class, THE Timer_App SHALL automatically load the Timer_Preset associated with the Active_Class classType
2. WHEN the Timer_App loads a Timer_Preset, THE Timer_App SHALL update the configured round duration, rest duration, and number of rounds to match the preset values
3. WHILE a Session is running, THE Timer_App SHALL defer any automatic preset loading until the current Session is stopped
4. WHEN the Active_Class changes and no Session is running, THE Timer_App SHALL apply the new Timer_Preset immediately

### Requirement 20: Schedule Display

**User Story:** As a gym user, I want to see the current or next class information on screen, so that I know what class is happening and what is coming up.

#### Acceptance Criteria

1. THE Timer_App SHALL display a Schedule_Display section on the main screen showing the Active_Class title, classType, and remaining time in the class
2. WHEN no Active_Class exists, THE Timer_App SHALL display the next upcoming Class_Entry title, classType, day, and start time in the Schedule_Display section
3. WHEN no Class_Entry exists in the Class_Schedule, THE Timer_App SHALL display "No classes scheduled" in the Schedule_Display section

### Requirement 21: Schedule Loading from Config File

**User Story:** As a gym owner, I want the app to load the class schedule from a config.json file in the same folder as the HTML page, so that I can update the schedule by editing a single file.

#### Acceptance Criteria

1. WHEN the Timer_App loads, THE Timer_App SHALL fetch the Class_Schedule from a file named config.json located in the same directory as the HTML page using an HTTP GET request
2. THE Timer_App SHALL expect the config.json file to contain a JSON object with a "schedule" property holding the Class_Schedule array
3. IF the config.json file fails to load or contains invalid JSON, THEN THE Timer_App SHALL display a descriptive error message in the Schedule_Display section and fall back to operating without a schedule
4. THE Timer_App SHALL persist the last successfully loaded Class_Schedule in local storage for offline use
5. WHEN the config.json file contains Timer_Preset overrides for any Class_Type, THE Timer_App SHALL use the overridden values instead of the default Timer_Preset values
