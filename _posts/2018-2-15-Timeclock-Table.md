---
layout: post
title: Improving UI in the Time Clock
---

Our time clock has had some issues with how with how supervisors add, edit, and delete student punches. Supervisors could modify a punches, but afterwards it was hard to see how your edits effect the students time. This past week I aimed to provide a clear interface to users that gives instant feedback to reduce confusion and frustration of the supervisors.

The previous flow for supervisors is:

1. Make an edit
1. Click off of a student and then click back or refresh the page
1. Look at the amount of hours worked (This could be different then what gets printed on the time sheet.)
1. Print the time sheet
1. Check to see if the pay actually worked out

_Repeat as necessary_

All of these steps lead to supervisors getting frustrated or confused. The solution, for them, was to either put in a ticket or try to edit the time until something accurate resulted.

## Improving UI

If someone came by to ask for help with their student's punches, it was easiest for me to use something like [MySQL Workbench](https://www.mysql.com/products/workbench/) and see what the punches look like in the database. Most of the time there was some kind of odd misplaced punch and I would delete it. This was almost always followed by the supervisor saying something like, "Oh it was super clear when you look at it that way, but it's hard to see that with what we have."

I did a little searching and decided to use [React Table](https://github.com/react-tools/react-table) because it had some built in sorting, searching, and it was easy to just throw your data at it. The UI is clear and more to what I feel most people have seen before. It basically looks like a spreadsheet.

## Responsiveness

We needed a way to provide instant feedback to a supervisor right after a someone makes an edit. We needed to get away from users having click around or refresh the page to try and trigger some `componentDidMount` to get the student's information to update. The new table view uses the [Presentation/Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0), so the top level container gets the punches and drops them down to it's children. So if I ever want to show a user how their changes affect the student's hours worked, I can re pull the student's punches and the UI will update.

I also provided a few checks for students punches to help supervisors see problems like:

1. **Double Clock Ins/Outs** - A supervisor accidentally add in an extra clock in/out.
1. **Overworking/Missed Punch** - A student has allegedly worked over 8 hours straight.

Both of these will get highlighted for the supervisor to draw their eyes straight to the problem. This directly fixes the issue of a supervisor not knowing if the change they just made causes more issues. As soon as a supervisor makes an edit, the punches get re-pulled, both of these checks happen, and the rows will get highlighted if anything is wrong.

## Now What?

I should have the table finished on Monday and will release it to our largest departments. After two reps of pay periods, we will pull feed back
