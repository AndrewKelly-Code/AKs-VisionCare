# AK's VisionCare Version 1.0.0

## 1. Introduction

### 1.1 Purpose
The purpose of this app is to help users who uses computers to build up and maintain good eye health habits, especially for users who would spend multiple hours in front of the computer.

DISCLAIMER: This app is not a medical diagnostic app. If you are concerned about certain symptoms that you are experiencing, you should discuss them with a medical professional. 

This app's main focus is on provided some useful tips on how to reduce certain negative effects on the body that you might experience while using a computer, and helps you enforce those tips on yourself.
### 1.2 Problem Statement
This project came from a personal problem that I experience, which include things like dry eyes, eye strain, nausea, headaches etc... when I'm not careful about my eye health in front of a computer.
### 1.3 Intended User Group
This app is intended for all users who uses a computer, as everyone could benefit from building up or maintain healthy eye and general health habits.
## 2. Project Scope

### 2.1 In Scope
- The app shall measure the time between blinks for the user
- The app shall provide a reminder for the user to blink after a certain amount of time has past since they last blinked
- The app shall use a webcam to be able to measure the time between blinks
- The app shall not store any personal information permanently
- The app shall process all user information locally, without connecting to any external networks
### 2.2 Out of Scope
- The app shall not keep track of user's metrics, such as blink rates etc...
- The app shall not remind users to take breaks
- The app shall not remind the user to drink water or use eye drops
- The app shall not tell the user that they are too close or too far away from the screen
- the app shall not tell the user if the user's environment is too dark or too bright to work in
## 3. Functional Requirements

### FR-01: Camera Access
The program shall establish a connection to the user's camera device and retrieve video frames from the camera when access permission has been granted by the user
### FR-01 Acceptance Criteria:
- AC-01: The program successfully establish a connection with a camera device 
- AC-02: A preview window appears showing a live video feed from the camera's perspective

### FR-02: Camera Access Permission Denied Error Handling
The program shall display an error message, stating that access to the camera was denied, with instructions on how the user can give the program access to the camera and a retry button to reattempt to establish a connection to the camera
### FR-02 Acceptance Criteria:
- AC-01: The program shall remain open when permission to the camera is denied
- AC-02: The program shall display a message stating that it does not have access to a camera
- AC-03: The program shall display a message stating how to grant the program access to a camera
- AC-04: The program shall have a "Retry" button that allows a user to manually reattempt to establish a connection between the program and the camera by clicking on the button
- AC-05: When the "Retry" button is clicked, a visual indicator shall show that an attempt to establish a connection with the camera is in progress
- AC-06: If permission is granted and the program successfully gain access to a camera, the error message shall be dismissed, and normal operation shall resume according to FR-01
- AC-07: If permission isn't granted to the program and the "Retry" button is clicked, the same error message from AC-02 shall reappear and stay displayed

### FR-03: Camera Detection and Selection
The program shall detect camera devices connected to the user's device and provide a menu for the user to select which camera device they would like to use for the program
### FR-03 Acceptance Criteria:
- AC-01: The program shall display a menu button 
- AC-02: Clicking on the menu button shows a list of available camera devices that can be connected to the program
- AC-03: The list of camera devices is provided by the user's operating system
- AC-04: A "Refresh List" button is permanently visible above the menu button
- AC-05: Clicking the "Refresh List" button replaces the list of available camera showed with the list received from the operating system at the time of clicking the refresh button
- AC-06: A permanent message will be displayed underneath the camera selection button that states that the user should check that the camera is connected and configured properly to work with the device in the operating system's settings
- AC-07: If no camera devices are detected, the camera selection menu shall still open but not display any camera devices for selection
- AC-08: The selected camera device shall be used to attempt to establish a connection to the camera device as according to FR-01
- AC-09: The program shall retrieve a list of camera devices connected from the operating system automatically at the time of launching the program each time. This is only done once.

### FR-04: User's Face Detection
After successfully completing FR-01 and establishing a connection with the selected camera, the program shall attempt to detect the user's face

### FR-04 Acceptance Criteria:
- AC-01: The program shall take a video frame from the selected camera
- AC-02: The program shall use the video frame and attempt to locate a face
- AC-03: If it cannot detect a face, the program shall take in another video frame from the selected camera and repeat AC-02
- AC-04: After a face has been successfully detected, the location around the detected face within the frame is marked and supplied to the subsequent landmark identifer component
- AC-05: After successfully completing AC-04, the program shall appempt AC-02 again.


### FR-05: Facial Landmark Identification
After successfully locating a face inside the video frame, as according to FR-04, the program shall identify the landmarks around the eye of the face inside the frame that is to be used to calculate the Eye Aspect Ratio

### FR-05 Acceptance Criteria:
- AC-01: Using the information given from FR-04, the program shall attempt to locate the landmarks around the eye of the face marked inside the frame 
- AC-02: If all the required landmarks around the eyes are successfully identified, the program shall pass on the coordinates of each landmark to the Eye Aspect Ratio score calculation component
- AC-03: If all the required landmarks around the eyes are not successfully identified, the program shall not pass on any coordinates of any landmarks to the Eye Aspect Ratio score calculation component 

### FR-06: Calculating the Eye Aspect Ratio (EAR)
After successfully identifying a full set of coordinates for the required landmarks around the eyes of a face, the program shall use the coordinates to calculate the Eye Aspect Ratio score for each eye independently using the supplied sets of coordinates

### FR-06 Acceptance Criteria:
- AC-01: Using the information given from FR-05, the program shall calculate the Eye Aspect Ratio using the specified equation
- AC-02: If the coordinates provided results is not a valid finite numerical value, the program shall report that the calculation failed to the component requesting the information

### FR-07: Calibrating the Eye Aspect Ratio for the User's Closed and Opened Eye State
After a successful connection has been established to the selected camera, as according to FR-01, the user shall be provided with a sequence to calibrate their Eye Detection Ratio score for their eyes in its natural opened and closed state for both their left and right eye

### FR-07 Acceptance Criteria:
- AC-01: After successfully reaching the camera preview window as according to FR-01, the program shall prompt the user with a message to calibrate their Eye Aspect Ratio before continuing
- AC-02: The program shall display a message to the user and a button that allows the user to begin measuring the EAR score for both their eyes in its opened state
- AC-03: When the user clicks on the button from AC-02, the program shall attempt to measure the average EAR score of both the user's eyes in its opened state, using information from frames captured in a specified period of time
- AC-04: After AC-03 successfully completes, the program shall prompt the user with a message and a button to begin measuring the EAR score for both their eyes in their closed position
- AC-05: When the user clicks on the button from AC-04, the program shall attempt to measure the average EAR score of both the user's eyes in its closed state, using information from frames captured in a specified period of time
- AC-06: The EAR score of the opened and closed states shall be validated if enough frames with all required landmarks presents were captured during the specified time frame, which will check to see if the values captured are within the valid range
- AC-07: If EAR scores are successfully validated, they shall be pass to other components for further use
- AC-08: If the collected measurements passes validation according to AC-07, The program shall provided the option for the user to retry the calibration or to continue using the current calibrations
- AC-09: If the collected measurements fails validation according to AC-07, the program shall display an error message stating that the calibration failed, and provide the user with the option to retry the calibration
- AC-10: If the camera device was disconnected during the calibration process, the program shall display and error message and return the user to the screen specified in FR-03.
- AC-11: If there already exists a set of calibrated values for the selected camera, the user shall skip the calibration sequence

### FR-08:

### FR-08 Acceptance Criteria:
- AC-01:



## 4. Non-Functional Requirements

### NFR-01:

### NFR-01 Acceptance Criteria:

## 5. Assumptions and Constraints