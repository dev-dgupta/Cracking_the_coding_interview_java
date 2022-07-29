## Duplicate URLs:
**You have 10 billion URLs. How do you detect the duplicate documents? In this case,
assume "duplicate" means that the URLs are identical.**

## Solution
So, we need to identify the duplicate/identical URLs. The Simplest solution, is to have a hashSet or hash table, and if the 
url is already present, you update the frequency.
But, for this we need to first determine the space that these 10 billion URLs take initially.

**How much space do 10 billion URLs take up?**
If each URL is an average of 100 characters, and each character is 4 bytes, then this list of 10 billion URLs
will take up about 4 terabytes. 

space =>     1 URL takes = 4x100 = 4kb of memory
            10 billion URLs = 4kb x 10 billion => 4kb x 1000 x 1 million  => 4MB x 1 million  => 4 TB 

We are probably not going to hold that much data in memory. We could solve this either by storing some data on disk or 
by splitting up the data across machines.

### Solution 1: Disk Storage
If we stored all the data on one machine, we would o two passes of the document. The first pass would
split the list of URLs into 4000 chunks of 1 GB each. An easy way to do that might be to store each URL in
a file named <x>.txt where x = hash(url)%4000. That is, we divide up the URLs based on their hash
value (modulo the number of chunks). This way, all URLs with the same hash value would be in the same file.
In the second pass, we would essentially implement the simple solution we came up with earlier: load each
file into memory, create a hash table of the URLs, and look for duplicates.

### Solution 2: Multiple Machines
The other solution is to perform essentially the same procedure, but to use multiple machines. In this solution,
rather than storing the data in the file <x>. txt, we would send the URL to machine x.
Using multiple machines has pros and cons. The main pro is that we can **parallelize the operation**, such
that all 4000 chunks are processed simultaneously. For large amounts of data, this might result in a faster
solution.
The disadvantage though is that we are now relying on 4000 different machines to operate perfectly. That
may not be realistic ( particularly with more data and more machines ), and we'll need to start considering
how to handle failure. Additionally, we have increased the complexity of the system simply by involving so
many machines.