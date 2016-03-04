# 1DCombatSim
This is just for use on my C++ 1st year uni project
This should take user inputs and output a randomised simulation


#include <iostream>
#include <string>
#include <random>
#include <ctime>

using namespace std;

int main()
{
	cout << "1D 3 WAY RANDOM COMBAT SIMULATOR\n";
	cout << "***Skeletons vs Humans vs Zombies***\n\n";


	mt19937 randomEngine(time(0));
	uniform_real_distribution<float> attack(0.0, 1.0);
	uniform_int_distribution<int> enemy(1, 2);

	//make army 1 properties
	float humanAttack = 0.6;
	float maxhumanHealth = 250.0;
	float humanDamage = 200.0;
	float currentHumanHealth = maxhumanHealth;

	//make army 2 properties
	float skeletonAttack = 0.2;
	float maxskeletonHealth = 50.0;
	float skeletonDamage = 40.0;
	float currentSkeletonHealth = maxskeletonHealth;

	//make army 3 properties
	float zombieAttack = 0.4;
	float maxzombieHealth = 150.0;
	float zombieDamage = 100.0;
	float currentZombieHealth = maxzombieHealth;

	int numSkeletons, numHumans, numZombies;
	int startSkeletons, startHumans, startZombies;
	int attackEnemy;
	char turn = 'H';
	float attackResult;

	//Get the army sizes
	cout << "Enter the human army size: ";
	cin >> numHumans;
	startHumans = numHumans;
	cout << "Enter the skeleton hoard size: ";
	cin >> numSkeletons;
	startSkeletons = numSkeletons;
	cout << "Enter the zombie mass size: ";
	cin >> numZombies;
	startZombies = numZombies;

	cout << "THE BATTLE BEGINS!!!\n";
	while (numHumans > 0 && numSkeletons > 0 && numZombies > 0)
	{
		//get attackroll
		attackResult = attack(randomEngine);
		if (turn == 'H')
		{
			if (attackResult <= humanAttack)
			{
				attackEnemy = enemy(randomEngine);
				if (attackEnemy == 1)
				{
					currentSkeletonHealth -= humanDamage;
				}
				else
				{
					currentZombieHealth -= humanDamage;
				}

				if (currentSkeletonHealth < 0)
				{
					numSkeletons--;
					currentSkeletonHealth = maxskeletonHealth;
				}
				if (currentZombieHealth < 0)
				{
					numZombies--;
					currentZombieHealth = maxzombieHealth;
				}
			}
			turn = 'S';
		}
		else if (turn == 'S')
		{
			if (attackResult <= skeletonAttack)
			{
				attackEnemy = enemy(randomEngine);
				if (attackEnemy == 1)
				{
					currentHumanHealth -= skeletonDamage;
				}
				else
				{
					currentZombieHealth -= skeletonDamage;
				}
				if (currentHumanHealth < 0)
				{
					numHumans--;
					currentHumanHealth = maxhumanHealth;
				}
				if (currentZombieHealth < 0)
				{
					numZombies--;
					currentZombieHealth = maxzombieHealth;
				}
			}
			turn = 'Z';
		}
		else
		{
			if (attackResult <= zombieAttack)
			{
				attackEnemy = enemy(randomEngine);
				if (attackEnemy == 1)
				{
					currentSkeletonHealth -= zombieDamage;
				}
				else
				{
					currentHumanHealth -= zombieDamage;
				}
				if (currentHumanHealth < 0)
				{
					numHumans--;
					currentHumanHealth = maxhumanHealth;
				}
				if (currentSkeletonHealth < 0)
				{
					numSkeletons--;
					currentSkeletonHealth = maxskeletonHealth;
				}
			}
			turn = 'H';
		}
	}
	cout << "\n\nTHE BATTLE IS OVER\n\n";
	if (numHumans > numSkeletons && numHumans > numZombies)
	{
		cout << "Humans won!!!\n\n";
		cout << "There are " << numHumans << " humans left.\n";
		cout << "There are " << numZombies << " zombies left.\n";
		cout << "There are " << numSkeletons << " skeletons left.\n";
	}
	else if (numSkeletons > numHumans && numSkeletons > numZombies)
	{
		cout << "Skelteons won!!!\n\n";
		cout << "There are " << numSkeletons << " skeletons left.\n";
		cout << "There are " << numZombies << " zombies left.\n";
		cout << "There are " << numHumans << " humans left.\n";
	}
	else
	{
		cout << "Zombies won!!!\n\n";
		cout << "There are " << numZombies << " zombies left.\n";
		cout << "There are " << numSkeletons << " skeletons left.\n";
		cout << "There are " << numHumans << " humans left.\n";
	}
	cout << startHumans - numHumans << " humans and " << startSkeletons - numSkeletons << " skeletons and " << startZombies - numZombies << " zombies died\n";

	system("PAUSE");
	return 0;
}
