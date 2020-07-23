# TODO(s)

# ~~renaming existing commit(ing) - view to book(ing) - view~~

## ~~purpose~~
~~There should be two views: both a view to commit tickets (as a kind of work in progress ticket)~~
~~and a (kind of) booking tickets (as a kind of booking several time-entry of a day) to a booking code.~~

# ~~new view: commit - view~~

## purpose
~~After renaming the current committing view to booking view, a new commit for displaying th hours to commit on a (work in progress) ticket.~~

# create new data-base collection: commit(ing)-time-records

## purpose

In order to store the data of the new committing view, a new data-base collection is necessary.
(By the way, the same interface for both committing-time-records and booking-time-records can be used!)

# rename existing timeRecords to (something like) booking-time-records

# purpose
As there is already a timeRecords - collection, it should be renamed to booking-time-records.
(So it is clear what data it contains!)

# code cleanup and refactoring

## views
Some views contain complex logic. It should be revisited whether this logic is still necessary
(for a first draft of the time-tracking software, regarding simplicity and a "running" version).

## server
There are (at least) some parts of the code which get never called.
So, these part can be deleted!.

## client