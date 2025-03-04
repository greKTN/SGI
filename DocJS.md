# Documentation for JS

##  Folder structure
The JS files are organized with this order:
```
VirtualCLinics/JS/
├── models/
│   ├── dbManager.js
│   └── authManager.js
├── services/
│   └── config.js
├── utils/
│   └── canvasUtils.js
├── components/
|    ├── navabar.js
|    ├── pup-up.js
```
Where:

- `/models`: For classes and data structure.
- `/services`: For API calls and the payments management.
- `/utils`: To save helpfuls features.
- `/components`: For reusable UI components and To save all  the logic of the HTML's

## Table of contents

1. [Models](#models)
    - [dbManager.js](#dbmanager)
    - [authManager.js](#authmanager)
2. [Services](#services)
    - [configFb.js](#configfb)
3. [Utils](#utils)
    - [canvasUtils.js](#canvasutils)
    
4. [Components](#components)
    - [navabar.js](#navbar)
---

## Models
The next content will explain every code that is in the `/models` directory.
-  Explanation the code objectives
- Args
- Description of every each method of the class
- Where this class is used
- Dependices
### dbManager
**Objective:** The `DbManager` class integrates Firebase Realtime Database methods (set, update, remove, get, push) to handle CRUD (`Create`, `Read`, `Update`, `Delete`) operations for users, indications, and templates. It also includes utility methods for verifying users and retrieving user data by email.

---

### **Constructor**
  Adds a new user to the database. The data must be provided in **JSON format**, like this:  

  ```javascript
constructor(auth, app) {
    this.auth = auth;
    this.db = getDatabase(app);
}
  ```

  - **Args**:  
    `auth`: Firebase Authentication instance. 
    `auth`: Firebase App instance.
  - **Attributes**:  
    `this.auth`: Firebase App instance.
    `this.db`: Firebase Realtime Database instance.

---

### **User Methods**
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `writeUserData(data)`  | `data`: A JSON object containing the user's information.   | `true` if the user was successfully added; otherwise, `false`.       |Adds a new user to the database. The data must be provided in **JSON format**|
| `readUserData()`   | None   |Nested JSON object containing all users' information. If no users exist, returns `null`.| Retrieves all users' data from the platform. The returned data is structured as a nested JSON object, where each key corresponds to a user ID.|
| `updateUserData(id, data)`   | `id`: The unique identifier of the user to be updated.<br><br>`data`: A JSON object containing the fields to be updated.  |`true` if the update was successful; otherwise, `false`.| Updates an existing user's data in the database. The data should be provided in **JSON format**, similar to the `writeUserData` method.|
| `deleteUser(id)`   |`id`: The unique identifier of the user to be deleted.|`true` if the deletion was successful; otherwise, `false`.| Monitors whether a user is logged in or logged out and updates the application state accordingly.|
| `getUserData(id)`   |`id`: The unique identifier of the user|`userData` if the search was successful; otherwise, `false` or `null`.| Get all the data from one user by id.|
| `getUserByEmail(email)`   |`id`: The unique identifier of the user|`userData` if the search was successful; otherwise, `null`.| Get all the data from one user by email.|
| `verifyUser(id)`   |`id`: The unique identifier of the user|`true` if the process was successful; otherwise, `false`.| Get all the data from one user by email.|
  
**Recommended use for Write method:**
```javascript
  writeUserData({
      name: "Juan Edgardo Gonzalez Smith",
      card: "V12234567",
      email: "anemail@gmail.com",
      tlf: "+58 401 3478",
      registered: 1740869302, // UNIX timestamp
      type: "default", // Possible values: master, admin/clinic, independent, associated
      position: "neurologist",
      verified: false
  });
  ```
---
### **Indication Methods**
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `writeIndicationData(id,data)`  | `id`:The user Id that is storing the indication<br><br>`data`: A JSON object containing the indication information.   | `true` if the user was successfully added; otherwise, `false`.       |Adds a new indication to the database under a specific user. Each indication is assigned a unique ID automatically.|
| `readIndicationData(id)`   | `id`: The unique identifier of the user whose indications are being retrieved.   |Nested JSON object containing all indications for the specified user. If no indications exist, returns `null`.| Retrieves all indications associated with a specific user.|
| `updateIndicationData(id,indicationid, data)`   | `id`: The unique identifier of the user to whom the indication belongs.<br><br> `indicationid`: The unique identifier of the indication to be updated.<br><br>`data`: A JSON object containing the fields to be updated.  |`true` if the update was successful; otherwise, `false`.| Updates an existing indication's data in the database.|
| `deleteIndication(id, indicationid)`   |`id`: The unique identifier of the user to whom the indication belongs.<br><br>`indicationid`: The unique identifier of the indication to be deleted.|`true` if the deletion was successful; otherwise, `false`.| Deletes a specific indication from the database.|

---
### **Template Methods**
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `writeTemplateData(id,data)`  | `id`: The unique identifier of the user <br><br>`data`: A JSON object containing the template information.   | `true` if the user was successfully added; otherwise, `false`.       |  Adds a new template to the database under a specific user. Each template is assigned a unique ID automatically.|
| `readTemplateData(id)`   | `id`: The unique identifier of the user whose indications are being retrieved.   |Nested JSON object containing all indications for the specified user. If no template exist, returns `null`.| Retrieves all templates associated with a specific user.|
| `updateTemplateData(id, templateid, data)`   | `id`: The unique identifier of the user to whom the template belongs.<br><br>`templateid`: The unique identifier of the template to be updated.<br><br>`data`: A JSON object containing the fields to be updated.  |`true` if the update was successful; otherwise, `false`.| Updates an existing template's data in the database.|
| `deleteTemplate(id, templateid)`   |`id`: The unique identifier of the user to whom the template belongs.<br><br>`templateid`: The unique identifier of the template to be deleted.  |`true` if the deletion was successful; otherwise, `false`.|Deletes a specific template from the database.|


### **Additional Notes**
- **Error Handling**: All methods include basic error handling. If an error occurs, the method logs the error (commented out in the code) and returns `false`.  
- **Database Structure**:  
  - Users are stored under the `users/` path.  
  - Indications are stored under the `indications/{userId}/` path.  
  - Templates are stored under the `templates/{userId}/` path.  
- **Dependencies**: The class relies on Firebase Realtime Database methods (`set`, `get`, `update`, `remove`, etc.) imported from the Firebase SDK.

---


### authManager


The `Auth` class integrates Firebase Authentication methods (`createUserWithEmailAndPassword`, `signInWithEmailAndPassword`, `signOut`, etc.) with the `dbManager` class to handle user registration, login, logout, and account deletion. It also observes authentication state changes and ensures proper user data management.

### **Constructor**
```javascript
constructor(auth, app) {
    this.auth = auth;
    this.db = new dbManager(auth, app);
    this.user = null;
}
```
- **Args**:
  - `auth`: Firebase Authentication instance.
  - `app`: Firebase App instance.
- **Attributes**:
  - `this.auth`: Firebase Authentication instance.
  - `this.db`: Instance of `dbManager` for database operations.
  - `this.user`: Stores the currently authenticated user (if any).

---

## **Methods**
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `registerUser(data, password)`  | `data`: A JSON object containing user details (e.g., name, email, etc.).<br><br>`password`: The password for the new user.   | `true` if the user was successfully registered and their data was saved.<br><br>`false` if an error occurred (e.g., email already in use).       |Registers a new user with email and password, sends a verification email (commented out), and saves user data in the database.|
| `loginUser(email, password)`   | `email`: The user's email.<br><br>`password`: The user's password.   |`true` if the user was successfully logged in.<br><br>`false` if the user's email is unverified or an error occurred.| Authenticates the user via Firebase Authentication and verifies their email status before proceeding.|
| `signOutUser()`   | None|`true` if the user was successfully signed out.<br><br>`false` if an error occurred.| Signs out the user from Firebase Authentication and clears their session data.|
| `onAuthStateChange()`   | None|None| Monitors whether a user is logged in or logged out and updates the application state accordingly.|
| `deleteUser()`   | None|`true` if the user was successfully deleted.<br><br>`false` if an error occurred (e.g., requiring recent login).| Removes the current user's data from the database and deletes their account from Firebase Authentication.|


## Utils
### canvasUtils

The `canvasUtils.js` file contains utility functions related to rendering and manipulating rectangles on a canvas. These functions are primarily used for drawing, moving, and resizing rectangles, as well as handling user interactions with the canvas. The file is part of the `/utils` directory, which is dedicated to reusable helper functions.

---

## Functions
| **Name**         | **Args** | **Return**          | **Description**     |
|--------------|------|--------------|--------------|
| `renderRectangle()`   | None   | None       |Clears the canvas and renders rectangles based on the active rectangle type. This function ensures that both inactive and active rectangles are drawn correctly, with appropriate colors and styles.       |
| `renderMoveHandle()`   | `targetRect`: The rectangle for which the move handle is rendered.   | None       |Renders the move handle (center point) for a given rectangle. The move handle allows users to drag and reposition the rectangle.|
| `renderResizeHandles()`   | `targetRect`: The rectangle for which the resize handles are rendered.|None| Renders resize handles (corners and sides) for a given rectangle. These handles allow users to resize the rectangle by dragging them.|


## Additional Notes

- **Error Handling:**  
  All functions include basic error handling. If an invalid `targetRect` is passed, the function logs an error message and exits gracefully.

- **Dependencies:**  
  The functions in this file rely on the `localStorage` API to save and retrieve rectangle data. They also depend on the canvas rendering context (`ctx`) for drawing operations.

- **Folder Structure:**  
  This file is located in the `/utils` directory, which is dedicated to reusable helper functions.

---

