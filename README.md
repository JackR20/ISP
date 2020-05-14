# ISP
Repository for my ISP game.
import random

health = int(100)
enemyHealth = int(100)
action = " "
potion = True
shot = int(0)
attack = int(0)
shield = False
enemyAction = int(0)
enemyPotion = True
enemyShield = False
tempDamage = int(0)

#intro
print(
    """
Welcome to [name]! In this turn-based battle, you will fight against an opponent of equal strength.
Type "stats" to see your gear and info!
    """
    )
#battle loop
while health > 0 and enemyHealth > 0:
    action = input("Input your action: ")
    if action == "stats":
        print("Your health:", health)
        print("Enemy health:", enemyHealth)
        print("Gear:")
        print("Sword - 10 damage, 90% accuracy")
        print("Bow - 20 damage, 45% accuracy")
        if potion == True:
            print("Potion x1 - heals 25 health")
        print("Shield - halves the damage of the enemy's next attack\n and adds it to your next attack")
        print("Available actions: stats, attack, shoot, block, potion")
    elif action == "shoot":
        shot = random.randint(1, 100)
        if shot <= 45:
            enemyHealth -= 20
            enemyHealth -= tempDamage
            print("Hit! Enemy's health:", enemyHealth, "\n")
        else:
            print("Miss! Enemy's health:", enemyHealth, "\n")
    elif action == "attack":
        attack = random.randint(1, 100)
        if attack <= 90:
            enemyHealth -= 10
            enemyHealth -= tempDamage
            print("Hit! Enemy's health:", enemyHealth, "\n")
        else:
            print("Miss! Enemy's health:", enemyHealth, "\n")
    elif action == "block":
        shield = True
        print("You ready your shield.\n")
    elif action == "potion":
        if potion == True:
            health += 25
            print("You drank your potion! Current health:", health, "\n")
            potion = False
        else:
            print("You don't have a potion!\n")
            action = " "
    else:
        print("Invalid command! Enter a new command.\n")
        action = " "
    tempDamage = 0
    #enemy turn
    if action != " " and action != "stats" and enemyHealth > 0:
        enemyAction = random.randint(1, 2)
        if enemyAction == 1:
            print("The enemy shoots at you with a bow!")
            shot = random.randint(1, 100)
            if shot <= 45:
                if shield == False:
                    health -= 20
                    print("Hit! Your health:", health, "\n")
                elif shield == True:
                    health -= 10
                    tempDamage = 10
                    print("Blocked hit! Your health:", health, "\n")
            else:
                print("Miss! Your health:", health, "\n")
        elif enemyAction == 2:
            print("The enemy swings at you with a sword!")
            attack = random.randint(1, 100)
            if attack <= 90:
                if shield == False:
                    health -= 10
                    print("Hit! Your health:", health, "\n")
                elif shield == True:
                    health -= 5
                    tempDamage = 5
                    print("Blocked hit! Your health:", health, "\n")
            else:
                print("Miss! Your health:", health, "\n")
        #else:
            #print("filler")
        shield = False
#conclusion
if health <= 0:
    input("You lose! Press enter to exit.")
if enemyHealth <=0:
    input("You win! Press enter to exit.")
