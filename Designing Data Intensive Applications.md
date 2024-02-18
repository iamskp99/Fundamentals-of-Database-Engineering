# Twitter Feed System Designing

So how can you retrieve all the tweets of all the people that you follow.

There are two approaches : 
  1. Retrieve all the posts from all the users that you follow based on timestamp.
      ```sql
      SELECT tweets.*, users.* FROM tweets
       JOIN users ON tweets.sender_id = users.id
       JOIN follows ON follows.followee_id = users.id
       WHERE follows.follower_id = current_user
      ```

  2. Make a cache table, something like a mailbox and for example when a user a post something then for all the users that follow a,
     the post will be pushed to the mailbox of each user.

Twitter used to follow approach 1 but systems struggled to keep up with the load.
But why ?

It is so because no. of reads became very large than no. of writes and the table size was also increasing with the no. of tweets.
So twitter came up with plan 2. Now the reads are optimised but the writes will now take time but its bearable because no. of 
writes are quite less. But there is the following scenario.

Let's say you follow n people on twitter and x of those users are your friends (who are not that famous) and one of the people you
follow is Lewis Hamilton. One night Lewis posts that he is moving from Mercedes to Ferrari. As you can see in this case when Lewis
will make a post on twitter then the no. of writes will be quite large so in that case it will be better for user to go for approach 1

Therefore,twitter uses hybrid of both approaches.


# LSM Trees

So, we have 3 moving parts here : memtable, index table and SS Table

<img width="1512" alt="Screenshot 2024-02-19 at 1 53 07 AM" src="https://github.com/iamskp99/Fundamentals-of-Database-Engineering/assets/42648568/5c6c2168-9bd5-48cd-b91a-5a2429bd8eb5">

For quick writes, we will maintain a BST in our memory and we will flush our writes directly.

Now the question arises. What about fault tolerance ?
We mantain a WAL for every transaction and persist it on the disk.
(Note : )






