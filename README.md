# ISP
Repository for my ISP game.
import random

action = None
enemyAction = None
turn = True
health = int(100)
enemyHealth = int(100)
roll = int(0)
SWORD_ACCURACY = int(90)
BOW_ACCURACY = int(45)
SWORD_DAMAGE = int(10)
BOW_DAMGE = int(20)

def sword(turn):
    #calculate enemy's health if you attack with a sword
    if turn == True:
        print("You swing at the enemy with your sword!")
        if roll <= SWORD_ACCURACY:
            enemyHealth -= SWORD_DAMAGE
            print("Hit! Enemy's health is now", enemyHealth)
        else:
            print("Miss!")
        return enemyHealth
    #calculate your health
    else:
        print("The enemy swings at you with a sword!")
        if roll <= SWORD_ACCURACY:
            health -= SWORD_DAMAGE
            print("Hit! Your health is now", health)
        else:
            print("Miss!")
        return health

def bow(turn):
    #same as sword, but calculate for a bow
    if turn == True:
        print("You fire at the enemy with your bow!")
        if roll <= BOW_ACCURACY:
            enemyHealth -= BOW_DAMAGE
            print("Hit! Enemy's health is now", enemyHealth)
        else:
            print("Miss!")
        return enemyHealth
    #calculate your health
    else:
        print("The enemy fires at you with a bow!")
        if roll <= BOW_ACCURACY:
            health -= BOW_DAMAGE
            print("Hit! Your health is now", health)
        else:
            print("Miss!")
        return health

def stats():
    #write out information about your stats
    print("Your health:", health)
    print("Enemy health:", enemyHealth)
    print(
    """
Your gear:
Sword - 10 damage, 90% accuracy
Bow - 20 damage, 45% accuracy
    """
    )

def turnSwitch(turn):
    if turn == True:
        turn = False
    else:
        turn = True
    return turn

#intro
print(
"""
Welcome to [name]! In this turn-based battle, you will fight against an enemy.
Type 'stats' to see your health and gear, or 'sword' or 'bow' to get straight into the action!
"""
)
while health > 0 and enemyHealth > 0:
    #main battle loop
    if turn == True:
        roll = random.randint(1, 100)
        action = input("Enter your action: ")
        if action == "stats":
            stats()
        elif action == "sword":
            sword(True)
            if enemyHealth > 0: 
                turnSwitch(True)
        elif action == "bow":
            bow(True)
            if enemyHealth > 0:
                turnSwitch(True)
        else:
            action = input("Invalid action. Enter again. ")
    else:
        roll = random.randint(1, 100)
        enemyAction = random.choice("sword", "bow")
        if enemyAction == sword:
            sword(False)
            turnSwitch(False)
        else:
            bow(False)
            turnSwitch(False)
#conclusion
if health > 0:
    print("You win! Congratulations!")
elif enemyHealth > 0:
    print("You lose.")
input("Press enter to exit.")
