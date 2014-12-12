#include <iostream>
#include <string>
#include "Name.h"
#include <cstdlib>
#include <windows.h>
#include <fstream>

using namespace std;

bool Leave();
unsigned Random(unsigned);
unsigned Gamble(unsigned);
void Stat_Update(int[],int[]);
void Win_Lose(string, int[], int[]);
void StoreName(Name&);
void ShowName(Name);
void E();

HANDLE screen= GetStdHandle(STD_OUTPUT_HANDLE); //allows me to interact with color text, declared globaly so the function can be called as well
unsigned Gamble_Unit;
unsigned Counter=0;

int main()	//main body of the program

{
	Name ENEMY;
	StoreName(ENEMY);
	ShowName(ENEMY);
	E();

	fstream InstructFile;
	string Buffer;
	InstructFile.open("Instruction.dat", ios::out);//given name for output file
	InstructFile << "To win Drop the enemys health down to 0, To lose have your health drop to 0\n\n"
		<<"Your attack must be greather than enemys defence in order to be able to attack successfully\n"
		<<"Have your attack greater than enemys defence and attack for greater damage\n\n"
		<<"Your Defence must be greather than enemys attack to be able to defend\n\n"
		<<"Your speed can be used to defend or attack depending on the stats\n"
		<<"If your speed is higher than enemys speed and attack, damage can be delt\n"
		<<"Else if speed is greater than enemys speed and defence you prepare for a counter\n\n"
		<<"Preperations are a ditch effort to increase ones own stats\n"
		<<"If both your attack and defence are lower than enemys try it out\n"
		<<"Else if your attack or defence is lower than the enemys it may help as a light boost\n\n"
		<<"Sacrifice some of your stat points to increase another\n\n"
		<<"Gamble is risky but a higher counter helps, increase all your stats and decrease your enemys\n"
		<<"that is if your successful otherwise its like a mirror effect..\n\n";
	InstructFile.close();	//closed the file

	const int SAC = 20, INC=15;		//constants declared for use in program, they will never change
	const int Your_Stats=4;//Name of the arrays with a limit of the amount of information each one has
	const int Enemy_Stats=4;
	const int Ins=19;
	int EStats[Enemy_Stats];
	int Stats[Your_Stats];
	
	int Number=0, YVal=0, BaseVal=0, Menu, SacMenu, ASacM, DSacM, SSacM;
	string Name;
	unsigned Seed;	//used for random skill generator
	bool Decide;

	Beep(540,500);//Sound effects
	Beep(580,500);
	Beep(620,500);
	
	SetConsoleTextAttribute (screen,12);	//red color added to text for looks
	
	cout <<"         Random Battle\n";	//Title of game in red text

	SetConsoleTextAttribute (screen, 7);	//return to natural white color


	cout << "Please press enter, the moment you do,\n"
		<< "points will be destributed at random,\n"
		<< "depending on what values you enter..\n";
		E();
	cout <<"But before that what is your name?:\n";
		cin >> Name;	//input name into character string
	cout<<"Well than ";
		SetConsoleTextAttribute (screen,12);
	cout << Name << "";
		SetConsoleTextAttribute (screen,7);
	cout<< " lets see if you can win..\n\n";
	cin.get();

	//random numbers must be added to obtain points for the game
	cout <<"Now input 3 numbers:\n";

	for(YVal=0; YVal < 3; YVal++)	//Will cycle untill 5 numbers that are entered from 1-9
	{
			cout << "\nNumber " << YVal+1 <<": \n";
			cin >> Seed;
			
			BaseVal=Random(Seed);
			Stats[YVal]=BaseVal;
			system("CLS");
	}

	cout <<"Now input 3 more numbers:\n";

	for(Number=0; Number < 3; Number++)	//Will cycle untill 3 numbers that are entered from 1-9
	{
			cout << "\nNumber " << Number+1 <<": \n";
			cin >> Seed;
			
			BaseVal=Random(Seed);
			EStats[Number]=BaseVal;
			system("CLS");
	}
	
			Stats[3]=10;
			EStats[3]=10;

	//So long as the Either your health or your enemy health is above 0 this will run
	while(EStats[3]>0 && Stats[3]>0){
		//Your starting stats
		SetConsoleTextAttribute (screen,9);
	
		cout <<""<< Name << " ";
	SetConsoleTextAttribute (screen,7);
	cout<< "your points are as follows:\nHealth: "<<Stats[3] <<"\nAttack: "<< Stats[0] << "\n" 
		<< "Defence: "<< Stats[1] <<"\nSpeed: "<< Stats[2] <<"\n";
		//Your enemy stats
	cout<< "\nYour Enemys points are as follows:\nHealth: "<<EStats[3]<<"\nAttack: "<< EStats[0] << "\n" 
		<< "Defence: "<< EStats[1] <<"\nSpeed: "<< EStats[2] <<"\n"
		<<"\nRound: "<<Counter<<"\n";
		//Main Menu for the game
	cout<< "\nWhat will you do?\n"
	<< "1-Attack\n"
	<< "2-Defend\n"
	<< "3-Move\n"
	<< "4-Prepare\n"
	<< "5-Power Move\n"
	<< "6-Sacrifice\n"
	<< "7-Gamble\n"
	<< "8-Game Instructions\n"
	<< "0-Give up\n\t\t\t\t\t\t";

	//input number for the menu, every one does something that will effect your stats or the enemy stats
	SetConsoleTextAttribute (screen,12);
	cin >> Menu;
	SetConsoleTextAttribute (screen,7);

	switch (Menu)
	do{
		//if 0 is pressed game will end 
	case 0:
		cout << "\nCouldent handle it ";
			SetConsoleTextAttribute (screen,12);
		cout << ""<<Name<<"";
			SetConsoleTextAttribute (screen,7);
		cout<<", be gone than.";
		E();

		return 0;

		case 1: 
			if (Stats[0] > EStats[1] && Stats[0] > EStats[0]){
				cout << "\nAttack is very effective\n";
				EStats[3]-=3;
				Stats[0]-=4;
				Stats[2]+=3;
				Stats[1]-=4;
				EStats[0]+=4;
				EStats[1]+=6;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
		

			}else if (Stats[0] > EStats[1]){
				cout << "\nAttack is effective\n";
				EStats[3]-=2;
				Stats[0]-=4;
				Stats[2]+=2;
				Stats[1]-=1;
				EStats[0]+=2;
				EStats[1]+=4;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
		
			}else{
				cout << "\nThe attack was not successfult\n";
				EStats[1]+=3;
				Stats[3]-=2;
				Stats[0]-=5;
				Stats[2]-=3;
				Stats[1]-=2;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
	}
			system("CLS");
			break;

	case 2: 
		if (Stats[1] >= EStats[0]){
				cout << "\nAttack was Defended\n";
				EStats[0]+=2;
				EStats[2]-=3;
				Stats[2]+=5;
				Stats[0]+=2;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
		
	}else{
			cout << "\nYou could not withstand the attack\n";
				Stats[3]--;
				Stats[1]-=3;
				Stats[2]+=4;
				Stats[0]+=3;
			    Stat_Update(Stats,EStats);
				Counter++;
				E();
	}
		system("CLS");
		break;

		case 3: 
		if (Stats[2] > EStats[2] && Stats[2] > EStats[0]){
				cout << "\nEnemy fell into a trap\n"
					<< "you had laid, damage was delt\n";
				Stats[0]-=3;
				Stats[1]+=4;
				Stats[2]-=7;
				EStats[3]--;
				EStats[1]+=4;
				EStats[2]+=4;
				EStats[0]+=6;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
		
		}else if (Stats[2] > EStats[2] || Stats[2] > EStats[1]){
				cout << "\nLeft defences in order to\n"
					<< "prepared for a counter attack\n";
				Stats[0]+=10;
				Stats[1]-=3;
				Stats[2]-=3;
				EStats[1]+=3;
				EStats[2]+=2;
				EStats[0]+=2;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
		

		}else{
		cout << "\nCould not counter and took damage\n";
			EStats[2]+=3;
			EStats[1]+=2;
			Stats[3]--;
			Stats[1]+=5;
			Stats[2]-=3;
			Stats[0]+=2;
			Stat_Update(Stats,EStats);
			Counter++;
			E();
		}
		system("CLS");
		break;

		case 4:
			if (Stats[0] <= EStats[1] && Stats[1] <= EStats[0]){
				cout << "\nHeavy emergency strategies are taken\n";
				EStats[3]++;
				Stats[0]+=8;
				Stats[1]+=8;
				Stats[2]+=5;
				EStats[1]+=2;
				EStats[2]+=4;
				Stat_Update(Stats,EStats);
				Counter++;
				E();

			}else if (Stats[0] <= EStats[1] || Stats[1] <= EStats[0]){
				cout << "\nFocused on light Attack and Defence strategies\n";
				EStats[3]++;
				Stats[0]+=5;
				Stats[1]+=5;
				Stats[2]-=2;
				EStats[1]+=1;
				EStats[2]+=2;
				Stat_Update(Stats,EStats);
				Counter++;
				E();

		
	}else{
		cout << "\nEnemy cought you off guard\n";
			EStats[2]-=3;
			EStats[0]+=2;
			Stats[3]--;
			Stats[1]-=3;
			Stats[2]+=6;
			Stats[0]+=2;
			Stat_Update(Stats,EStats);
			Counter++;
			E();
		}
		system("CLS");
		break;
		case 5: 
			if (Stats[0] > EStats[0] && Stats[2] > EStats[2] && Stats[1] > EStats[1]){
				cout << "\nCritical damage delt!\n";
				EStats[3]-=5;
				Stats[0]-=4;
				Stats[1]-=3;
				Stats[2]-=2;
				EStats[1]-=1;
				EStats[2]-=2;
				EStats[0]-=1;
				Stat_Update(Stats,EStats);
				Counter++;
				E();
		
	}else{
		cout << "\nAttack failed damage taken, armor damaged\n";
			EStats[2]+=3;
			EStats[1]+=3;
			Stats[3]-=2;
			Stats[1]-=5;
			Stats[2]+=6;
			Stats[0]+=6;
			Stat_Update(Stats,EStats);
			Counter++;
			E();
		}
		system("CLS");
		break;
		case 6: 
			//depending on what option you decide this menu will open a new menu
				cout << "\nWhich skill would you like to\n"
					<<"sacrifice to strengthen another?\n"
					<<"Realize ";
					SetConsoleTextAttribute (screen,12);	
				cout << ""<<Name<<"";
					SetConsoleTextAttribute (screen,7);	
				cout <<", your enemy will get\n"
					<<"slightly stronger on all stats\n"
					<<"1-Attack\n"
					<<"2-Defence\n"
					<<"3-Speed\n";
				EStats[0]+=3;
				EStats[1]+=3;
				EStats[2]+=3;
				cin >> SacMenu;
					switch (SacMenu){
					case 1:
						cout << "\nAttack.. very well into what?\n"
					<<"1-Defence\n"
					<<"2-Speed\n";
						cin >> ASacM;
						switch (ASacM){
						case 1:
							cout<< "Defence.. ok than it shall be done\n";
							Stats[0] -=SAC;//using constant to subtract to points 
							Stats[1] +=INC;//using constant to add points
							Stat_Update(Stats,EStats);
							Counter++;
							E();
							break;
							
						case 2:
							cout<< "Speed.. ok than it shall be done\n";
							Stats[0]-=SAC;
							Stats[2] -=INC;
							Stat_Update(Stats,EStats);
							Counter++;
							E();
							break;

							default : 
							cout << "Not possible\n";
								E();
								break;
						
						}break;


					case 2:
						cout << "\nDefence.. very well into what?\n"
					<<"1-Attack\n"
					<<"2-Speed\n";
						cin >> DSacM;
						switch (DSacM){
						case 1:
							cout<< "Attack.. ok than it shall be done\n";
							Stats[1] -= SAC;
							Stats[0] += INC;
							Stat_Update(Stats,EStats);
							Counter++;
							E();
							break;
						case 2:
							cout<< "Speed.. ok than it shall be done\n";
							Stats[1] -= SAC;
							Stats[2] += INC;
							Stat_Update(Stats,EStats);
							Counter++;
							E();
							break;
							default : 
							cout << "Not possible\n";
								E();
								break;
						}break;

					case 3:
						cout << "\nSpeed.. very well into what?\n"
					<<"1-Attack\n"
					<<"2-Defence\n";
						cin >> SSacM;
						switch (SSacM){
						case 1:
							cout<< "Attack.. ok than it shall be done\n";
							Stats[2] -=SAC;
							Stats[0] += INC;
							Stat_Update(Stats,EStats);
							Counter++;
							E();
							break;
						case 2:
							cout<< "Defence.. ok than it shall be done\n";
							Stats[2] -= SAC;
							Stats[1] += INC;
							Stat_Update(Stats,EStats);
							Counter++;
							E();
							break;
							default : 
							cout << "Not possible\n";
								E();
								break;

						}break;
						
					default : 
						cout << "Not possible";
						E();
						break;
						E();
					}break;

					case 7:
						cout <<"As it states this is a gamble..\n";
							Decide=Leave(); //boolean function, depending on whether it is true or false will either run the code or return to menu

						if (Decide == 1){	//if true, will continue the program
						}else if (Decide == 0){ //else if false, will end it
						cout << "Smart choice..";
						E();
						system("CLS");
						break;
}
						cout<< "Now than lets see if luck is on your side..\n"
							<<"Input a Number: ";
						cin >> Gamble_Unit;
						Gamble_Unit=Random(Gamble_Unit);
						Gamble_Unit=Gamble(Gamble_Unit);
						cout <<" Your counter is "<<Counter<<" while it seems the unit you\n"
							<<"you have gambled against is.. "<< Gamble_Unit<<"\n";
						if (Counter>Gamble_Unit){
							cout<< "Seems luck was on your side.";
							Stats[0]+=INC;
							Stats[1]+=INC;
							Stats[2]+=INC;
							EStats[0]-=SAC;
							EStats[1]-=SAC;
							EStats[2]-=SAC;
							Stat_Update(Stats,EStats);
							Counter=1;
							E();
						}else{
							cout <<"It seems luck was not on your side";
							Stats[0]-=SAC;
							Stats[1]-=SAC;
							Stats[2]-=SAC;
							EStats[0]+=INC;
							EStats[1]+=INC;
							EStats[2]+=INC;
							Stat_Update(Stats,EStats);
							Counter=1;
							E();
						}break;

					case 8:
						system("CLS");
						InstructFile.open("Instruction.dat", ios::in);
						for(BaseVal=0; BaseVal < Ins; BaseVal++)
						{
						getline(InstructFile, Buffer);
						cout << Buffer << endl;
						}
						cin.get();
						cin.get();
						InstructFile.close();
						system("CLS");
						break;
				//if a value is entered that is not permitted this will play
		default: "Not a valid option please choose another option..";
				E();
				break;

		}while(Menu!=0);

	}
	//if your health drops below or too 0 this will run
		Win_Lose(Name, Stats, EStats);
	return 0;
}

void E()
{
	
	cin.get();
	system("CLS");
}

void Stat_Update(int Stats[], int EStats[])
	//displays all updated stats after every action you take. will not return a value but cleaned up the code a bit
{
	cout << "\nStat Update:\n\n"
				<<"Enemy Health: "<< EStats[3] <<".\n"
				<< "Enemy Attack: "<< EStats[0] <<".\n"
				<< "Enemy Defence: "<< EStats[1] <<".\n"
				<< "Enemy Speed: " << EStats[2]<<"\n"
				<< "Your Health: " << Stats[3] <<".\n"
				<< "Your Attack: " << Stats[0] <<".\n"
				<< "Your Defence: " << Stats[1] << "\n"
				<< "Your Speed: " << Stats[2] << "\n";
}

void Win_Lose(string Name, int Stats[], int EStats[])
	//moved the win lose option to its own function so as to not have to mess with it anymore seeing as it is functioning
{
	int URHealth, ENHealth;
	URHealth= Stats[3];
	ENHealth= EStats[3];

	if (URHealth <=0){
				cout<<" Seems luck was not on your side ";
					SetConsoleTextAttribute (screen,12);	
				cout << ""<<Name<<"";
					SetConsoleTextAttribute (screen,7);	
				cout <<".. better luck next time";
				cin.get();
				//if your enemy drops below or too 0 this will run
			}else if(ENHealth<=0){
				cout<<"You Won, great job it seems you can defend yourself ";
					SetConsoleTextAttribute (screen,10);
				cout <<""<<Name<<"";
					SetConsoleTextAttribute (screen,7);
				cout<<" Haha.";
			cin.get();
			}

}

unsigned Random(unsigned SEED)	//random number function, used an unsigned int seeing as the sees were all unsigned
{	unsigned NewVal;
	srand(SEED);	//stores SEED value into srand 
	
	NewVal=rand()% 100+1;	//obtains a number from 1- 100 depending on what value is entered
	
	return NewVal; //will return randomized new value
}

unsigned Gamble(unsigned Gamble)	//Gamble mechanic, the first number entered will go to the first random number function
	//Than it will get another go at a smaller random number generator to make things fair
	//will return the value, and the main will decide whether it is enough to make things work or not
{
	srand(Gamble);
	Gamble=rand()%50+1;

	return Gamble;
}

bool Leave()	//boolean function with input validation, once a correct number is entered it will return it to the main and will decide what will happen depending on the result
{
	bool Decision;

	
	cout << "Before we start are"
	<< "\nyou sure you want to do this?\n\n"
	<< "1 for yes, 0 for no\n";
	cin >> Decision;
	
	if (Decision!=1 && Decision !=0){	//input validation, if the value entered is not 0 or 1 it will ask for another value
	
	cout<<"That is not a valid option please your answer\n\n";
		cin >> Decision;
}else{
	return Decision;	//will return the result of the boolean statement
	}}
	
void StoreName(Name &enemy)
{
	char name;
	cout<< "Please input a letter you dislike:\n\n";
	cin>>name;
	
	
	enemy.NameInfo(name);
}

void ShowName (Name enemy)
{
	system("CLS");
	cout<<"Well.. it seems that ";
			
	SetConsoleTextAttribute (screen,12);	//red color added to text for looks
	
	cout <<enemy.getname();

	SetConsoleTextAttribute (screen, 7);	//return to natural white color
	cout<< " has sided with Cthulu.\n"
		<<"Well let us not wait any longer lets defeat it once and for all.\n"
		<<"besides.. its not like you liked that letter anyways..\n";
	cin.get();

	
}
