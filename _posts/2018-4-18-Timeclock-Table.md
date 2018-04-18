---
layout: post
title: Improving UI in the Time Clock
---

Our time clock has had some issues with how with how supervisors add, edit, and delete student punches. Supervisors could modify a punches, but afterwards it was hard to see how your edits effect the students time. This past week I aimed to provide a clear interface to users that gives instant feedback to reduce confusion and frustration of the supervisors.

The previous flow for fixing a punch:

1.  Find the error. _This can be a task on it's own_.
1.  Make an edit
1.  Click off of a student and then click back or refresh the page
1.  Look at the amount of hours worked _This could be different then what gets printed on the time sheet._
1.  Print the time sheet
1.  Check to see if the pay actually worked out

_Repeat as necessary_

All of these steps lead to supervisors getting frustrated or confused. The solution, for them, was to either put in a ticket or try to edit the time until something accurate resulted.

## Improving UI

If someone came by to ask for help with their student's punches, it was easiest for me to use something like [MySQL Workbench](https://www.mysql.com/products/workbench/) and see what the punches look like in the database. Most of the time there was some kind of odd misplaced punch and I would delete it. This was almost always followed by the supervisor saying something like, "Oh it was super clear when you look at it that way, but it's hard to see that with what we have."

I did a little searching and decided to use [React Table](https://github.com/react-tools/react-table) because it had some built in sorting, searching, and it was easy to just throw your data at it.

## Responsiveness

We needed a way to provide instant feedback to a supervisor right after a someone makes an edit. We needed to get away from users having click around to try and trigger a `componentDidUpdate` or refresh the page to try and trigger some `componentDidMount` to get the student's information to update. The new table view uses the [Presentation/Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0), so the top level container gets the punches and drops them down to it's children. So if I ever want to show a user how their changes affect the student's hours worked, I can re pull the student's punches and the UI will update.

I also provided a few checks for students punches to help supervisors see problems like:

1.  **Double Clock Ins/Outs** - A supervisor accidentally add in an extra clock in/out.
1.  **Overworking/Missed Punch** - A student has allegedly worked over 8 hours straight.

Both of these will get highlighted for the supervisor to draw their eyes straight to the problem. This directly fixes the issue of a supervisor not knowing if the change they just made causes more issues. As soon as a supervisor makes an edit, the punches get re-pulled, both of these checks happen, and the rows will get highlighted if anything is wrong.

## Punch Error Checking

Checking the punches has to find errors that are objectively incorrect. Objective errors include working too much and malformed clock ins/outs. Checking for over work is straight forward. We already have some code that calculates the amount of time worked and how much a student has been paid per punch. It's as simple as

`amountOfHoursWorked > maxHoursWorked`

Finding the malformed punches is a slightly harder issue, but not too bad. Our data base punches table is set up to take in single punches. So each individual row is a punch. Pairing up these punches can be done, but is brittle to punches that don't follow a clock in followed by a clock out.

How do this rogue punches enter the database?

_Ya students figuring out a way to clock in a million times?_

It mostly boils down to a supervisor adding in extra punches because the old UI isn't clear about how the punches are laid out. The old UI will try to pair up the punches and let you know how much a student has worked, but if there is a rogue punch it becomes inaccurate. The goal of this table is to provide both the derived calculations and the raw data.

So how is it done?

`^10(10)*$`

I take just the boolean value of the `isClockIn` attribute of a punch and make string of 1's and 0's. If the string fails, then I can highlight the row.

Crazy to think that the lunch box problems I got in my university's automata class could be applied to something in an actual application.

<img src="{{ site.baseurl }}/images/timeclock-table/studentsTable.webp" alt="Table of students time totals" />

This is the table that the users first see. It lists their students who have punches for the pay period they have selected.

If a student has worked too much or has double clock ins/out they will be highlighted

<img src="{{ site.baseurl }}/images/timeclock-table/studentTableWithHighlight.webp" alt="A close up of a student's row that is highlighted yellow" />

The student above is highlighted because one of their punches is over 9 hours. Once a supervisor clicks on the student they will see that student's table as shown below.

<img src="{{ site.baseurl }}/images/timeclock-table/individualTable.webp" alt="Table of an individual student's time totals" />

At this point the supervisor has options to add, edit, or delete any of the punches that may be cause the row to highlight. After any of these actions, the punches will be re pulled and the calculations for pay, hours worked, and potential errors will be reevaluated.

The new flow is

1.  See that a student is highlighted.
1.  Click on the student and sees the highlighted punch.
1.  Edit the punches to the correct time _After submitting the page re calculates amounts_

_Repeat as necessary_
