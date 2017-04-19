---
layout: post
title:  "Python : Object oriented programming"
description: "A python game to learn object oriented programming"
date:   2011-04-01
tags: [Python]
comments: true
source: "https://github.com/GitJit/py-kitchen/tree/master/apps/07.1-wizard_battle"
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Python programming : https://pythonprogramming.net/",
   "Automate boring stuff : https://automatetheboringstuff.com/",
   
]

excerpt: "In this post we will be learning object oriented programming in python by developing a simple battle game. we will cover topics like classes, inheritance, 
polymorphism etc.. in python "
---
In this post we will be learning object oriented programming in python by developing a simple battle game. we will cover topics like classes, inheritance, 
polymorphism etc...  

This is how the game works. Our hero is trapped in a forest,where there will be 
different kind of creatures who is ready to attack the hero.A creature will appear in fron of our hero and our hero can do three actions 1) Attack 2) Run 3) List all creatures.

All our actors in this game have 2 things in common. They have a name and a level. Based
on level we decides the strength of attack.Here is how we determines the winner. When
our hero attacks, we roll a dice for hero and creature. Multiply the roll value with 
the level to determine the attack strength.One with higher attack strength wins the 
battle.So based on this we can create a base class which contains the common fields
and functions. This is part of `actor.py` module.

<pre class="line-numbers" >
<code class="language-python">
# This is the base class
class Creature:
    def __init__(self, name, level):
        self.name = name
        self.level = level

    def __repr__(self):
        return "Name is {} and level is {}".format(self.name, self.level)

    def get_roll(self):
        roll = random.randint(1, 12) * self.level
        return roll
    </code>
</pre>  

`__init__` is the magic method which acts as the constructor of the class. get_roll 
is the common method that implements dice rolling. 

Now let us create the Hero class, which is inherited from the Creature base class. Hero
is a human and human's can have IQ in addition to default level. So we over-ride the 
constructor by adding an iq parameter. In addition,Hero can attack the creature,so we have added that method too.We also have to accommodate iq level when we roll for Hero,
so we over-ride the get_roll() method too as shown below. Its important to remember that
we are calling base class methods inside the over-ridden method when necessary.

<pre class="line-numbers" >
<code class="language-python">
# This class represents the Hero.
class Hero(Creature):
    def __init__(self, name, level, iq):
        super().__init__(name, level)
        self.iq = iq

    def get_roll(self):
        base_roll = super().get_roll()
        return int(base_roll * (self.iq/10))

    def attack(self, opponent):
        my_roll = self.get_roll()
        creature_roll = opponent.get_roll()
        print('Your roll is {} and {} roll is {}'.format(my_roll, opponent.name, creature_roll))
        if my_roll >= creature_roll:
            return True
        else:
            return False

</code>
</pre>    

In similar way we are also creating some creatures inheriting from the base class.

<pre class="line-numbers" >
<code class="language-python">
class Reptile(Creature):
    def __init__(self, name, level, is_poison):
        super().__init__(name, level)
        self.is_poison = is_poison

    def get_roll(self):
        base_roll = super().get_roll()
        poison_modifier = 2 if self.is_poison else 1
        return int(base_roll * poison_modifier)

</code>
</pre>    

Now let us create a Dragon and Predator respectively.

<pre class="line-numbers" >
<code class="language-python">

# This class represents a dragon
class Dragon(Creature):
    def __init__(self, name, level, breaths_fire):
        super().__init__(name, level)
        self.breaths_fire = breaths_fire

    def get_roll(self):
        base_roll = super().get_roll()
        fire_modifier = 5 if self.breaths_fire else 1

        return int(base_roll * fire_modifier )

</code>
</pre> 

Predator class is just same as the base class. We can add some methods or fields later
if needed. For time being just pass.


<pre class="line-numbers" >
<code class="language-python">

# This class represents a predator.
class Predator(Creature):
    pass
    
</code>
</pre> 

Now we are done with creating all actors in our game, now let us focus on our game loop.
The program will exit game loop if all creatures are killed by hero or if player exited
the program by entering an invalid key other than (a, r or l). We started creating our hero and array of creatures (line : 4 and 5).  
Once we enter the loop, an active creature is selected using random.choice() (line:9). The name and level of the active creature is displayed to player. Player can provide the input based on selected creature as shown in line : 12.

If hero attacks and failed, then hero is given three lives. We use thread module to sleep for 5 seconds to revitalize our hero as show in line:31.
  
<pre class="line-numbers" >
<code class="language-python">
def game_loop():

    life_count = 0;
    hero = Hero('David', 100, 50)
    creatures = [Reptile('Viper', 50, True), Predator('Tiger', 100),
                 Dragon('Dragon', 200, True)]

    while True:
        active_creature = random.choice(creatures)
        print("A {} with level {} shows up from the forest".
            format(active_creature.name, active_creature.level))
        action = input("Enter your action : [A]ttack, [R]un, [L]ook : ")
        action = action.lower()
        if action == 'a':
           result = hero.attack(active_creature)
           if result:
               print('Hero defeated the {}'.format(active_creature.name))
               creatures.remove(active_creature)
           else:
               print('{} defeated the Hero. Hero hides..'.format(active_creature.name))
               time.sleep(5)
               life_count += 1
               if life_count > 3:
                   print('No more lives .. game over!')
                   break
               else:
                   print('Hero is revitalized with rest and back in game')

        elif action == 'r':
           print('Run away..')
        elif action == 'l':
           for c in creatures:
               print(c.name)
        else:
           break
        if not creatures:
           print('You killed all creatures and game over !')
           break
    print()
    
</code>
</pre>

This our main entry  and includes..  

<pre class="line-numbers" >
<code class="language-python">
from actor import Hero, Reptile, Predator, Dragon
import random
import time

if __name__ == '__main__':
    print_header()
    game_loop()
</code>
</pre>

Output:

<pre class="line-numbers" >
<code class="language-bash">

------------------------------------
      Wizard Battle (OOP)           
------------------------------------

A Dragon with level 200 shows up from the forest
Enter your action : [A]ttack, [R]un, [L]ook : A
Your roll is 6000 and Dragon roll is 4000
Hero defeated the Dragon
A Tiger with level 100 shows up from the forest
Enter your action : [A]ttack, [R]un, [L]ook : A
Your roll is 5500 and Tiger roll is 200
Hero defeated the Tiger
A Viper with level 50 shows up from the forest
Enter your action : [A]ttack, [R]un, [L]ook : L
Viper
A Viper with level 50 shows up from the forest
Enter your action : [A]ttack, [R]un, [L]ook : A
Your roll is 3000 and Viper roll is 300
Hero defeated the Viper
You killed all creatures and game over !

</code>
</pre>




