# ISP
Repository for my ISP game.
import random
import pygame

pygame.init()
display = pygame.display.set_mode((400, 400))
display.fill((255, 255, 255))
pygame.display.update()
font = pygame.font.Font(None, 18)
t = None
color = (0, 0, 0)

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

def text(t, color, coords, size):
    font = pygame.font.Font(None, size)
    t = font.render(t, 1, color)
    display.blit(t, coords)
    pygame.display.update()

def conclusion():
    if health > 0 and enemyHealth <= 0:
        print("You win! Congratulations!")
    else:
        print("You lose!")
    input("Press enter to exit.")

#intro
text(
    """
Welcome! In this turn-based battle,\n you will fight against an enemy of equal strength.\n
Press 'Stats' to see your gear, info, and actions you can take against the enemy.\n
Your goal is to reduce the enemy's health to zero.\n
Press enter to continue.
    """, (0, 0, 0), (50, 50), 20)
input()
display.fill((255, 255, 255))
pygame.display.update()
#battle loop
while health > 0 and enemyHealth > 0:
    #GUI
    pygame.draw.rect(display, (128, 128, 128), (150, 25, 100, 50))
    text("Stats", (0, 0, 0), (175, 40), 20)
    pygame.draw.rect(display, (255, 0, 0), (50, 100, 100, 50))
    text("Sword", (0, 0, 0), (75, 115), 20)
    pygame.draw.rect(display, (0, 0, 255), (250, 100, 100, 50))
    text("Bow", (0, 0, 0), (275, 115), 20)
    pygame.draw.rect(display, (255, 255, 0), (50, 250, 100, 50))
    text("Shield", (0, 0, 0), (75, 265), 20)
    pygame.draw.rect(display, (0, 255, 0), (250, 250, 100, 50))
    text("Potion", (0, 0, 0), (275, 265), 20)
    pygame.display.update()

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
