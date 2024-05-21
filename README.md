# Hangman-Game-with-c-
Development of a console based hangman game with c++.
#include <iostream>
#include <ctime>
#include <cstdlib>
#include <string.h>
using namespace std;

const int MAX_LIVES = 6;
//take list of words
const char words[][20] = {
    "fast", "programming", "students", "are", "genius",
    "apple", "banana", "cherry", "dog", "elephant",
    "flower", "grape", "house", "internet", "jungle",
    "kangaroo", "lemon", "mango", "notebook", "ocean",
    "penguin", "queen", "rabbit", "sun", "tiger",
    "umbrella", "victory", "watermelon", "xylophone", "yacht",
    "zebra", "avocado", "ball", "cat", "dragon",
    "fire", "giraffe", "happiness", "island",
    "jacket", "kiwi", "lion", "mountain", "noodle",
    "octopus", "panda", "quokka", "rainbow", "sunset",
    "tornado", "unicorn", "volcano", "waffle", "x-ray",
    "yogurt", "zeppelin", "alpaca", "butterfly", "cactus",
    "dolphin", "eagle", "flamingo", "guitar", "hedgehog",
    "iguana", "jellyfish", "koala", "lighthouse", "mongoose",
    "nail", "parrot", "rhinoceros", "sailboat", "toucan",
    "vulture", "whale", "yak",  "acrobatics", "balloon",
    "caterpillar", "dandelion", "giraffe",
    "helicopter", "jaguar", "kangaroo", "lemur",
    "macaw", "ostrich", "penguin", "quetzal",
    "raccoon", "squirrel", "tiger",  "vampire",
    "wombat", "pencil", "helicopter", "television", "eraser",
	"ambulance", "beach"
};
const int numWords = sizeof(words) / sizeof(words[0]);   //find no of elements

void createlistOfWords(char secretWord[]) {
    srand(static_cast<unsigned>(time(0)));  // Seed random number generator
    int randomIndex = rand() % numWords + 1;    // Generate random index
    int i;
    for (i = 0; words[randomIndex][i] != '\0'; ++i) {
        secretWord[i] = words[randomIndex][i];   // Copy characters
    }
    secretWord[i] = '\0';  // Terminate the string with null character
}


void desicion(const char playerName[], const char secretWord[], int lives){
	if(lives==0){
		cout<<"Sorry,"<<playerName<<".You've lost. The secret word was: "<<secretWord<<endl;
	} else {
		cout<<"Congratulations,"<<playerName<<"! You have won. The secret word was: "<<secretWord<<endl;
	}
}

//draw hangman function
void drawHangman(int lives) {
    switch (lives) {
        case 0:
            cout << " +---+\n"
                 << " |   |\n"
                 << "     |\n"
                 << "     |\n"
                 << "     |\n"
                 << "     |\n"
                 << "==========\n";
            break;
        case 1:
            cout << " +---+\n"
                 << " |   |\n"
                 << " O   |\n"
                 << "     |\n"
                 << "     |\n"
                 << "     |\n"
                 << "==========\n";
            break;
        case 2:
            cout << " +---+\n"
                 << " |   |\n"
                 << " O   |\n"
                 << " |   |\n"
                 << "     |\n"
                 << "     |\n"
                 << "==========\n";
            break;
        case 3:
            cout << " +---+\n"
                 << " |   |\n"
                 << " O   |\n"
                 << "/|   |\n"
                 << "     |\n"
                 << "     |\n"
                 << "==========\n";
            break;
        case 4:
            cout << " +---+\n"
                 << " |   |\n"
                 << " O   |\n"
                 << "/|\\  |\n"
                 << "     |\n"
                 << "     |\n"
                 << "==========\n";
            break;
        case 5:
            cout << " +---+\n"
                 << " |   |\n"
                 << " O   |\n"
                 << "/|\\  |\n"
                 << "/    |\n"
                 << "     |\n"
                 << "==========\n";
            break;
        case 6:
            cout << " +---+\n"
                 << " |   |\n"
                 << " O   |\n"
                 << "/|\\  |\n"
                 << "/ \\  |\n"
                 << "     |\n"
                 << "==========\n";
            break;
    }
}
//isLetterInWord function
bool isLetterInWord(char letter, const char word[]) {
	//loop through each character in word
    for (int i = 0; word[i] != '\0'; ++i) {
    	//check if current character is equal to the target one
        if (word[i] == letter) {
        	//if found, return true
            return true;
        }
    }
    //if not found, return false
    return false;
}

//isAlphabetic function used for input validation of characters entered
bool isAlphabetic(char ch) {
    return (ch >= 'A' && ch <= 'Z') || (ch >= 'a' && ch <= 'z');
}

int main() {
    char playerName[50];
    cout << "Welcome to Hangman!" << endl;
    // Input validation loop
    while (true) {
        cout << "Enter your name: ";
        cin >> playerName;

        // Check if each character is an alphabet character
        bool isValid = true;
        for (int i = 0; playerName[i] != '\0'; ++i) {
            if (!isAlphabetic(playerName[i])) {
                isValid = false;
                break;
            }
        }

        // Display the result based on validation
        if (isValid) {
            break;  // Exit the loop if the name is valid
        } else {
            cout << "Invalid input. Name should contain only alphabetic characters." << endl;
        }
    }

    char secretWord[20];
    //call createlistOfWords Function
    createlistOfWords(secretWord);
    
    //variable decleration and initialization
    int lives = MAX_LIVES;

    cout << "Hello, " << playerName << "! Let's start the game." << endl;
    cout<<"_______________________________"<<endl;
    cout<<endl;
    
    
    //declare array
    char guessedWord[20];
    for (int i = 0; secretWord[i] != '\0'; ++i) {
        guessedWord[i] = '_'; //to hide the secret word, initialize guessed word with underscores
    }
    guessedWord[20] = '\0'; //to ensure it is null terminated
    
    //continue as long as lives are greater then zero
    while (lives > 0) {
        cout << "Secret word: " << guessedWord << "  Lives: " << lives << endl;

        char guess;
        cout << "Guess a letter: ";
        cin >> guess;               //take input of letter from user

        if (isLetterInWord(guess, secretWord)) { //call isLetterInWord function
            cout << "Good guess!" << endl;

            for (int i = 0; secretWord[i] != '\0'; ++i) {
            	//check if guessed letter matches with current character
                if (secretWord[i] == guess) {
                    guessedWord[i] = guess;   //update position with guessed letter
                }
            }
        } else {
            cout << "Wrong guess!" << endl;    //update user
                lives--;                   //else decrement by -1
        }
        
        cout<<endl;
        
        drawHangman(MAX_LIVES - lives);    //pass updated value of lives in drawHangman function
        cout<<"Currently your lives are: "<<lives<<endl;
        
        cout<<endl;
        
        cout<<"_______________________________"<<endl;
        cout<<endl;

        bool isComplete = true;            //to keep track if the word has been completely guessed and is initially true
        for (int i = 0; secretWord[i] != '\0'; ++i) {
        	//check if current position has a undescore in guessed word
            if (guessedWord[i] == '_') {
            	//if found we set isComplete flag to false
                isComplete = false;  //break loop
                break;
            }
        }

        if (isComplete) {
            cout << "Congratulations, " << playerName << "! You have won. The secret word was: " << secretWord << endl; //display to user
            break;
        }
    }

    if (lives == 0) {
        cout << "Sorry, " << playerName << ". You've lost. The secret word was: " << secretWord << endl;    //display to user if loves are zero
    }

    return 0;
}
