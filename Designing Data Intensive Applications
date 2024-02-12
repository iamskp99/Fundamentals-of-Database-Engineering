# Twitter Feed System Designing

So how can you retrieve all the tweets of all the people that you follow.

There are two approaches : 
  1. Retrieve all the posts from all the users that you follow based on timestamp.
  2. Make a cache table, something like a mailbox and for example when a user a post something then for all the users that follow a,
     the post will be pushed to the mailbox of each user.

Twitter used to follow approach 1 but systems struggled to keep up with the load.
But why ?


Let's say you follow n people on twitter and x of those users are your friends (who are not that famous) and one of the people you
follow is Lewis Hamilton. One night Lewis posts that he is moving from Mercedes to Ferrari.

```sql
SELECT tweets.*, users.* FROM tweets
 JOIN users ON tweets.sender_id = users.id
 JOIN follows ON follows.followee_id = users.id
 WHERE follows.follower_id = current_user
```


