#include <stdio.h>
#include <stdlib.h>
#include <time.h>

struct Player {
    char name[30];
    int health;
    int money;
    int weapon;
    int level;
    int missions;
};

void showStatus(struct Player p) {
    printf("\n========== PLAYER STATUS ==========\n");
    printf("Name       : %s\n", p.name);
    printf("Health     : %d\n", p.health);
    printf("Money      : $%d\n", p.money);
    printf("Weapon     : %d\n", p.weapon);
    printf("Level      : %d\n", p.level);
    printf("Missions   : %d\n", p.missions);
    printf("===================================\n");
}

void mission(struct Player *p) {
    int choice;
    int enemyHealth;
    int damage;

    printf("\n========== MISSION ==========\n");
    printf("1. Street Race\n");
    printf("2. Robbery Mission\n");
    printf("3. Gang Fight\n");
    printf("4. Back\n");
    printf("Choose mission: ");
    scanf("%d", &choice);

    if (choice == 4)
        return;

    if (choice == 1) {
        printf("\n🏎️ Street Race Started!\n");

        if (rand() % 2 == 0) {
            printf("You won the race!\n");
            p->money += 500;
            p->missions++;
            printf("Reward: $500\n");
        } else {
            printf("You lost the race!\n");
            p->health -= 10;
        }
    }

    else if (choice == 2) {
        printf("\n💰 Robbery Mission Started!\n");

        if (rand() % 2 == 0) {
            printf("Mission successful!\n");
            p->money += 1000;
            p->missions++;
            printf("Reward: $1000\n");
        } else {
            printf("Police caught you!\n");
            p->health -= 25;
            p->money -= 100;

            if (p->money < 0)
                p->money = 0;
        }
    }

    else if (choice == 3) {
        printf("\n⚔️ Gang Fight Started!\n");

        enemyHealth = 100;

        while (enemyHealth > 0 && p->health > 0) {
            printf("\nYour Health: %d\n", p->health);
            printf("Enemy Health: %d\n", enemyHealth);

            printf("1. Attack\n");
            printf("2. Run Away\n");
            printf("Choose: ");
            scanf("%d", &choice);

            if (choice == 2) {
                printf("You escaped!\n");
                return;
            }

            if (choice == 1) {
                damage = p->weapon * 10;
                enemyHealth -= damage;

                printf("You attacked the enemy for %d damage!\n",
                       damage);

                if (enemyHealth > 0) {
                    damage = (rand() % 20) + 5;
                    p->health -= damage;

                    printf("Enemy attacked you for %d damage!\n",
                           damage);
                }
            }
        }

        if (p->health > 0) {
            printf("\n🎉 You defeated the gang!\n");
            p->money += 1500;
            p->missions++;
            printf("Reward: $1500\n");
        } else {
            printf("\n💀 You lost the fight!\n");
        }
    }

    else {
        printf("Invalid choice!\n");
    }

    /* Level up */
    if (p->missions >= p->level * 3) {
        p->level++;
        p->health = 100;

        printf("\n⭐ LEVEL UP!\n");
        printf("You are now Level %d!\n", p->level);
        printf("Health restored to 100.\n");
    }
}

void weaponShop(struct Player *p) {
    int choice;

    printf("\n========== WEAPON SHOP ==========\n");
    printf("Your Money: $%d\n\n", p->money);

    printf("1. Pistol   - $500  - Damage 20\n");
    printf("2. Shotgun  - $1000 - Damage 40\n");
    printf("3. Rifle    - $2000 - Damage 60\n");
    printf("4. Back\n");

    printf("\nChoose weapon: ");
    scanf("%d", &choice);

    if (choice == 1) {
        if (p->money >= 500) {
            p->money -= 500;
            p->weapon = 2;
            printf("🔫 Pistol purchased!\n");
        } else {
            printf("Not enough money!\n");
        }
    }

    else if (choice == 2) {
        if (p->money >= 1000) {
            p->money -= 1000;
            p->weapon = 4;
            printf("🔫 Shotgun purchased!\n");
        } else {
            printf("Not enough money!\n");
        }
    }

    else if (choice == 3) {
        if (p->money >= 2000) {
            p->money -= 2000;
            p->weapon = 6;
            printf("🔫 Rifle purchased!\n");
        } else {
            printf("Not enough money!\n");
        }
    }

    else if (choice == 4) {
        return;
    }

    else {
        printf("Invalid choice!\n");
    }
}

void hospital(struct Player *p) {
    int cost = 300;

    printf("\n========== HOSPITAL ==========\n");
    printf("Treatment Cost: $300\n");

    if (p->health >= 100) {
        printf("You are already at full health!\n");
    }
    else if (p->money >= cost) {
        p->money -= cost;
        p->health = 100;

        printf("🏥 Treatment successful!\n");
        printf("Health restored to 100.\n");
    }
    else {
        printf("You don't have enough money!\n");
    }
}

int main() {
    struct Player player;
    int choice;

    srand(time(NULL));

    printf("========================================\n");
    printf("       🎮 MINI GTA - C EDITION 🎮       \n");
    printf("========================================\n");

    printf("\nEnter your player name: ");
    scanf("%29s", player.name);

    /* Starting values */
    player.health = 100;
    player.money = 1000;
    player.weapon = 1;
    player.level = 1;
    player.missions = 0;

    printf("\nWelcome, %s!\n", player.name);
    printf("Your city adventure begins...\n");

    do {
        printf("\n\n========== CITY MAP ==========\n");
        printf("1. 👤 Player Status\n");
        printf("2. 🎯 Missions\n");
        printf("3. 🔫 Weapon Shop\n");
        printf("4. 🏥 Hospital\n");
        printf("5. 💰 Check Money\n");
        printf("6. 🚪 Exit Game\n");
        printf("==============================\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {

            case 1:
                showStatus(player);
                break;

            case 2:
                mission(&player);
                break;

            case 3:
                weaponShop(&player);
                break;

            case 4:
                hospital(&player);
                break;

            case 5:
                printf("\nYour money: $%d\n", player.money);
                break;

            case 6:
                printf("\nThanks for playing, %s! 🎮\n",
                       player.name);
                break;

            default:
                printf("\nInvalid choice! Try again.\n");
        }

        /* Game over */
        if (player.health <= 0) {
            printf("\n================================\n");
            printf("          GAME OVER 💀\n");
            printf("================================\n");
            printf("You ran out of health.\n");
            break;
        }

    } while (choice != 6);

    return 0;
}
