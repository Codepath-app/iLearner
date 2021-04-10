# Wee-learn

## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)

## Overview
### Description
[A central data centre for secondary schools in third world countries. Over the past year, students regular schedule were disrupted and as such most of them have no form of organization for their studies anymore. With I-Learn, young learners will be given the opportunity to have all their academic contents in one place - making exam preparation simplified with a strong support system.]

### App Evaluation
[Evaluation of your app across the following attributes]
- **Category:**
    - Educational Social Media App
- **Mobile:**
    - For logging in/out and viewing feed timeline
- **Story:**
    - Allow users to view and download posted content
- **Market:**
    - .edu enrolled students have access to log into the app 
- **Habit:**
    - user can view all content available at any time 
- **Scope:**
    - Students and teachers can test the app as long as they have an active .edu email address
    - Schools may define their own scope for the interaction of students within the app
    - Teachers have the functionality to chose what times students can interact with each other in the app

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

**Required Must-have Stories**

* login
* profile view
* view posts
* submit posts
* access external links
* logout
* ...

**Optional Nice-to-have Stories**

* Ability to seperate and limit student access to posted content by students to only instructors they are registered with
* editing content**
* Get notified when new content is posted
* ...

### 2. Screen Archetypes

* [list first screen here]
* Login
   * user should be able to login/register
 
* [list second screen here]
* home screen
   * user should be able to have a main content screen 
  
* [list third screen here]
* logout
   * user should be
   * ...

### 3. Navigation

**Tab Navigation** (Tab to Screen)
* timeline stream
* course selection stream
* user profile
* chat groups

**Flow Navigation** (Screen to Screen)

* [login screen]
   * [Signing in to the application takes you to the second screen]
   * ...
   
* [Timeline Screen]
  * this screen shows all the lates updates made by an instructor; clicking on any timeline elements will take user directly to that update. A special navigation button on the screen is also available to take users to the third screen]
  
* [Course selection Screen]
   * On this screen, a list of tabs will be displayed relative to the    
     courses the student is enrolled in]
   * Clicking on either tab will bring students to a third page where 
     content relative to the subject will be displayed]
   * ...
* [User profile screen]
    * logged in user should be able to see all personal data here and have the ability to update it. 
   * ...

## Wireframes

<img src="https://github.com/Codepath-app/iLearner/blob/main/IMG-20210403-WA0008.jpg">
        

### [BONUS] Digital Wireframes & Mockups

<img src ="https://github.com/Codepath-app/iLearner/blob/main/Flowchart%20(1).jpg">

### [BONUS] Interactive Prototype


## Schema 

### Models

#### User/Instructor

    | Property      | Type     | Description |
    | ------------- | -------- | ------------|
    | email         | String   | unique id for the user post (default field) |
    | first_name    | String   | first name of user |
    | last_name     | String   | last name of user  |
    
#### User/Student

    | Property      | Type     | Description |
    | ------------- | -------- | ------------|
    | email         | String   | unique id for the user post (default field) |
    | first_name    | String   | first name of user |
    | last_name     | String   | last name of user  |
    
 #### Course

    | Property      | Type     | Description |
    | ------------- | -------- | ------------|
    | name          | String   | name of course |
    | code          | String   | course leve code |
    | instructor    | Pointer  | reference to instructor  |
    | schedule      | Map      | day and time of class  |
   
### Networking

#### List of network requests by screen

   -  Api rules
      - (Access Control) This allows anybody to read and post from the database. No authentications required for now.
      ```json
        rules_version = '2';
        service cloud.firestore {
          match /databases/{database}/documents {
             match /{document=**} {
               allow read, write: if
                  request.time < timestamp.date(2021, 5, 6);
                }
            }
        }
      ```
   -  Instructor Class 
  
      - (Read/GET) This is the call to the api to retrieve data from firebasefirestore real-time database (instructor data only)
         ```java
        public LinkedList<Map<String, Object>> read()
        {
        //This the api call made to the user collection on the cloud real-time database
        db.collection("user").document("WoW4mXdA8bBB0SHAgwdW").collection("instructor")
                .get()
                .addOnCompleteListener(task -> {
                    LinkedList<Map<String,Object> > list = new LinkedList<>();
                    if (task.isSuccessful()) {
                        //This for loop is used to retrieve data. Instead of getting the whole chunk of data
                        //we can get indivual snapshots at a time and manipulate it as we please.
                        for (QueryDocumentSnapshot document : Objects.requireNonNull(task.getResult())) {
                            //This log statement is there to help us debug.
                            //document.getdata() is the key function that returns a map instance of the information stored on the cloud
                            Log.d(TAG, document.getId() + " => " + document.getData().toString());
                            //while the loop is running and retrieving data, we are pushing each instance into a local linked list
                            list.add(document.getData()); } }
                    else {
                        Log.w(TAG, "Error getting documents.", task.getException()); }
                    //This is there for debuggin purposes.
                    //We basically coping the local linked list into a global element.
                    instructors.addAll(list); });
                //The global linkedlist is then returned
         return instructors;

        }
         ```
      - (Create/POST) Create a new instructor
        ```java
            public void write(Instructor inst) {
                // Create a new instructor
                Map<String, String> instructor = new HashMap<>();
                instructor.put("first_name", inst.getFirst_name());
                instructor.put("last_name", inst.getLast_name());
                instructor.put("email", inst.getEmail());
                instructor.put("profile_picture", inst.getProfile_picture());
                // Add a new document with a generated ID
                db.collection("user").document("WoW4mXdA8bBB0SHAgwdW").collection("instructor")
                    .add(instructor) //this is the point where we add the map to the database
                    .addOnSuccessListener(new OnSuccessListener<DocumentReference>() {                    
                        @Override
                        public void onSuccess(DocumentReference documentReference) {
                            Log.d(TAG, "DocumentSnapshot added with ID: " + documentReference.getId()); }
                    })
                    .addOnFailureListener(new OnFailureListener() {
                        @Override
                        public void onFailure(@NonNull Exception e) {
                            Log.w(TAG, "Error adding document", e);
                        }
                    });
                }
        ```
#### [OPTIONAL:] Existing API Endpoints
##### Google Firebase
- Base URL - [https://console.firebase.google.com](https://console.firebase.google.com)

##### Project Repo url
- [https://github.com/Codepath-app/WeeLearn](https://github.com/Codepath-app/WeeLearn)

#### Issues
- [https://github.com/Codepath-app/WeeLearn/issues](https://github.com/Codepath-app/WeeLearn/issues)

### Projects
- [https://github.com/Codepath-app/WeeLearn/projects/1](https://github.com/Codepath-app/WeeLearn/projects/1)

### Milestone
- [https://github.com/Codepath-app/WeeLearn/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22The+Looks%22](https://github.com/Codepath-app/WeeLearn/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22The+Looks%22)
- [https://github.com/Codepath-app/WeeLearn/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Api+up+and+running%22](https://github.com/Codepath-app/WeeLearn/issues?q=is%3Aopen+is%3Aissue+milestone%3A%22Api+up+and+running%22)

