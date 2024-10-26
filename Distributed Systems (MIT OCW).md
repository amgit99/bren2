If at all there is a way to do things on a single computer, DO IT. Do not go after making a distributed system. With that optimistic thought, let's begin this course.
So why make these:
- <span style="color:#e1db3d">Parallelism</span>, need fo speed
- <span style="color:#e1db3d">Fault Tolerance</span>, one goes down, there are many
- <span style="color:#e1db3d">Natural/Physical reasons</span>, some applications (two talking machines in two corners of the world) are inherently distributed.
- <span style="color:#e1db3d">Security reasons</span>, if an application has critical information, separation/isolation can make it more secure.
Challenges:
- <span style="color:#e1db3d">Concurrency</span>, having large communication delays makes the race condition & timing issues worse.
- <span style="color:#e1db3d">Partial Failure</span>, more moving parts meaning more subsets of systems can fail.
- <span style="color:#e1db3d">Getting the performance boost</span>, the overheads of a distributed system mostly outweigh the performance boost, delicate balance.