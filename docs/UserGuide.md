---
layout: page
title: User Guide
---

PlanEase is a **desktop app tailored for event planners to organise and manage their contacts**. It is optimised for use via a Command Line Interface (CLI), while still retaining the familiar GUI from the address book. If you are a fast typist, PlanEase will accelerate your current workflow compared to your traditional address books.
* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

## Quick start

1. Ensure you have Java `11` or above installed in your Computer.

2. Download the latest [planease.jar](https://github.com/AY2223S2-CS2103-W16-3/tp/releases/tag/v1.3).

3. Copy the file to the folder you want to use as the _home folder_ for your PlanEase application.

4. Open a command terminal, `cd` into the folder you put the jar file in, and use the `java -jar planease.jar` command to run the application.<br>
   A GUI similar to the below should appear in a few seconds. Note how the app contains some sample data.<br>
   ![Ui](images/Ui.png)

5. Type the command in the command box and press Enter to execute it. e.g. typing **`help`** and pressing Enter will open the help window.<br>
   Some example commands you can try:

   * `list` : Lists all contacts.

   * `listevent` : Lists all events.

   * `listall` : Lists all contacts and events using a single command.

   * `add n/John Doe p/98765432 e/johnd@example.com a/John street, block 123, #01-01` : Adds a contact named `John Doe` to the Address Book.

   * `addevent ev/CEO's 50th Birthday Carnival from/17-07-2023 12:00 to/23-07-2023 22:00` : Adds an event named `CEO's 50th Birthday Carnival` to the Address Book.

   * `delete 3` : Deletes the 3rd contact shown in the current contact list.

   * `delevent 1` : Deletes the 1st event shown in the current event list.

   * `clear` : Deletes all contacts and events.

   * `exit` : Exits the app.

6. Refer to the [Features](#features) below for details of each command.

--------------------------------------------------------------------------------------------------------------------

## Features

<div markdown="block" class="alert alert-info">

**:information_source: Notes about the command format:**<br>

* Words in `UPPER_CASE` are the parameters to be supplied by the user.<br>
  e.g. in `add n/NAME`, `NAME` is a parameter which can be used as `add n/John Doe`.

* Items in square brackets are optional.<br>
  e.g `n/NAME [evt/EVENT_INDEX]` can be used as `n/John Doe evt/1` or as `n/John Doe`.

* Items with `…`​ after them can be used multiple times including zero times.<br>
  e.g. `[evt/EVENT_INDEX]…​` can be used as ` ` (i.e. 0 times), `evt/1`, `evt/1 evt/2` etc.

* Parameters can be in any order.<br>
  e.g. if the command specifies `n/NAME p/PHONE_NUMBER`, `p/PHONE_NUMBER n/NAME` is also acceptable.

* If a parameter is expected only once in the command but you specified it multiple times, only the last occurrence of the parameter will be taken.<br>
  e.g. if you specify `p/12341234 p/56785678`, only `p/56785678` will be taken.

* Extraneous parameters for commands that do not take in parameters (such as `help`, `list`, `exit` and `clear`) will be ignored.<br>
  e.g. if the command specifies `help 123`, it will be interpreted as `help`.

* All `DATETIME` must strictly be in this format: `DD-MM-YYYY HH:mm`.

</div>

### Contact Features

#### Adding a person: `add`

Adds a person to the address book and adds existing event to this person if event index is specified.

Format: `add n/NAME p/PHONE_NUMBER e/EMAIL a/ADDRESS [evt/EVENT_INDEX]…​`

* Person name cannot exceed 50 characters.
* The event index refers to the index number shown in the displayed event list.
* The event index **must be a positive integer** 1, 2, 3, …​

<div markdown="span" class="alert alert-primary">:bulb: **Note:**
A person can have any number of events (including 0)
</div>

Examples:
* `add n/John Doe p/98765432 e/johnd@example.com a/John street, block 123, #01-01` Adds person to the address book.
* `add n/Pearlyn Yeah p/89027382 e/pearlyn@email.com a/Blk 124 Orange Street 25 evt/1` Adds 1st event to the new person `Pearlyn Yeah` in the address book.

#### Listing all persons : `list`

Shows a list of all persons in the address book.

Format: `list`

#### Editing a person : `edit`

Edits an existing person in the address book.

Format: `edit INDEX [n/NAME] [p/PHONE] [e/EMAIL] [a/ADDRESS] [evt/EVENT INDEX]…​`

* Edits the person at the specified `INDEX`. The index refers to the index number shown in the displayed person list. The index **must be a positive integer** 1, 2, 3, …​
* At least one of the optional fields must be provided.
* Existing values will be updated to the input values.
* When editing tags, the tags will be added to the existing tags of the person i.e adding of tags is cumulative.
* You can remove all the person’s event tags by typing `evt/` without
    specifying any event index after it.

Examples:
*  `edit 1 p/91234567 e/johndoe@example.com` Edits the phone number and email address of the 1st person to be `91234567` and `johndoe@example.com` respectively.
*  `edit 2 n/Betsy Crower evt/` Edits the name of the 2nd person to be `Betsy Crower` and clears all existing event tags.

#### Locating persons by name: `find`

Finds persons whose names contain any of the given keywords.

Format: `find KEYWORD [MORE_KEYWORDS]`

* The search is case-insensitive. e.g `hans` will match `Hans`
* The order of the keywords does not matter. e.g. `Hans Bo` will match `Bo Hans`
* Only the name is searched.
* Only full words will be matched e.g. `Han` will not match `Hans`
* Persons matching at least one keyword will be returned (i.e. `OR` search).
  e.g. `Hans Bo` will return `Hans Gruber`, `Bo Yang`

Examples:
* `find John` returns `john` and `John Doe`
* `find alex david` returns `Alex Yeoh`, `David Li`<br>
  ![result for 'find alex david'](images/findAlexDavidResult.png)

#### Deleting a person : `delete`

Deletes the specified person from the address book.

Format: `delete INDEX`

* Deletes the person at the specified `INDEX`.
* The index refers to the index number shown in the displayed person list.
* The index **must be a positive integer** 1, 2, 3, …​

Examples:
* `list` followed by `delete 2` deletes the 2nd person in the address book.
* `find Betsy` followed by `delete 1` deletes the 1st person in the results of the `find` command.

### Event Features

#### Adding a new event : `addevent`

Adds a new event with the given event name, start date time, and end date time.

Format: `addevent ev/EVENT_NAME from/DATETIME to/DATETIME`

* Event name can be a combination of alphanumeric and punctuations with spaces.
* Event name must begin with alphanumeric.
* Event name cannot exceed 50 characters.
* Start date time cannot be after the end date time.

<div markdown="span" class="alert alert-primary">:bulb: **Note:**
Past events can be added for tracking purposes.
</div>

Examples:
* `addevent ev/Wedding Dinner from/01-05-2023 17:00 to/01-05-2023 21:00`
* `addevent ev/Dinner from/01-05-2023 17:30 to/01-05-2023 19:30`

#### Listing all events : `listevent`

Prints all the existing events in the address book.

Format: `listevent`

* Does not require any additional user arguments.
* Follow strictly to the command `listevent`.

Examples:
* `listevent` prints a list of events.

#### Listing persons from an event: `listevcontact`

Shows a list of persons in the address book whose event set contains the specified event.

Format: `listevcontact EVENT_INDEX`

* Lists all persons from the event at the specified `EVENT_INDEX` in the address book.
* The event index refers to the index number shown in the displayed event list.
* The event index **must be a positive integer** 1, 2, 3, …​

Examples:
* `listevent` followed by `listevcontact 2` lists all the persons whose event set contains the 2nd event in the event list.

#### Sorting events: `sortevent`

Sorts the events in the current event list based on the user's input.

Format: `sortevent SORT_TYPE`

* Sorts all the events in the current event list based on the specified `SORT_TYPE`.
* `SORT_TYPE` must be one of these values:
  * `a`: Sorts based on event names in ascending ASCII order.
  * `b`: Sorts based on event names in descending ASCII order.
  * `c`: Sorts based on start date times in ascending order.
  * `d`: Sorts based on end date times in ascending order.
* There must be at least 2 events in the current event list to sort the event list.

Examples:
* `listevent` followed by `sortevent c` sorts all the events in the address book based on their start date times in ascending order.

#### Editing an event : `editevent`

Edits an existing event in the address book.

Format: `editevent EVENT_INDEX [ev/EVENT_NAME] [from/DATETIME] [to/DATETIME]`

* Edits the event at the specified `EVENT_INDEX` and edits the relevant event tag tied to all persons in the address book.
* The event index refers to the index number shown in the displayed event list. The event index **must be a positive integer** 1, 2, 3, …​
* At least one of the optional fields must be provided.
* Existing values will be updated to the input values.
* Edits will not allow start date time to be after the end date time.

Examples:
*  `editevent 1 ev/Birthday Party from/17-07-2023 12:00` Edits the event name and start datetime of the 1st event to be `Birthday Party` and `17-07-2023 12:00` respectively.

#### Finding events by name: `findevent`

Finds events whose names contain any of the given keywords.

Format: `findevent KEYWORD [MORE_KEYWORDS]`

* The search is case-insensitive. e.g `party` will match `Party`
* The order of the keywords does not matter. e.g. `Birthday Party` will match `Party Birthday`
* Only the event name is searched.
* Only full words will be matched e.g. `Concert` will not match `Concerts`
* Events matching at least one keyword will be returned (i.e. `OR` search).
  e.g. `Wedding Dinner` will return `Wedding Ceremony`, `Birthday Dinner`

Examples:
* `findevent Dinner` returns `Wedding Dinner` and `Dinner and Dance`
* `findevent birthday party` returns `Birthday Lunch`, `Graduation Party`

#### Deleting an event : `delevent`

Deletes the specified event from the event list and deletes all occurrences of the event tied to persons in the address book, if any.

Format: `delevent EVENT_INDEX`

* Deletes the event at the specified `EVENT_INDEX` and all occurrences of the event tied to persons in the address book, if any.
* The event index refers to the index number shown in the displayed event list.
* The event index **must be a positive integer** 1, 2, 3, …​

Examples:
* `listevent` followed by `delevent 2` deletes the 2nd event in the event list and all occurrences of the 2nd event tied to persons in the address book, if any.

### Other Features

#### Listing all persons and events : `listall`

Prints all the existing persons and events in the address book.

Format: `listall`

#### Viewing help : `help`

Shows a message explaining how to access the help page.

![help message](images/helpMessage.png)

Format: `help`

#### Clearing all entries : `clear`

Clears all entries from the address book.

Format: `clear`

#### Exiting the program : `exit`

Exits the program.

Format: `exit`

#### Saving the data

AddressBook data are saved in the hard disk automatically after any command that changes the data. There is no need to save manually.

#### Editing the data file

AddressBook data are saved as a JSON file `[JAR file location]/data/addressbook.json`. Advanced users are welcome to update data directly by editing that data file.

--------------------------------------------------------------------------------------------------------------------

## FAQ

**Q**: How do I transfer my data to another Computer?<br>
**A**: Install the app in the other computer and overwrite the empty data file it creates with the file that contains the data of your previous AddressBook home folder.

--------------------------------------------------------------------------------------------------------------------

## Command summary

Action | Format, Examples
--------|------------------
**Add Contact** | `add n/NAME p/PHONE_NUMBER e/EMAIL a/ADDRESS [ev/EVENT_INDEX] …​` <br> e.g., `add n/Pearlyn Yeah p/89027382 e/pearlyn@email.com a/Blk 124 Orange Street 25 evt/1`
**Add Event** | `addevent ev/EVENT_NAME from/DATETIME to/DATETIME` <br> e.g., `addevent ev/Wedding Dinner from/01-05-2023 17:00 to/01-05-2023 21:00`
**Clear** | `clear`
**Delete Contact** | `delete INDEX`<br> e.g., `delete 3`
**Delete Event** | `delevent EVENT_INDEX` <br> e.g., `delevent 2`
**Edit Contact** | `edit INDEX [n/NAME] [p/PHONE_NUMBER] [e/EMAIL] [a/ADDRESS] [evt/EVENT_INDEX]…​`<br> e.g.,`edit 2 n/James Lee e/jameslee@example.com`
**Edit Event** | `editevent EVENT_INDEX [ev/EVENT_NAME] [from/DATETIME] [to/DATETIME]​`<br> e.g.,`editevent 1 ev/Birthday Party from/17-07-2023 12:00`
**Find Contact** |  `find KEYWORD [MORE_KEYWORDS]`
**Find Event** | `findevent KEYWORD [MORE_KEYWORDS]`
**List All Contacts and Events** | `listall`
**List Contact** | `list`
**List Event** | `listevent`
**List Contact From event** | `listevcontact`
**Help** | `help`
**Sort Event** | `sortevent SORT_TYPE` <br> e.g., `sortevent c`





