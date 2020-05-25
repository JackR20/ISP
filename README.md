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
tempDamage = int(0)
gear = ["Sword", "Bow", "Potion", "Shield", "Arrows"]
arrows = int(3)
enemyArrows = int(3)

SWORD_ACCURACY = int(90)
BOW_ACCURACY = int(60)
SWORD_DAMAGE = int(10)
BOW_DAMAGE = int(20)
POTION_HEALTH = int(25)

def intro():
    print(
    """
Welcome to [name]! In this turn-based battle, you will fight against an opponent of equal strength.
Type "stats" to see your gear and info!
    """
    )

def conclusion():
    if health > 0 and enemyHealth <= 0:
        print("You win! Congratulations!")
    else:
        print("You lose!")
    input("Press enter to exit.")

#intro
intro()
#battle loop
while health > 0 and enemyHealth > 0:
    action = input("Input your action: ")
    if action == "stats":
        print("Your health:", health)
        print("Enemy health:", enemyHealth)
        print("Gear:")
        for item in gear:
            print(item, end = ("; "))
        print("Sword - 10 damage, 90% accuracy")
        print("Bow - 20 damage, 60% accuracy")
        if potion == True:
            print("Potion x1 - heals 25 health")
        print("Shield - halves the damage of the enemy's next attack\n and adds it to your next attack")
        print("Available actions: stats, attack, shoot, block, potion")

    elif action == "shoot" and arrows > 0:
        shot = random.randint(1, 100)
        if shot <= BOW_ACCURACY:
            enemyHealth -= BOW_DAMAGE
            enemyHealth -= tempDamage
            print("Hit! Enemy's health:", enemyHealth, "\n")
        else:
            print("Miss! Enemy's health:", enemyHealth, "\n")
        arrows -= 1
        print("Arrows left:", arrows)
        if arrows == 0:
            del gear[len(gear) - 1]

    elif action == "attack":
        attack = random.randint(1, 100)
        if attack <= SWORD_ACCURACY:
            enemyHealth -= SWORD_DAMAGE
            enemyHealth -= tempDamage
            print("Hit! Enemy's health:", enemyHealth, "\n")
        else:
            print("Miss! Enemy's health:", enemyHealth, "\n")

    elif action == "block":
        shield = True
        print("You ready your shield.\n")

    elif action == "potion":
        if potion == True and health <= 75:
            health += POTION_HEALTH
            print("You drank your potion! Current health:", health, "\n")
            potion = False
            del gear[2]
        elif potion == True and health > 75:
            print("You haven't lost enough health to use your potion!")
            action = " "
        else:
            print("You don't have a potion!\n")
            action = " "

    else:
        print("Invalid command! Enter a new command.\n")
        action = " "
    tempDamage = 0

    #enemy turn
    if action != " " and action != "stats" and enemyHealth > 0:
        #calculate move
        if enemyHealth <= 75 and enemyPotion == True:
            enemyAction = 3
        elif enemyHealth + 20 <= health and enemyArrows > 0:
            enemyAction = 1
        elif enemyArrows > 0:
            enemyAction = random.randint(1, 2)
        else:
            enemyAction = 2

        if enemyAction == 1:
            print("The enemy shoots at you with a bow!")
            shot = random.randint(1, 100)
            if shot <= BOW_ACCURACY:
                if shield == False:
                    health -= BOW_DAMAGE
                    print("Hit! Your health:", health, "\n")
                elif shield == True:
                    health -= BOW_DAMAGE / 2
                    tempDamage = BOW_DAMAGE / 2
                    print("Blocked hit! Your health:", health, "\n")
            else:
                print("Miss! Your health:", health, "\n")
            enemyArrows -= 1
            print("Enemy arrows left:", enemyArrows)

        elif enemyAction == 2:
            print("The enemy swings at you with a sword!")
            attack = random.randint(1, 100)
            if attack <= SWORD_ACCURACY:
                if shield == False:
                    health -= SWORD_DAMAGE
                    print("Hit! Your health:", health, "\n")
                elif shield == True:
                    health -= SWORD_DAMAGE / 2
                    tempDamage = SWORD_DAMAGE / 2
                    print("Blocked hit! Your health:", health, "\n")
            else:
                print("Miss! Your health:", health, "\n")
        else:
            enemyHealth += POTION_HEALTH
            enemyPotion = False
            print("The enemy drank a potion! Enemy health:", enemyHealth)
        shield = False

#conclusion
conclusion()
