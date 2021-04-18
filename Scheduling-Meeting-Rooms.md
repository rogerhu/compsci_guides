## Question
**Given an array of meeting time intervals consisting of start and end times `{[start1, end1], [start2,end2],...}` find the minimum number of conference rooms required.**

### Understanding the problem
Let's start by understanding the problem we're trying to solve.

If we were given the following intervals: `[07:00 - 09:00], [08:00 - 10:00] , [09:00 - 10:00]` what would we want to return? 

The first meeting definitely requires its own room. The second meeting will require an additional room. The third meeting can be in the same room as the first meeting. So in total we would need 2 rooms in this case.

In the example we had above, the meeting rooms were conveniently sorted by the start times. But can we always assume that will be the case? The problem doesn't state that's an assumption we can make, so let's take a look at another example of when the meeting room intervals aren't sorted in any way.

Suppose now we are given the following intervals : `[09:00 - 9:30], [07:00 - 09:00], [08:00 - 10:00], [09:30-10:00]`

If we drew this out, the meeting room intervals would look something like:

```

          -   (9:00-9:30)
  - - - -     (7:00-9:00)
      - - - - (8:00-10:00)
            - (9:30-10:00)
```

**We have 2 overlapping intervals, so we can see that we would need 2 meeting rooms for this case.** 

**Approach #1**:
Now that we have a good understanding of what we're trying to solve, let's plan out our approach.

A naive solution could be to iterate through the intervals and start a new meeting room if there are no current open rooms. If there is a current open meeting room, we can slot the next meeting in the list in the open meeting room and update the interval to reflect the times the meeting room is booked.

Going back to our example of the following intervals: `[09:00 - 9:30], [07:00 - 09:00], [08:00 - 10:00], [09:30-10:00]` if we followed our approach, the following would occur:
1. we allocate a meeting room for 9-9:30
2. we don't have to allocate another meeting room for the 7-9 meeting because it doesn't overlap with 9-9:30. Let's update the interval from 9-9:30 to 7-9:30 by combining the two meetings. This lets us know that we have one meeting room booked from 7-9:30. Our meeting room count is still at 1
3. 8-10 overlaps with 7-9:30, so we have to allocate another meeting room. our count is at 2 now
4. 9:30-10 overlaps with 8-10, but it doesn't overlap with 7-9:30, so we can reuse the first meeting room. Our meeting rooms count is still 2, and our intervals are now 8-10 and 7-10 (taking into account 3 different meetings)

In the end we would return 2.

But what is the problem with this approach? While step 4 was easy to visualize and explain, would we be able to code it efficiently? It's easy to tell if the current interval we're examining overlaps with the previous interval we're looked at, but how can we tell if it overlaps with other meeting rooms? We would have to iterate through all the current meeting rooms in progress to do the comparison, and that could turn out to be very inefficient. 

Furthermore, our example conveniently had intervals that were back to back. What if we had two intervals [7-9] and [10-12] ? Would we combine them to be 7-12? That would make us miss out on scheduling any meetings from 9-10, when the room is actually free. Could we instead have a 2D array that stored all the intervals for a meeting room ([[7-9], [10-12]]? While that's certainly one option, our approach seems to be growing in complexity as we think of these different cases.

**Approach #2**:
One of the reasons that our first approach didn't work was because we couldn't simply compare one interval with the interval before it. We had to take into account all the intervals before it in order to check if there was a free room. 

But what if we could simply compare every interval with the interval before it to check for overlaps? Would that help simplify our solution?

Let's assume the first step of our algorithm is to sort the intervals to make the rest of the solution simpler. Now we have to decide how to sort the intervals. Should we sort it by start time? End time? Duration?

Sorting by duration sadly wouldn't help us with the problem we ended up facing in Approach #1. Sorting by End Time also doesn't seem like a good idea as that doesn't really help us determine if two intervals overlap. Let's **sort by start time** as it can help us iterate through the list and determine overlaps. 

After we've sorted all the intervals by start time our previous example of : 
`[09:00 - 9:30], [07:00 - 09:00], [08:00 - 10:00], [09:30-10:00]`
would turn into
`[07:00 - 09:00], [08:00 - 10:00], [09:00 - 9:30], [09:30-10:00]`

With these sorted set of data, we can take a greedy approach. Always try to schedule a meeting room in an open room. If there are no open rooms, open a new room.

Pseudocode:
```
int numberOfMeetingRoomsNeeded(List listOfIntervals)
    listOfIntervals.sortByStartTime()
    foreach (interval : listOfIntervals)
        if interval.startTime < meetingWithEarliestEndTime
            numberOfMeetingRoomsUsed++
        else
            update meetingWithEarliestEndTime
    return numberOfMeetingRoomsUsed
```

Now that we have the pseudocode, there are a couple of things to think about as we translate it to actual code.
1. How should we implement `sortByStartTime()` ? One way is we can create a new function that takes in a list of intervals and returns a new list of the same intervals sorted. This would take up extra space creating the new list, so it would be preferable to sort the list in place. A common way in Java to sort objects in place is by implementing your own Comparator function.

2. How do we execute 'update meeting with earliest end time' ? One way to do this could be to keep an array of all the end times of ongoing meetings. This could get complicated to manage and keep sorted. But since the two factors we care about are (1) being able to get the earliest end time and (2) be able to insert a new end time and ensure that we can still always be able to do (1)  , it sounds like this is the perfect use case for using a MinHeap.

```
public int minMeetingRooms(Interval[] intervals) {
    if(intervals==null||intervals.length==0)
        return 0;
 
    Arrays.sort(intervals, new Comparator<Interval>(){
        public int compare(Interval i1, Interval i2){
            return i1.start-i2.start;
        }
    });
 
    PriorityQueue<Integer> queue = new PriorityQueue<Integer>();
    int numberOfMeetingRoomsUsed = 1;
    queue.offer(intervals[0].end);
 
    for(int i = 1; i < intervals.length; i++) {
        if (intervals[i].start < queue.peek()) {
            numberOfMeetingRoomsUsed++;
        } else {
            queue.poll();
        }
 
        queue.offer(intervals[i].end);
    }
 
    return numberOfMeetingRoomsUsed;
}
```

