# Bitmaps
A bitmap is a string of $n$ binary digits that can be used to represent the status of $n$ items. For example, suppose we have several resources, and the availability of each resource is indicated by the value of a binary digit: 0 means that the resource is available, while 1 indicates that it is unavailable (or vice versa). The value of the $i^{th}$ position in the bitmap is associated with the $i^{th}$ resource. As an example, consider the bitmap shown below:
`001011101`
REsources 2, 4, 5, 6, and 8 are unavailable; resources 0, 1, 3, and 7 are available. 
 The power of bitmaps becomes apparent when we consider their space eficiency. If we were to use an eight-bit Boolean value instead of a single bit, the resulting data structure would be eight times larger. Thus, bitmaps are commonly used when there is a need to represen the availability of a large number of resources. Disk drives provide a nice illustration. A mediu-sized disk drive might be divided into several thousand individual units, called disk blocks. A bitmap can be used to indicate the availability of each disk block.
  In summary, data structures are pervasive in operating system implementations. Thus, we will see the structures discussed here, along with others, throughout this text as we explore kernel algoritms and their implementations. 

# Computing Environments
So far, we have briefly described several aspects of computer systems and the operating systems that manage them. We turn now to a discussion of how operating systems are used in a variety of computing environments. 
## Traditional Computing
 As computing has matured, the lines seperating many of the traditional computing environments have blurred. Consider the "typical office environment." Just a few years ago, this environment consisted of PCs connected to a network, with servers providing file and print services. Remote access was awkward, and portability was achieved by use of laptop computers.
  