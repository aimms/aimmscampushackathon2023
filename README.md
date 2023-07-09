# aimmscampushackathon2023

This repository contains all information on the AIMMS 2023 Campus Hackathon

## The problem

The problem we selected is based on the [ITC 2021: International Timetabling Competition on Sports Timetabling](https://www.sportscheduling.ugent.be/ITC2021/index.php).

We will plan one-on-one matches between sports teams that belong to a single league. We call the complete set of matches a tournament. This tournament follows a double round-robin (2RR). Every team plays against every other team two times: once at home, once away. As a result we have (N - 1) x 2 matches per team, where N is the total number of teams.

The planning horizon is divided into slots ((N - 1) x 2). We assume that each team plays exactly once per slot. We also assume an even number of teams.
There are several types of constraints that we briefly present here. 

**The challenge can be very hard! We do not expect you to implement all constraints. We will have a growing level of difficulties, so don’t worry to get them all done!**

### Constraints

There are both soft and hard constraints. SOFT and HARD tags are included with each one. A PENALTY is also included for each unit of violation. The unit of violation depends on the constraint. A HARD constraint should always be satisfied (although this is sometimes very difficult).

### Objective function

We want to minimize the sum of penalized soft constraints violations.

### Structure

If the timetable instance is "phased", then the first half of slots in the tournament is a complete round-robin and the second half is another complete round-robin. That is, each pair of teams only sees each other once in each half, with their home-away status inverted between halves.

### Capacity Constraints

> \<CA1 teams="0" max="0" mode="H" slots="0" type="HARD"/>

Each team from teams plays at most max home/away games (mode = "H“ or “A”) during time slots in slots. Penalty is number of games over max.

Ex.: Team 0 cannot play at home on time slot 0.


> \<CA2 teams1="0" min="0" max="1" mode1="HA" mode2="GLOBAL" teams2="1;2“ slots ="0;1;2" type="SOFT"/>

Each team in teams1 plays at most max home/away/any games (mode1 = "H“, “A” or “HA”), against teams in teams2 during time slots in slots. Mode2 will always be “GLOBAL”.

Penalty is number of games over max.

Ex.: Team 0 plays at most one game against teams 1 and 2 during the first three time slots. 

> \<CA3 teams1="0" max="2" mode1="HA" teams2="1;2;3" intp="3" mode2="SLOTS" type="SOFT"/>

Each team in teams1 plays at most max home/away/any games (mode1 = "H“, “A” or “HA”) against teams in teams2 in each sequence of intp time slots. mode2 = "SLOTS“ always.

Penalty is number of games over max in any sequence of intp.Ex.: Team 0 plays at most two consecutive games against teams 1, 2, and 3. 

> \<CA4 teams1="0;1" max="3" mode1="H" teams2="2,3" mode2="GLOBAL“ slots ="0;1" type="HARD"/> 
 
Teams in teams1 play at most max home/away/any games (mode1 = "H“, “A” or “HA”) against teams in teams2 during time slots in slots (when mode2 = "GLOBAL") or during each individual time slot in slots (mode2 = "EVERY").

Penalty is number of games over max.

Ex.: Teams 0 and 1 together play at most three home games against teams 2 and 3 during the first two time slots. 

### Game constraints

> \<GA1 min="0" max="0" meetings="0,1;1,2;" slots="3" type="HARD"/>

At least min and at most max games from meetings = {(i1, j1 ), (i2, j2 ),… } take place during time slots in slots.

Penalty is number of games under min or over max.

Ex.: Game (0,1) and (1,2) cannot take place during time slot 3. 

### Break constraints

> \<BR1 teams="0" intp="0" mode2="HA" slots="1" type="HARD"/>

Each team in teams has at most intp home/away/any breaks (mode2 = "H“, “A” or “HA”) during time slots in slots. 

Penalty is the number of breaks over the limit. We say that a team has a break if it has two consecutive home games, or two consecutive away games.

Ex.: Team 0 cannot have a break on time slot 1 (they cannot repeat the home or away of slot 0).

> \<BR2 homeMode="HA" teams="0;1" mode2="LEQ" intp="2" slots="0;1;2;3" type="HARD"/>

The sum over all breaks (homeMode = "HA", the only mode we consider) in teams is no more than (mode2 = "LEQ", the only mode we consider) intp during time slots in slots. 

Penalty is the number of breaks over the limit.

Ex.: Team 0 and 1 together do not have more than two breaks during the first four time slots. 

### Fairness Constraints

> \<FA2 teams="0;1;2" mode="H" intp="1" slots="0;1;2;3" type="HARD"/>

Each pair of teams in teams has a difference in played home games (mode = "H", the only mode we consider) that is not larger than intp after each time slot in slots.

Penalty is the largest difference between home games over the limit.

Ex.: the difference in home games played between teams 0, 1 and 2 is not larger than 1 during the first four time slots.

### Separation Constriants

> \<SE1 teams="0;1" min="5" mode1="SLOTS" type="HARD"/>

Each pair of teams in teams has at least min time slots (mode1 = "SLOTS", the only mode we consider) between two consecutive mutual games. 

Ex.: there are at least 5 time slots between the mutual games of team 0 and 1 (that is matches 0-1 and 1-0). 

Penalty is the number of slots less then the minimum limit.