RetroAchievements (RA) provides users the ability to earn achievements in retro games from an RA achievement set. It awards achievements by comparing a game’s memory, henceforth referred to as RAM, to achievement code written by an RA developer. Achievement code, also known as logic, is a list of memory conditions chosen by the developer, that when all are simultaneously true on a single frame, will award an achievement.

## What Is An Achievement Set?

An achievement set is the compilation of individual achievements, rich presence and leaderboards for a particular game. Achievements are comprised of achievement logic, a title, a description, a point value and a badge. Additionally, a set must contain a Rich Presence script which provides site users information on where active players are in a game. Sets may have leaderboards that track how well players perform in certain things in a game such as how quickly they can beat stages, how many points they can score and many other things. RA users interface with all these components and subcomponents. Each one is a vital part of the player experience.

## What Is Achievement Code And How Is It Written?

RA code is a custom text based language that is interpreted by RA supporting emulators. Developers write code using either the Achievement Editor in the RAIntegration toolkit or by using RATools, a standalone executable that provides a scripting language for achievement development. Both the achievement editor and RATools produce code that is used for all achievements, leaderboards and Rich Presence.

## How Is Achievement Code Processed?

A game’s RAM is its most basic form. RAM is simply the game's memory addresses and their respective values at any given time. When processed by a console or emulator, the RAM can be used to recognize what is happening within the game. Games are processed in a series of frames, generally 60, 50, 30 or 25 per second depending on console and format. RA processes all unearned achievement logic in a set every frame. Because of this, developers can create logic such that when all of its conditions are simultaneously true, identify something uniquely precise happening in a game and award an achievement, activate a leaderboard and provide accurate Rich Presence. It is an achievement developer’s job to understand enough of a game's RAM to be able to construct this logic.

Understanding how the RAM works that a developer intends to use in their logic is essential to creating a stable set that awards achievements, submits leaderboard scores and accurately displays Rich Presence only as intended. RA developers use developer emulator tools to inspect a game's RAM and determine which addresses are responsible for things they intend to use for logic. Developers have many ways to precisely define exactly which memory conditions must be true in order to construct logic, including retaining knowledge that something occurred previously in memory, but may no longer be true.

## Achievement Logic and Processing Example

As a simple example, to award an achievement for obtaining the hammer in Zelda II: The Adventure of Link, a developer would need to figure out which memory addresses within the game were associated with a few unique things about obtaining the hammer. There are often many ways to do this. One way a developer might approach this achievement is to find the addresses for the room ID to ensure the player is in the room with the hammer, also perhaps an area ID to ensure the player is in the right section of the game and lastly an address that indicates whether the player actually gets the hammer. The reason a room and area ID are important is because when a player loads a save file that has collected the hammer, the memory would indicate that possessed of the hammer goes from untrue to true, but this isn't where the achievement should award. The room and area ID conditions would ensure that a player was actually obtaining the hammer in game and not just loading a save file that already possessed it. A solid achievement could be constructed to award when the following is true simultaneously:

**Room ID = room the hammer is in**<br>
**Area ID = area of game the hammer is in**<br>
**Possesion of hammer changes from not possessed to possessed**

Here is what this achievement would look like in the Achievement Editor
![image](https://github.com/RetroAchievements/docs/assets/106546659/f59cfd18-452d-45a4-9847-92ac0cd86b29)<br>
In this example, address 0x0561 is the room ID and its value is 0x15 when the player is in the room with the hammer, address 0x076e is the area ID and its value is 0x02 when in Death Mountain where the hammer is located and Bit0 of address 0x078b indicates if the player has the hammer. This bit changes from 0 to 1 when the player acquires the hammer, so the achievement checks for a frame where this bit is greater than it was the previous frame which is precisely when the hammer is obtained since bits can only be either 1 or 0. If all three conditions are true on the same frame, the achievement is awarded. This can only happen when the player obtains the hammer while in the hammer room in Death Mountain, not at some other time such as loading a save file.

Every frame the player is playing the game, the RAM is being compared to the achievement logic to check if all conditions are true on that frame. For this example achievement, all conditions can only be true on the same frame when a player is in the room and area where the hammer is obtained and the player obtains the hammer. When that happens, all of the conditions defined in the logic will be true and the achievement will be immediately awarded.

## Leaderboard and Rich Presence Code

Leaderboards function quite similarly to achievements in terms of logic, but need a list of conditions to tell the leaderboard when to activate, when to cancel itself, and when and what value to submit to the leaderboard. Creating a leaderboard is essentially like creating a few small achievements that will be processed sequentially.

Rich Presence code is very similar to achievement and leaderboard code, but is written slightly differently due to the way it is handled by the site. Rich Presence is written and submitted to RA as a script, as opposed to a string of code like achievements and leaderboards. It still uses the same fundamental code as achievements and leaderboards, but has some additional features such as custom macros that may be used.

## Take Aways

The key to writing solid code is to first understand what in RAM can be used to recognize a particular game event for which a developer intends to award an achievement, use for a leaderboard or Rich Presence. The next steps are finding the necessary memory addresses and their values to recognize when the event occurs and constructing logic to create a list of conditions that will only be simultaneously true on the intended frame.
