---
layout: page
title: Developer Guide
---
* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

## **Setting up, getting started**

Refer to the guide [_Setting up and getting started_](SettingUp.md).

--------------------------------------------------------------------------------------------------------------------

## **Design**

### Architecture

<img src="images/ArchitectureDiagram.png" width="450" />

The ***Architecture Diagram*** given above explains the high-level design of the App. Given below is a quick overview of each component.

<div markdown="span" class="alert alert-primary">

:bulb: **Tip:** The `.puml` files used to create diagrams in this document can be found in the [diagrams](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/docs/diagrams) folder. Refer to the [_PlantUML Tutorial_ at se-edu/guides](https://se-education.org/guides/tutorials/plantUml.html) to learn how to create and edit diagrams.

</div>

**`Main`** has two classes called [`Main`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/java/seedu/address/Main.java) and [`MainApp`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/java/seedu/address/MainApp.java). It is responsible for,
* At app launch: Initializes the components in the correct sequence, and connects them up with each other.
* At shut down: Shuts down the components and invokes cleanup methods where necessary.

[**`Commons`**](#common-classes) represents a collection of classes used by multiple other components.

The rest of the App consists of four components.

* [**`UI`**](#ui-component): The UI of the App.
* [**`Logic`**](#logic-component): The command executor.
* [**`Model`**](#model-component): Holds the data of the App in memory.
* [**`Storage`**](#storage-component): Reads data from, and writes data to, the hard disk.

Each of the four components,

* defines its *API* in an `interface` with the same name as the Component.
* exposes its functionality using a concrete `{Component Name}Manager` class (which implements the corresponding API `interface` mentioned in the previous point.

For example, the `Logic` component (see the class diagram given below) defines its API in the `Logic.java` interface and exposes its functionality using the `LogicManager.java` class which implements the `Logic` interface.

![Class Diagram of the Logic Component](images/LogicClassDiagram.png)

**How the architecture components interact with each other**

The *Sequence Diagram* below shows how the components interact with each other for the scenario where the user issues the command `delete 1`.

<img src="images/ArchitectureSequenceDiagram.png" width="574" />

The sections below give more details of each component.

### UI component

![Structure of the UI Component](images/UiClassDiagram.png)

**API** :
[`Ui.java`](https://github.com/AY2021S1-CS2103T-T13-1/tp/blob/master/src/main/java/seedu/address/ui/Ui.java)

The UI consists of a `MainWindow` that is made up of parts e.g.`CommandBox`, `ResultDisplay`, `PersonListPanel`, `StatusBarFooter` etc. All these, including the `MainWindow`, inherit from the abstract `UiPart` class.

The `UI` component uses JavaFx UI framework. The layout of these UI parts are defined in matching `.fxml` files that are in the `src/main/resources/view` folder. For example, the layout of the [`MainWindow`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/java/seedu/address/ui/MainWindow.java) is specified in [`MainWindow.fxml`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/resources/view/MainWindow.fxml)

The `UI` component,

* Executes user commands using the `Logic` component.
* Listens for changes to `Model` data so that the UI can be updated with the modified data.

### Logic component

![Structure of the Logic Component](images/LogicClassDiagram.png)

**API** :
[`Logic.java`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/java/seedu/address/logic/Logic.java)

1. `Logic` uses the `AddressBookParser` class to parse the user command.
1. This results in a `Command` object which is executed by the `LogicManager`.
1. The command execution can affect the `Model` (e.g. adding a person).
1. The result of the command execution is encapsulated as a `CommandResult` object which is passed back to the `Ui`.
1. In addition, the `CommandResult` object can also instruct the `Ui` to perform certain actions, such as displaying help to the user.

Given below is the Sequence Diagram for interactions within the `Logic` component for the `execute("delete 1")` API call.

![Interactions Inside the Logic Component for the `delete 1` Command](images/DeleteSequenceDiagram.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** The lifeline for `DeleteCommandParser` should end at the destroy marker (X) but due to a limitation of PlantUML, the lifeline reaches the end of diagram.
</div>

### Model component

![Structure of the Model Component](images/ModelClassDiagramNew.png)

**API** : [`Model.java`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/java/seedu/address/model/Model.java)

The `Model`,

* stores a `UserPref` object that represents the user’s preferences.
* stores a `PersonBook`, `LocationBook` and `VisitBook` for the three types of data.
* exposes an unmodifiable `ObservableList` of each type which can be 'observed' e.g. the UI can be bound to this list so that the UI automatically updates when the data in the list change.
* does not depend on any of the other three components.

The `Person`, `Location` and `Visit` components are shown in more detail below.

![Structure of Person and Location components](images/PersonLocationClassDiagram.png)
![Structure of the Visit component](images/VisitClassDiagram.png)


<div markdown="span" class="alert alert-info">:information_source: **Note:** An alternative (arguably, a more OOP) model is given below. It has a `Tag` list in the `AddressBook`, which `Person` references. This allows `AddressBook` to only require one `Tag` object per unique `Tag`, instead of each `Person` needing their own `Tag` object.<br>
![BetterModelClassDiagram](images/BetterModelClassDiagram.png)

</div>


### Storage component

![Structure of the Storage Component](images/StorageClassDiagram.png)

**API** : [`Storage.java`](https://github.com/AY2021S1-CS2103T-T13-1/tp/tree/master/src/main/java/seedu/address/storage/Storage.java)

The `Storage` component,
* can save `UserPref` objects in json format and read it back.
* can save the address book data in json format and read it back.

### Common classes

Classes used by multiple components are in the `seedu.addressbook.commons` package.

--------------------------------------------------------------------------------------------------------------------

## **Implementation**

This section describes some noteworthy details on how certain features are implemented.

### Add person

This is a placeholder section for the "Manage data using CSV files section." Please update the link in that section if the header of this
section is changed.
### Add location

### Add visit

### Manage data using CSV files (Siang Ern)

Most data collected by the target user group are likely to be in the form of Excel documents. As such, it is necessary for VirusTracker to include features to import and export data in a way that is compatible with Excel.

The features are facilitated by the `addFromCsv` and `exportToCsv` commands. They allow VirusTracker to manage data using **Comma-separated values** files which could also be handled by Excel.

#### Importing data from a CSV file

This feature essentially acts as a "bulk add" operation. The number of rows in the CSV file corresponds to the **maximum** number of objects that could be added to VirusTracker.

**Format:** `addFromCsv FILEPATH l/LIST_TYPE`

* `FILEPATH` refers to the absolute path that the file would be located at.
* `LIST_TYPE` is the data type that the user is attempting to add. The `addFromCsv` command supports three list types:
    1. [people](#add-person)
    2. [locations](#add-location)
    3. [visits](#add-visit)
* Each row in the specified CSV file must follow the format for the add command of the respective type. To find out about the format, you may click the relevant list type above.

An example of a CSV file that is used to add people is shown below. Notice that column G is not completely filled as the field is optional.

![SamplePersonCsv](images/ExamplePersonCsv.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** The number of columns in the CSV file should correspond to the number of parameters required to create the related object. Information on additional columns **will be disregarded**.

</div>

#### Sequence diagram

The sequence diagram below shows how the adding operation works. Certain utility classes have been omitted for readability.

![AddFromCsvCommandSequenceDiagram](images/AddFromCsvSequenceDiagram.png)


<div markdown="span" class="alert alert-info">:information_source: **Note:** The above sequence diagram uses the `AddPersonsFromCsvCommand` to handle adding people from CSV files. For locations and visits, replace the command with `AddLocationsFromCsvCommand` and
`AddVisitsFromCsvCommand` respectively. The behaviour of the three commands are the same as the above sequence diagram.

</div>

<div markdown="span" class="alert alert-info">:information_source: **Note:** Lifelines should end at the destroy marker (X) but due to a limitation of PlantUML, the lifeline reaches the end of diagram.

</div>

The following activity diagram summarizes what happens when a user executes the `addFromCsv` command.

![AddFromCsvActivityDiagram](images/AddFromCsvActivityDiagram.png)


#### Design considerations:

The design considerations below highlight alternative ways the command could have been implemented, and provides reasons for the choice of implementation.

##### Aspect: How are exceptions handled

For this aspect, we make a distinction between fatal exceptions and non-fatal exceptions.

* Fatal exceptions
  * Exceptions that cause the command to be unable to execute further.
  * These exceptions include but are not limited to: _missing parameters in CSV file_, _invalid file path, etc._    

* Non-fatal exceptions
  * The command could still continue executing even if this exception occured.
  * For example, _duplicate objects in the CSV file._
  * They tend to occur on a row by row basis in the CSV file.
  
* **Alternative 1:** Stop execution of command only for destructive exceptions.
  * Pros: User does not need to rerun the command for every exception that occurs.
  * Cons: Allowing certain exceptions to pass through may result in some bugs further down the line.

* **Alternative 2:** Stop execution for every exception that occurs.
  * Pros: Ensures correctness of the added objects.
  * Cons: May lead to worse user experience having to constantly rerun the command.
  
**Implementation**
 
Alternative 1 was chosen as the implementation with considerations from alternative 2.
* Upon encountering:
  * Fatal exception
    * The command stops execution and informs the user of the error.
  * Non-fatal exception
    * The command continues executing.
    * The rows where the exception occurs are recorded.
    * Upon finishing execution, the user is informed of the success and notified of the erroneous rows.
    
**Rationale**

As this command handles data from CSV files, it is likely the CSV files would be large (containing more than 10,000 rows).
  * Time taken for the `addFromCsv` command to execute is significant.
  
As such, the above implementation helps to reduce the need to read the large file repeatedly. 
  * User can fix the erroneous rows on a new CSV file.
  * Execution of the command would be faster than if the same large file was executed again.

This would minimise the impact to user experience as the user would spend less time fixing the errors.

#### Aspect: Absolute file path

The file path the command uses is the absolute path.
* The absolute path provides the complete details to locate the CSV file.
* Helps to avoid navigation errors that may result from using relative path names.

By allowing the user to specify the path name, it also gives the user a choice on where to put his CSV files instead of enforcing a particular directory for
him to store the files.

#### Aspect: Reusing list types and the list prefix 'l/'

**Concern**: The list types are used for the `list` command, which appear to be unrelated to the `addFromCsv` command.

**Rationale**

This command handles people, locations and visits which corresponded to the list types already implemented for use for the list command.

By using a format similar to the `list` command, it avoids the need for implementing a possible **data type** parameter which would not have a
significant difference from list type.

Furthermore, it allows the user to use a format that they are already comfortable with.

_{more aspects and alternatives to be added}_

### List high risk locations of infection
This feature allows the VirusTracker to display a list high risk location of infection. This list of high risk location 
of infection is generated using data currently stored in VirusTracker. The data include the infection status of all 
people, all visits that are made by infected people and all locations. 

**Format:** `list l/high-risk-locations`

#### Implementation
This feature is one of the different `list` commands. It has the same command word as other `list` commands, which is 
`list`. The `LIST_TYPE` of this command is `high-risk-location`.

* Infected person is defined as person with infection status as `true`
* Infected visit is defined as visit made by any infected person
* Infected location is defined as location that has been visited by any infected person
* Number of high risk location of infection is defined as:    
    * If number of infected location is larger than 60% of number of total number of location, then the number of high risk 
location is `40% * (number of total locations)`.  
    * Else, number of high risk location is the number of infected location. 

1. When this command is executed, a list of all infected people is obtained.
2. A list of all visits made by all infected people is obtained using the list of infected people.
3. Use a `HashMap` to store the location as the key and the number of visits made by any infected person to this 
location as the value. Generate this `HashMap` from the list of visits in step 2.
4. Sort the `HashMap` in Step 3 from most infected visits to least infected visits.
5. Calculate the number of high risk locaiton `n`, using number of infected location and number of total location.
6. Display the top `n` locations of the sorted infected location list as the list of high risk locations.

#### Sequence diagram
The sequence diagram below shows how the list operation works. Certain utility classes have been omitted for readability.
![ListHighRiskLocationSequenceDiagram](images/ListHighRiskLocationSequenceDiagram.png)

The following activity diagram summarizes what happens when a user executes the `list l/high-risk-locations` command.
![ListHighRiskLocationActivityDiagram](images/ListHighRiskLocationActivityDiagram.png)
#### Design consideration
##### Aspect: Definition of number of high risk location of infection
When displaying the list of high risk location of infection, the top few locations that has been most visited by any 
infected people are wanted. 

If number of infected location is larger than 60% of number of total number of location, then the number of high risk 
location is `40% * (number of total locations)`.  The number `40%` is an appropriate number as not too many nor too few
infected locations will be displayed.

Else, number of high risk location is the number of infected location. Since less than 40% of total locations are 
infected, all infected locations can be considered as high risk because they are the only few locations that are infected.

### \[Proposed\] Undo/redo feature

#### Proposed Implementation

The proposed undo/redo mechanism is facilitated by `VersionedAddressBook`. It extends `AddressBook` with an undo/redo history, stored internally as an `addressBookStateList` and `currentStatePointer`. Additionally, it implements the following operations:

* `VersionedAddressBook#commit()` — Saves the current address book state in its history.
* `VersionedAddressBook#undo()` — Restores the previous address book state from its history.
* `VersionedAddressBook#redo()` — Restores a previously undone address book state from its history.

These operations are exposed in the `Model` interface as `Model#commitAddressBook()`, `Model#undoAddressBook()` and `Model#redoAddressBook()` respectively.

Given below is an example usage scenario and how the undo/redo mechanism behaves at each step.

Step 1. The user launches the application for the first time. The `VersionedAddressBook` will be initialized with the initial address book state, and the `currentStatePointer` pointing to that single address book state.

![UndoRedoState0](images/UndoRedoState0.png)

Step 2. The user executes `delete 5` command to delete the 5th person in the address book. The `delete` command calls `Model#commitAddressBook()`, causing the modified state of the address book after the `delete 5` command executes to be saved in the `addressBookStateList`, and the `currentStatePointer` is shifted to the newly inserted address book state.

![UndoRedoState1](images/UndoRedoState1.png)

Step 3. The user executes `add n/David …​` to add a new person. The `add` command also calls `Model#commitAddressBook()`, causing another modified address book state to be saved into the `addressBookStateList`.

![UndoRedoState2](images/UndoRedoState2.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** If a command fails its execution, it will not call `Model#commitAddressBook()`, so the address book state will not be saved into the `addressBookStateList`.

</div>

Step 4. The user now decides that adding the person was a mistake, and decides to undo that action by executing the `undo` command. The `undo` command will call `Model#undoAddressBook()`, which will shift the `currentStatePointer` once to the left, pointing it to the previous address book state, and restores the address book to that state.

![UndoRedoState3](images/UndoRedoState3.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** If the `currentStatePointer` is at index 0, pointing to the initial AddressBook state, then there are no previous AddressBook states to restore. The `undo` command uses `Model#canUndoAddressBook()` to check if this is the case. If so, it will return an error to the user rather
than attempting to perform the undo.

</div>

The following sequence diagram shows how the undo operation works:

![UndoSequenceDiagram](images/UndoSequenceDiagram.png)

<div markdown="span" class="alert alert-info">:information_source: **Note:** The lifeline for `UndoCommand` should end at the destroy marker (X) but due to a limitation of PlantUML, the lifeline reaches the end of diagram.

</div>

The `redo` command does the opposite — it calls `Model#redoAddressBook()`, which shifts the `currentStatePointer` once to the right, pointing to the previously undone state, and restores the address book to that state.

<div markdown="span" class="alert alert-info">:information_source: **Note:** If the `currentStatePointer` is at index `addressBookStateList.size() - 1`, pointing to the latest address book state, then there are no undone AddressBook states to restore. The `redo` command uses `Model#canRedoAddressBook()` to check if this is the case. If so, it will return an error to the user rather than attempting to perform the redo.

</div>

Step 5. The user then decides to execute the command `list`. Commands that do not modify the address book, such as `list`, will usually not call `Model#commitAddressBook()`, `Model#undoAddressBook()` or `Model#redoAddressBook()`. Thus, the `addressBookStateList` remains unchanged.

![UndoRedoState4](images/UndoRedoState4.png)

Step 6. The user executes `clear`, which calls `Model#commitAddressBook()`. Since the `currentStatePointer` is not pointing at the end of the `addressBookStateList`, all address book states after the `currentStatePointer` will be purged. Reason: It no longer makes sense to redo the `add n/David …​` command. This is the behavior that most modern desktop applications follow.

![UndoRedoState5](images/UndoRedoState5.png)

The following activity diagram summarizes what happens when a user executes a new command:

![CommitActivityDiagram](images/CommitActivityDiagram.png)

#### Design consideration:

##### Aspect: How undo & redo executes

* **Alternative 1 (current choice):** Saves the entire address book.
  * Pros: Easy to implement.
  * Cons: May have performance issues in terms of memory usage.

* **Alternative 2:** Individual command knows how to undo/redo by
  itself.
  * Pros: Will use less memory (e.g. for `delete`, just save the person being deleted).
  * Cons: We must ensure that the implementation of each individual command are correct.

_{more aspects and alternatives to be added}_

### \[Proposed\] Data archiving

_{Explain here how the data archiving feature will be implemented}_


--------------------------------------------------------------------------------------------------------------------

## **Documentation, logging, testing, configuration, dev-ops**

* [Documentation guide](Documentation.md)
* [Testing guide](Testing.md)
* [Logging guide](Logging.md)
* [Configuration guide](Configuration.md)
* [DevOps guide](DevOps.md)

--------------------------------------------------------------------------------------------------------------------

## **Appendix: Requirements**

### Product scope

**Target user profile**:

* is a healthcare official involved in the Covid-19 pandemic
* has a need to generate statistics based on input data of the pandemic
* prefer desktop apps over other types
* can type fast
* prefers typing to mouse interactions
* is reasonably comfortable using CLI apps

**Value proposition**: produce useful statistics quickly and efficiently


### User stories

(For all user stories below, the **user** is a healthcare official, unless stated otherwise)

Priorities: High (must have) - `* * *`, Medium (nice to have) - `* *`, Low (unlikely to have) - `*`

| Priority | As a …​                                    | I want to …​                     | So that I can…​                                                        |
| -------- | ------------------------------------------ | ------------------------------ | ---------------------------------------------------------------------- |
| `* * *`  | user | generate a list of people currently stored in VirusTracker|  
| `* * *`  | user | generate a list of locations currently stored in VirusTracker|    
| `* * *`  | user | generate a list of visits currently stored in VirusTracker|    
| `* * *`  | user | add location data to a list | generate information about which location needs to be disinfected |
| `* * *`  | user | delete location data | to keep the locations list up to date|
| `* * *`  | user | edit location data | to keep the location information up to date with the latest address or name |
| `* * *`  | user | generate a list of infected people currently stored in VirusTracker|    
| `* * *`  | user | generate a list of quarantined people currently stored in VirusTracker|    
| `* * *`  | user with access to visits data from SafeEntry app   | add visit data to a list | generate desired lists and track contacts with the infected cases|
| `* * *`  | user with access to the visit list| delete all visits by date | remove all the outdated visits inside the list |
| `* * *`  | user setting up SafeEntry checkpoints | identify locations with high risk of infection | know which places need these checkpoints the most |
| `* * *`  | user publishing daily reports | generate daily statistics quickly and easily|                                                         |
| `* * *`  | user managing infected patient | update people's infection status | keep the current epidemic situation up to date |
| `* * *`  | user managing quarantined people | update people's quarantine status | be aware of a person's quarantine status |
| `* * *`  | user worried about virus outbreaks | generate locations that infected people have been to | disinfect those locations |
| `* * *`  | user worried about virus outbreaks | generate people that have been in contact with infected people | quarantine them for safety measures |
| `* * *`  | user with data stored in Excel files | import data from Excel files into VirusTracker | avoid typing out the data again |
| `* * *`  | user | export data from VirusTracker into a portable format | use the same data on multiple devices |

*{More to be added}*

### Use cases

(For all use cases below, the **System** is the `VirusTracker` and the **Actor** is the `user`, unless specified otherwise)

**UC01 - Add a location**

**MSS**

1.  User adds a location.
2.  System adds the new location.
3.  System displays the updated locations list.

    Use case ends.

**Extensions**
      
* 2a. System detects error in the entered data.
    * 2a1. System prompts user for correct data.
    * 2a2. User enters new data.  
    Steps 2a1-2a2 are repeated until the data entered are correct.  
    Use case resumes at step 3.
    
    Use case ends.

**UC02 - Delete a location**

**MSS**

1.  User identifies a location to be deleted.
2.  System deletes the location.
3.  System displays the updated locations list.

    Use case ends.

**Extensions**
      
* 2a. System detects that location does not exist.
    * 2a1. System prompts user for location to be deleted.
    * 2a2. User enters new data.  
    Steps 2a1-2a2 are repeated until the data entered are correct.  
    Use case resumes at step 3.
    
    Use case ends.

**UC03 - Edit a location**

**MSS**

1.  User edits a location.
2.  System updates the old location with the edited location.
3.  System displays the updated locations list.

    Use case ends.

**Extensions**
      
* 2a. System detects error in the entered data.
    * 2a1. System prompts user for correct data.
    * 2a2. User enters new data.  
    Steps 2a1-2a2 are repeated until the data entered are correct.  
    Use case resumes at step 3.
    
    Use case ends.

**UC04 - Add a visit**

**MSS**

1.  User chooses to add a visit.
2.  System requests for details of the visit. 
3.  User enters the required details.
4.  System adds the new visit.
5.  System displays the updated visits list.

    Use case ends.

**Extensions**
      
* 3a. System detects error in the entered data.
    * 3a1. System prompts user for correct data.
    * 3a2. User enters new data.  
    Steps 3a1-3a2 are repeated until the data entered are correct.  
    Use case resumes at step 4.
         
    Use case ends.

**UC05 - Delete visits**

**MSS**

1. User chooses to clear all the visits up to and before a date.
2. System requests for details of the targeted date.
3. User enters the required information.
4. System deletes the visits based on the information entered.
5. System displays the updated visits list.  
   
   Use case ends.
      
**Extensions**
       
* 3a. System detects error in the entered data.
    * 3a1. System prompts user for correct data.
    * 3a2. User enters new data.  
    Steps 3a1-3a2 are repeated until the data entered are correct.  
    Use case resumes at step 4.
         
    Use case ends.

**UC06 - Update infection status**

**MSS**

1. User provides updated quarantine status of a person.
2. System updates the quarantine status of the person.
3. System shows the list of people.

Use case ends.

**Extension**

* 2a. System is unable to find the person specified by the user.     
    * 2a1. System requests for correct details of the person.
    * 2a2. User enters the new details.  
    
  Steps 2a1 - 2a2 are repeated until the person specified by the user exists.  
  
  Use case resumes from step 3.
  
* *a. At any time, user choose to cancel the update.  
    
    Use case ends.  
            
**UC07 - Update quarantine status**

**MSS**

1. User provides updated quarantine status of a person.
2. System updates the quarantine status of the person.
3. System shows the list of people.

**Extension**

* 2a. System is unable to find the person specified by the user.     
    * 2a1. System requests for correct details of the person.
    * 2a2. User enters the new details.  
    
  Steps 2a1 - 2a2 are repeated until the person specified by the user exists.  
  Use case resumes from step 3.
  
* *a. At any time, user choose to cancel the update.    
    Use case ends.  

**UC08 - View all people**

**MSS**

1. User requests to list all people.
2. System shows a list of all people.

    Use case ends.

**Extensions**

* 2a. The list is empty.
    * 2a1. System generates a sample list.
    * 2a2. System shows the sample list.
    
  Use case ends.
    
**UC09 - View all locations**

**MSS**

1. User requests to list all locations.
2. System shows a list of all locations.

    Use case ends.

**Extensions**

* 2a. The list is empty.
    * 2a1. System generates a sample list.
    * 2a2. System shows the sample list.
    
  Use case ends.
    
**UC10 - View all visits**

**MSS**

1. User requests to list all visits.
2. System shows a list of all visits.

    Use case ends.

**Extensions**

* 2a. The list is empty.
    * 2a1. System generates a sample list.
    * 2a2. System shows the sample list.
    
  Use case ends.
    
**UC11 - View all infected people**

**MSS**

1. User requests to list all infected people
2. System shows a list of all infected people.

    Use case ends.

**Extensions**

* 2a. The list is empty.

  Use case ends.

* 2b. There are no infected people.
    * 2b1. Go to 2a.

**UC08 - View all quarantined people**

**MSS**

1. User requests to list all quarantined people
2. System shows a list of all quarantined people.

    Use case ends.

**Extensions**

* 2a. The list is empty.

  Use case ends.

* 2b. There are no quarantined people.
    * 2b1. Go to 2a.
    
**UC09 - View locations that an infected person has been to**

**MSS**

1. User requests to list locations that an infected person has been to.
2. System shows a list of locations that the infected person has been to.  

  Use case ends.

**Extensions**

* 3a. The list is empty.

    Use case ends.
    
* 2b. The person is not infected.

    Use case ends. 
    
* 2c. The system detects invalid user input.
    * 2c1. The system prompts the user to enter valid input.
    * 2c2. User enters new input.
    Steps 2c1-2c2 are repeated until the input entered is correct.  
        Use case resumes at step 2.
        
        Use case ends.       

**UC10 - View people in contact with an infected person**

**MSS**

1. User requests to list people in contact with an infected person.
2. System finds the infected person.
3. System shows a list of people in contact with an infected person.  

  Use case ends.

**Extensions**

* 3a. The list is empty.

    Use case ends.   
    
* 2b. The person is not infected.

    Use case ends. 
    
* 2c. The system detects invalid user input.
    * 2c1. The system prompts the user to enter valid input.
    * 2c2. User enters new input.
    Steps 2c1-2c2 are repeated until the input entered is correct.  
        Use case resumes at step 2.
        
        Use case ends.  
    
**UC11 - View high-risk locations**

**MSS**

1. User requests to list locations with high risk of infection.
2. System shows a list of locations with high risk.  

  Use case ends.

**Extensions**

* 2a. The location list is empty.

    Use case ends.

**UC12 - View summary of data**

**MSS**

1. User requests to view statistics of the day.
2. System shows the statistics of the day.

  Use case ends.

**Extensions**

* 2a. There is not enough information to produce a certain statistics.
    * 2a1. System flags that statistic as unavailable.
    * 2a2. For remaining valid statistics, go to 2. 
    
**UC13 - Add data from CSV file**

**MSS**

1. User requests to add data from a CSV file.
2. System requests for information for adding.
3. User enters the information required.
4. System adds the items and informs the user.

  Use case ends.

**Extensions**

* 3a. There is an error in the information entered.
    * 3a1. System requests for correct information.
    * 3a2. User enters new input.
    
    Steps 3a1 - 3a2 are repeated until the information entered is correct.  
        Use case resumes at step 3.
* 4a. The system is unable to add certain items in the file.
    * 4a1. System records the errors and notifies the user.
    
    System may decide to continue adding items. In that case, return to step 4.
    Otherwise, use case ends.
    
### Non-Functional Requirements

1.  Should work on any _mainstream OS_ as long as it has Java `11` or above installed.
2.  Should be able to hold up to 1000 items for each type of list without a noticeable sluggishness in performance for typical usage.
3.  Should be able to switch between different types of data and manipulate them efficiently and quickly.
4.  A user with above average typing speed for regular English text (i.e. not code, not system admin commands) should be able to accomplish most of the tasks faster using commands than using the mouse.
5.  Summary of statistics presented should be relevant and useful to the reader.
*{More to be added}*

### Glossary

* **Visits**: A visit event occurs whenever a `Person` enters a `Location`. The `Date` of this visit is also recorded.
* **Mainstream OS**: Windows, Linux, Unix, OS-X
* **Private contact detail**: A contact detail that is not meant to be shared with others

--------------------------------------------------------------------------------------------------------------------

## **Appendix: Instructions for manual testing**

Given below are instructions to test the app manually.

<div markdown="span" class="alert alert-info">:information_source: **Note:** These instructions only provide a starting point for testers to work on;
testers are expected to do more *exploratory* testing.

</div>

### Launch and shutdown

1. Initial launch

   1. Download the jar file and copy into an empty folder

   1. Double-click the jar file Expected: Shows the GUI with a set of sample contacts. The window size may not be optimum.

1. Saving window preferences

   1. Resize the window to an optimum size. Move the window to a different location. Close the window.

   1. Re-launch the app by double-clicking the jar file.<br>
       Expected: The most recent window size and location is retained.

1. _{ more test cases …​ }_

### Deleting a person

1. Deleting a person while all persons are being shown

   1. Prerequisites: List all persons using the `list` command. Multiple persons in the list.

   1. Test case: `delete 1`<br>
      Expected: First contact is deleted from the list. Details of the deleted contact shown in the status message. Timestamp in the status bar is updated.

   1. Test case: `delete 0`<br>
      Expected: No person is deleted. Error details shown in the status message. Status bar remains the same.

   1. Other incorrect delete commands to try: `delete`, `delete x`, `...` (where x is larger than the list size)<br>
      Expected: Similar to previous.

1. _{ more test cases …​ }_

### Saving data

1. Dealing with missing/corrupted data files

   1. _{explain how to simulate a missing/corrupted file, and the expected behavior}_

1. _{ more test cases …​ }_
