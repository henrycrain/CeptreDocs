---
title: Escape the Castle
tags: [getting_started, troubleshooting]
keywords:
summary: "Escape the castle is a video game whose mechaics we will define using Ceptre."
sidebar: mydoc_sidebar
permalink: Freedom.html
folder: Examples
---

## Concept

To escape the castle, we are going to make a map of game world in which main character "Thomas" is trying to escape a castle by collecting some items. In this rough concept we already have an idea of what the rules of this world will be:
Thomas will only be able to move from one room to another if the rooms are beside each other and if the entrance is open.
We also know which items are needed to be able to advance in the "game", where they are and where they will be needed.

Finally, we also know what the goal is: Escape the castle.
![FreedomConcept](https://user-images.githubusercontent.com/42487202/145275919-8f7e587a-df34-4c05-bcb5-b44b0293b1e5.png)



## Add Sets

Now we will add the elements we saw in the concept into the project. To start with, Navigate to the Editor Tab of the Ceptre Editor and locate the Sets. Select the option to Add a New Set, a dialogue box will appear from the website which will allow you to name it. 
 Our game world here is made up of 3 sets i.e. elements, rooms and trapped.

 1. elements - thomas, metal_key, golden_key, keypad_code (It comprises elements in game world)
 2. rooms - hallway, home_library, master_bedroom, main_room, road_home (It comprises all the rooms that Thomas will be able to move back and forth as well as interact with the items in them)
 3. trapped - dungeon, secret_room (It comprises all the rooms that Thomas can't interact with until he uses the respective key)

 Cepter text based code:
 ```
numbers : type.

elements : type.
thomas : elements.
metal_key : elements.
golden_key : elements.

rooms : type.
hallway : rooms.
home_library : rooms.
master_bedroom : rooms.
main_room : rooms.
road_home : rooms.

trapped : type.
dungeon : trapped.
secret_room : trapped.

```

Cepter Web Editor simulation :

<video width = "650" controls>
    <source src = "https://user-images.githubusercontent.com/42487202/145275866-9cdb9269-a5a4-44bf-9f7e-4409f88ab016.mp4">
</video>

## Add Predicates

In this example we have 4 predicates, note that we lock every predicate after definition so that we don't edit them erroneously post definition:

1. Locked - As the name suggests, this state should represent an element being in a room that they can't get out.
It is composed of two arguments:

    - Element: A character or an item
    - Trapped: A room that cannot be interacted until Thomas has a key.


2. At - This Predicate is similar to the previous one as they both indicate the location of an element, however in this one the elements can move between the rooms.
It is composed of two arguments:

    - Element: A character or an item.
    - Rooms: The rooms that Thomas will be able to move in and out without the need of a key.


3. Equip - In this world we want Thomas to be able to pick up and equip elements that they can use.
It is composed of two arguments:

    - Element: A character or an item. Once we make the rules this element will be Thomas.
    - Element: A character or an item. Once we make the rules this element will be the items that Thomas can pick up.


4. Adjacent - This Predicate will indicate which rooms are beside each other and once we make the Rules it will allow us to make Thomas only be able to move between rooms that are beside each other.
It is composed of two arguments, however they are the same Sets:

    - Rooms: The rooms that Thomas will be able to move in and out without the need of a key.

Cepter text based code:
```
locked elements trapped : pred.

at elements rooms : pred.

equip elements elements : pred.

adjacent rooms rooms : pred.

```

Cepter Web Editor simulation :

<video width = "650" controls>
    <source src = "https://user-images.githubusercontent.com/42487202/145281698-cab4c812-1f37-4517-ab5b-c6aa63c74442.mov">
</video>

## Add Rules

Now we will be adding the rules in which our world will be working on.
First, navigate to the "Add rule" button and select it. A new menu should appear.

After that select the "+" to Add a New condition or effect. For this example we will be making 8 rules. In reality, it is very unlikely that you will be able to know the exact number of rules that you will be needed, which is when the Trial and Error method will most likely be the most useful.


Since the "game" will begin with Thomas being locked up in the "dungeon", it would be best to make the rules in the order the player will most likely need them.
As we saw in the map of concept, we know that there is a key Thomas will need to escape the dungeon, so we'll start with that.

1. ***Pick up metal key*** - This rule requires two Conditions; which, as we covered before, are the Predicates we previously made:

    - Locked: Thomas (Element) is in the locked dungeon (trapped).
    - Locked: Metal key (Element) is in the locked dungeon (trapped).

Once these conditions are filled, the result of the rule firing would be:

    - Equip: Metal key (Element) Thomas (Element).

This should roughly read as Thomas having equipped the metal key.

Lastly, check the box beside the second condition. This will make the key disappear from the locked dungeon, since Thomas has already equipped it.

2. ***Leave dungeon*** - This rule requires two Conditions:

    - Locked: Thomas (Element) is in the dungeon (trapped).
    - Equip: Metal key (Element) Thomas (Element).

This option should only appear after Thomas has equipped the metal key, which is why the predicate "equip" is in the conditions.
Once these conditions are filled, the result of the rule firing would be:

    - At: Thomas (Element) is in the Hallway (Rooms).

The remove box of the first condition should be checked, which should remove Thomas from the dungeon after the rule is fired.

3. ***Move*** - Now we finally create the rule to move Thomas between rooms, as well as use the "variable" feature of the Ceptre.
This rule requires two Conditions:

    -   At: Thomas (Element) is in room L (Rooms).
    -   Adjacent: L (Rooms) is beside L2 (Rooms).
    
In the second argument of the Predicate you will want to select the option "New variable".
A new dialogue box will appear, and you'll be able to name the Variable and name it as you wish, in this case we can just name it "L" (to indicate "location").
A variable means that any element of the set could be filling this field and the rule can still be fired. This predicate should indicate that Thomas can't move between rooms that aren't beside each other.
In the second argument of this predicate we will also add a new variable. You can, again, name it as you wish.

Once these conditions are filled, the result of the rule firing would be:

    - At: Thomas (Element) is in room L2 (Rooms).

In the end you should be able to read this rule as:
If Thomas is in room L, room L and room L2 are adjacent, then you can move Thomas to room L2.
The remove box of the first condition should be checked, which should remove Thomas from the original room (as he cannot be in two rooms at the same time)

4. ***Pick up golden key*** - This rule is very similar to the one in the first one, the only difference being that the key is in a different room.
It requires two Conditions:

    - At: Thomas (Element) is in the master bedroom (Rooms).
    - At: Golden key (Element) is in the master bedroom (Rooms).

Once these conditions are filled, the result of the rule firing would be:

    - Equip: Golden key (Element) Thomas (Element).

This should roughly read as Thomas having equipped the golden key.

Lastly, check the box beside the second condition. This will make the key disappear from the master bedroom, since Thomas has already equipped it.

5. ***Enter secret room*** - This rule can be fired after Thomas equips the golden key since we don't want them to be able to enter the secret room without said key.
It requires two Conditions:

    - At: Thomas (Element) is in the master bedroom (Rooms).
    - Equip: Golden key (Element) Thomas (Element).

Once these conditions are filled, the result of the rule firing would be:

    - Locked: Thomas (Element) is in the secret room (trapped).


Lastly, check the box beside the first condition. This will remove Thomas from the home library and into the secret room, it isn't a cloning room after all.



6. ***Take metal key*** - Now, this is the last item Thomas will need to collect to be able to escape.
It needs two Conditions:

    - Locked: Thomas (Element) is in the secret room (trapped)
    - Locked: Metal key (Element) is in the secret room (trapped)

Once these conditions are filled, the result of the rule firing would be:

    - Equip: Metal key (Element) Thomas (Element).


And finally, check the box to remove the second condition (this would mean that after the rule is fired the metal key will disappear from the room as Thomas has equipped it.)



7. ***Leave secret room*** - This rule is much simpler than the other ones since it only needs Thomas to be in the secret room without any other prerequisite.
It needs only one Condition:

    - Locked: Thomas (Element) is in the secret room (trapped)

The result would simply take Thomas back to the master bedroom:

    - At: Thomas (Element) is in the the master bedroom (Rooms).


And lastly, check the box next to the condition to remove it. This would delete Thomas from the secret room.



8. ***Leave creepy house*** - And with this rule we have reached what would be the end of the game.
It needs two Conditions:

    - At: Thomas (Element) is in the main room (Rooms).
    - Equip: Keypad code (Element) Thomas (Element).

The result would finally put Thomas on his way home, wherever that is:

    - At: Thomas (Element) is on their road home (Rooms).


And lastly, check the box next to the first condition to remove it, removing Thomas from the creepy house.

Ceptre text based code:
```
stage main = { 

pick_up_metal_key : locked thomas dungeon * locked metal_key dungeon -o locked thomas dungeon * equip metal_key thomas * ().

leave_dungeon : locked thomas dungeon * equip metal_key thomas -o equip metal_key thomas * at thomas hallway * ().

move : at thomas L * adjacent L L2 -o adjacent L L2 * at thomas L2 * ().

pick_up_golden_key : at thomas master_bedroom * at golden_key master_bedroom -o at thomas master_bedroom * equip golden_key thomas * ().

take_keypad_code : locked thomas secret_room * locked metal_key secret_room -o locked thomas secret_room * equip metal_key thomas * ().

enter_secret_room : at thomas master_bedroom * equip golden_key thomas -o equip golden_key thomas * locked thomas secret_room * ().

leave_secret_room : locked thomas secret_room -o at thomas master_bedroom * ().

leave_creepy_house : at thomas main_room * equip metal_key thomas -o equip metal_key thomas * at thomas road_home * ().

}

```

Ceptre Web editor simulation for initial 3 rules: 
<video width = "650" controls>
    <source src = "https://user-images.githubusercontent.com/42487202/145291901-2f324f5e-a0ac-4a84-b60f-f4169baa26a2.mov">
</video>


## Initial State

The Initial State is the state of execution your program will begin in.
In this example you only have to think of it in two ways:

The building of your map, which will most likely stay stagnant.
In this case, the rooms (including the locked ones) will not change at all throughout all the "playthrough."

The first position of your elements. These will change in the playthroughs, but when you start the "game" again these elements will go back to the position you put them in.
In this case, the items and Thomas will all start in specific positions, however they will be moved or mutated in the game. Once you start the game again Thomas and the items will go back to their first positions.
With that in mind we will start by making the map.
We'll start by adding the adjacent rooms.

Since we already have some rules to move into certain places (Locked rooms and the exit) we do not have to add them here; however we do need to add the rest of the rooms since the rule that applies to them uses variables (The Move rule).

First we'll select the button "Add Atom."
An atom is just the predicate that we will be using for the Initial State.

For this whole step we will be using the "Adjacent" predicate.

Next we will insert the arguments:

    - Adjacent: Hallway (Rooms) Home library (Rooms).



{% include note.html content="This only means that Thomas will be able to go into the Home library, but not from the Home library to the Hallway. To allow them to go back and forward we need to do this rule again, just switching the arguments." %}

To save time we should also use the button "Duplicate Atom" which, as the name suggests, will make a copy of the atom.
This way we only have to switch the arguments without having to select the predicate again.

Ceptre text base code:

```
context init = 
{
    adjacent hallway home_library, 
    adjacent home_library hallway,
    adjacent home_library master_bedroom,
    adjacent master_bedroom home_library,
    adjacent master_bedroom main_room,
    adjacent main_room master_bedroom
}
```
Ceptre Web editor simulation: 
<video width = "650" controls>
    <source src = "https://user-images.githubusercontent.com/42487202/145315795-95935cb0-f0f0-436e-ad87-9632c5bbdac1.mov">
</video>


Similarly, we will be putting all the elements in their place, however we know that we will be able to change their original states.

Ceptre text base code:

```
context init = 
{
    adjacent hallway home_library, 
    adjacent home_library hallway,
    adjacent home_library master_bedroom,
    adjacent master_bedroom home_library,
    adjacent master_bedroom main_room,
    adjacent main_room master_bedroom,
    at golden_key master_bedroom, 
    locked thomas dungeon,
    locked metal_key dungeon
}

```
Ceptre Web editor simulation: 
<video width = "650" controls>
    <source src = "https://user-images.githubusercontent.com/42487202/145320882-b3f5f763-7846-4c66-a69f-d317fe957dc5.mov">
</video>

## Execution

Now we can finally start our prototype and see if it works.
The first thing we need to do is click on "Start Execution", because otherwise the fireable rules won't appear in the box in the Execution tab.

Once we start the execution, we will also be able to see the atoms that we coded before in the States box.


![image](https://user-images.githubusercontent.com/42487202/145321472-1ee3363e-8286-483f-b3eb-8765bf774b23.png)

![image](https://user-images.githubusercontent.com/42487202/145321493-b222eeef-fa26-4407-b57a-c319a5d81602.png)

