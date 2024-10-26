
| Property         | Value             |
| ---------------- | ----------------- |
| 📅 Date          | 13-09-2024, 01:49 |
| 🏷️ Tags         |                   |
| 🔗 Related Notes |                   |
Distributing traffic between instances of services is called Load Balancing. The logic that decides how the traffic is distributed is called the LB algorithm(round robin, hashing, random select, shortest queue).
Least Request is the one where a count of served requests is maintained and the one with lowest value is given more preference. Hashing is done using the http Header Name, some http Cookie, Source ip (this can be used for route requests from a certain area to a specific instance). One can also assign weights to the instance based on the locality, (80% to the closer instance, 20% to the farther one, overall the average response time will be same). 