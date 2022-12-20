# machine_notifier
A python script for audibly notifying when a CNC machine has finished its cycle
or alarmed out.
## Purpose
The purpose of this project is to add an audible alarm to the visible light
signal for our CNC machines. This is meant to increase awareness of when CNC 
cycles finish or when machines alarm out. The visible lights alone do not
provide immediate recognition of this state change. Adding an audible alarm
allows an operator to passivly know about the change without having to
constantly be looking at indicator lamps. The inicator lamps can then be used to
identify what machine needs attention. 

## Theory
The Patlites are wifi enabled indicator lamps that update a database whenever 
their state change. This project contains two scripts. The first monitors this
table for changes and makes decisions about what notifications need to be made.
These descisions are written to another table. The second script reads this 
descision table and makes the necessary announcements. 

The reason this script is broken into two parts is to cut down on the number of 
table queries necessary while still allowing for a distributed notification 
network. By having several computers running the second script, each production
cell can have its own notification speaker that only announces relevant changes
to machines. This provides scalability to more production cells and cuts down on
the possible annoyance of being bombarded by irrelevant notifications. 

Only the first script needs to query many table rows. This script may have to 
query several hundred rows, while the secondary script will only have to query
as many rows as there are machines. Because of the differing cycle times, I 
estimate that to find the current state of a machine 100*"machine count" rows
need to be quried. If the intermediatry table were not used this would cause
100*"machine count"*"speakers" queries. With the intermediary table this becomes 
100*"machine count" + "machine count"*"speakers". Since speakers is a function
of machine count, without an intermediary the number of queries expands by some
power while with the intermediary the number of queries only expands linearly. 
