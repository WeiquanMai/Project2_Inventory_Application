/*
CSC237 Project2: Inventory Management Application
Student: Weiquan Mai
Due Date: April 14, 2025
Description:
Program utilizes the InventoryItem object and command-loop structure to create an inventory management application.
Command loop has the following commands:
A- Allows user to increase units for an existing InventoryItem
H- Prints help text
I- Input inventory data from a file
N- Create new inventory item
O- Output inventory data to a file
P- Print current inventory list on the screen
Q- Quit the Program
R- Reduce units from an existing InventoryItem
*/

#include <iostream>
#include <string>
#include <fstream>
#include <iomanip>
#include "InventoryItem.h"

using namespace std;

// Function Prototypes
void addParts(InventoryItem* arrayPtr, int currentRecords);
void outputHelpText();
int inputInventoryData(InventoryItem* arrayPtr, int currentRecords);
void createNewItem(InventoryItem* arrayPtr, int &currentRecords);
void outputInventoryData(InventoryItem* arrayPtr, int currentRecords);
void printInventory(InventoryItem* arrayPtr, int currentRecords);
void removeParts(InventoryItem* arrayPtr, int currentRecords);
bool quit();
bool confirmYN();
int splitLineToArray(string inputText, string inputDelim, string outputFieldArray[], int maxFields);

// Constants
const string delimiter = "|";
const int ARRAY_SIZE = 100;

int main()
{
	// Variables
	string command;
	bool keepRunning = true;
	InventoryItem inventory[ARRAY_SIZE];
	InventoryItem* arrayPtr = inventory;
	int currentRecords = 0;
	
	// Command loop structure to keep running program unless q is pressed
	while (keepRunning == true)
	{
		// Prompt user for command input. Use command input to control function of the program
		cout << "Command: ";
		getline(cin, command);

		// ignore empty commands
		if (command == " ") {
			cin.ignore();
		}
		else if (command == "a" || command == "A")
		{
			addParts(arrayPtr, currentRecords);
		}
		else if (command == "h" || command == "H")
		{
			outputHelpText();
		}
		else if (command == "i" || command == "I")
		{
			currentRecords += inputInventoryData(arrayPtr, currentRecords);
		}
		else if (command == "n" || command == "N")
		{
			createNewItem(arrayPtr, currentRecords);
		}
		else if (command == "o" || command == "O")
		{
			outputInventoryData(arrayPtr, currentRecords);
		}
		else if (command == "p" || command == "P")
		{
			printInventory(arrayPtr, currentRecords);
		}
		else if (command == "r" || command == "R")
		{
			removeParts(arrayPtr, currentRecords);
		}
		else if (command == "q" || command == "Q")
		{
			keepRunning = !quit();
		}
		else
		{
			cout << "Invalid command: " << command << endl;
		}
	} // end while loop

	cout << "Exiting the program.";
	return 0;
} // End of function main

/*________________________________________________________________________________________
* Function: outputHelpText
* Description: Outputs a brief summary of the user commands
*/
void outputHelpText()
{
	// Output list of supported commands
	cout << "Supported commands: " << endl;
	cout << "    a    Add parts." << endl;
	cout << "    h    print Help text." << endl;
	cout << "    i    Input inventory data from a file." << endl;
	cout << "    n    New inventory item." << endl;
	cout << "    o    Output inventory data to a file." << endl;
	cout << "    p    Print inventory list." << endl;
	cout << "    q    quit (end the program)." << endl;
	cout << "    r    Remove parts." << endl;
} // End of function outputHelpText

/*________________________________________________________________________________________
* Function: confirmYN
* Description: Ask user to confirm if they want to quit the program.
* If user enters y or Y, then the program ends.
* If user enters n or N, then the program continues running.
* If user inputs any other value, then loop disregards value and runs again.
*/
bool confirmYN()
{
	string inputString;
	bool confirmationValue = false;
	bool inputIsOk = false;
	do
	{
		// Obtain input from user. If input is y or Y, close do-while loop, and return true to quit function. If input is n or N, close do-while loop, and return false to quit function
		// For any other input, tell user that the input is invalid, and prompt for another input.
		cout << "Are you sure that you want to exit the program? (y/n)" << endl;
		getline(cin, inputString);

		if (inputString == "y" || inputString == "Y")
		{
			confirmationValue = true;
			inputIsOk = true;
		}
		else if (inputString == "n" || inputString == "N")
		{
			confirmationValue = false;
			inputIsOk = true;
		}
		else
		{
			cout << "Invalid input: " << inputString << ". Please input y or n." << endl;
		}
	} while (inputIsOk == false);
	return confirmationValue;
} // End of function confirmYN

/*________________________________________________________________________________________
* Function: quit
* Description: Calls confirmYN function to confirm if user wants to exit the program
*/
bool quit()
{
	return confirmYN();
} // End of function quit

/* ============================================================================
*  Obtained splitLineToArray function from Professor Morgan.
*
*  Function: splitLineToArray - Scan through one line of text, output fields to array.
*  Inputs:
*      inputText =  string containing one line of text.
*      inputDelim = string containing the delimiter character
*                       (separates one data field from the next).
*      maxFields =  Maximum number of fields
*  Outputs:
*      returns the number of fields found.
*      outputFieldArray[] = array of strings containing output fields
*
*  Desciption:
*      Starting at the beginning of the inputText string, scan for each
*      occurrence of the inputDelimiter character/string.  The first data field begins
*      at the starting position and ends at the position before the delimiter.
*      The SECOND data field begins at the position AFTER the delimiter, and
*      extends to the position before the NEXT ocurrence of the delimiter.
*      If no delimiter is found, the the current data field extends to the
*      end of the line.
*
*      The function that CALLS splitLineToArray determines what the delimiter
*      is, and passes it to the function in the
*                 string inputDelim
*      parameter.  The inputDelim string may have a string length of
*      1 or some value greater than 1.  The inputDelim parameter must
*      NOT be the empty string.
*
*---------------------------------------------------------------------------*/

int splitLineToArray(string inputText, string inputDelim,
	string outputFieldArray[], int maxFields)
{
	//	Local Variables:
	int nextFieldPosition = 0;		//	Start position of data field.
	int foundPosition;				//  Position where delimiter was found,
	int fieldCount = 0;		        //  Number of data fields found so far,
	bool endOfLine = false;			//	Flag -- end of input line detected,
	string dataField;				//  Copy of the data field.

	if (inputDelim == "") {
		cout << "ERROR(splitLineToArray): the inputDelim parameter "
			<< "must NOT be the empty string." << endl;
		return 0;
	}
	while (!endOfLine)
	{
		foundPosition = inputText.find(inputDelim, nextFieldPosition);
		if (foundPosition != string::npos)
		{
			// (Here if 'inputDelim' was found in the input string.)
			// Copy the data field contents from the input string to the
			// 'dataField' local variable.
			dataField = inputText.substr(nextFieldPosition, foundPosition - nextFieldPosition);
			fieldCount++;

			// Adjust 'nextFieldPosition' as preparation for finding NEXT delimiter.
			nextFieldPosition = foundPosition + inputDelim.length();
		}
		else
		{
			// (Here if the string 'find' function reached the end of the input 
			// string WITHOUT finding the inputDelim value.)
			endOfLine = true;   // (The "while-loop" will not repeat again.)

			// Set foundPosition to the LENGTH of the original inputText, to handle
			// the case where there is more text AFTER the last occurrence of the 
			// inputDelim string.
			foundPosition = inputText.length();

			// If there is text to output to the caller, use the substr function
			// (in the string class) to copy that text to the dataField local 
			// variable, and increment the fieldCount local variable.
			if (foundPosition > nextFieldPosition)
			{
				dataField = inputText.substr(nextFieldPosition,
					foundPosition - nextFieldPosition);
				fieldCount++;
			}
		}


		if (fieldCount > 0)
		{
			if (fieldCount > maxFields)
			{
				cout << "ERROR at line __LINE__: too many input fields, fieldCount="
					<< fieldCount << endl;
			}
			else
			{
				outputFieldArray[fieldCount - 1] = dataField;
			}
		}
	} // (end while)


	return fieldCount;
} // (end function splitLineToArray )

/*________________________________________________________________________________________
* Function: inputInventoryData
* Description: Asks user for an input file name, and then calls splitLineToArray function to write the data from file into an array.
* Then uses the getter functions of InventoryItems to populate InventoryItem array.
*/
int inputInventoryData(InventoryItem* arrayPtr, int currentRecords)
{
	// Variables
	string inputFileName;
	string inputBuffer;
	ifstream inputFile;
	int fieldCount = 0;
	int lineCount = 0;
	string outputFieldArray[4];

	// Output a prompt, asking user to specify the name of the input file
	cout << "Enter name of input file: ";
	getline(cin, inputFileName);
	inputFile.open(inputFileName);

	// Check for inputFile open error
	while (!inputFile)
	{
		cout << "Error opening file: " << inputFileName << endl;
		cout << "Please enter another file to be opened: ";
		getline(cin, inputFileName); // Ask user to input another file
		inputFile.open(inputFileName);
	}

	// Read the data from the file into the InventoryItem array
	while (getline(inputFile, inputBuffer)) {
		// Call splitLineToArray function to split input data into outputFieldArray
		fieldCount = splitLineToArray(inputBuffer, delimiter, outputFieldArray, ARRAY_SIZE);

		// Use outputFieldArray data to populate data in InventoryItem structure array
		arrayPtr[lineCount + currentRecords].setDescription(outputFieldArray[1]);
		arrayPtr[lineCount + currentRecords].setCost(stod(outputFieldArray[2]));
		arrayPtr[lineCount + currentRecords].setUnits(stoi(outputFieldArray[3]));

		lineCount++;
	}

	cout << lineCount << " records loaded to array." << endl;
	return lineCount;
} // End of inputInventoryData function

/*________________________________________________________________________________________
* Function: printInventory
* Description: Outputs the contents that is currently in the InventoryItem array to the screen
*/
void printInventory(InventoryItem* arrayPtr, int currentRecords)
{
	// Format heading
	cout << left;
	cout << setw(12) << "Item Num" << setw(40) << "Description" << setw(8) << "Cost" << setw(10) << "Quantity" << endl;
	cout << setw(12) << "________" << setw(40) << "___________" << setw(8) << "____" << setw(10) << "________" << endl;

	// Use for loop to step through InventoryItem array and output the contents of the array to the screen
	for (int i = 0; i < currentRecords; i++)
	{
		cout << setw(12) << i <<
			setw(40) << arrayPtr[i].getDescription() <<
			setw(8) << arrayPtr[i].getCost() <<
			setw(10) << arrayPtr[i].getUnits() << endl;
	}
	cout << currentRecords << " records." << endl;
} // End of displayInventory function

/*________________________________________________________________________________________
* Function: addParts
* Description: Asks the user to specify desired item number to add quantity to and performs input validation on the specified item number.
* Then asks the user to specify how many units to add, and perform input validation on the amount to add.
* If inputs are accepted, the InventoryItem object is modified as requested.
*/
void addParts(InventoryItem* arrayPtr, int currentRecords)
{
	// Variables
	int itemNumber = 0;
	int currentParts;
	int addParts = 0;

	// Output a prompt, asking the usesr to specify the desired item number
	cout << "Choose an item number: " << endl;

	while (true) 
	{
		cin >> itemNumber;

		// If input is invalid, reset the fail state, and remove any invalid characters
		if (cin.fail()) 
		{
			cin.clear();
			cin.ignore();
			cout << "Error: Invalid input. Please enter an integer." << endl;
		}
		// Input validation on itemNumber. Specified number must be greater than 0
		else if (itemNumber < 0)
		{
			cout << "Error: Please enter a positive integer." << endl;
		}
		// Input validation on itemNumber. Specified number must be less than current array
		else if (itemNumber > (currentRecords - 1))
		{
			cout << "Error: Please enter an integer less than " << (currentRecords - 1) << "." << endl;
		}
		else // Input is valid and accepted.
		{
			break;
		}
	}

	// Retrieve currentParts from specified item number
	currentParts = arrayPtr[itemNumber].getUnits();

	// Output a prompt asking the user to specify how many units to add
	cout << "How many parts to add?" << endl;

	while (true)
	{
		cin >> addParts;

		// If input is invalid, reset the fail state, and remove any invalid characters
		if (cin.fail())
		{
			cin.clear();
			cin.ignore();
			cout << "Error: Invalid input. Please enter an integer." << endl;
		}
		// Input validation on addParts. Specified number must be greater than 0.
		else if (addParts < 0)
		{
			cout << "Error: Please enter a positive integer." << endl;
		}
		// Input validation on addParts. addParts + currentParts cannot exceed maximum quantity of 30.
		else if ((addParts + currentParts) > 30)
		{
			cout << "Error: Adding specified number would exceed maximum quantity of 30. Please specify a smaller number." << endl;
		}
		else // Input is valid and accepted
		{
			break;
		}
	}

	// Clear any remaining unecessary inputs
	cin.clear();
	cin.ignore();

	// If there are no errors, modify the InventoryItem object as requested
	arrayPtr[itemNumber].setUnits(currentParts + addParts);
} // End of addParts function

/*________________________________________________________________________________________
* Function: removeParts
* Description: Asks the user to specify desired item number to remove quantity from, and performs input validation on the specified item number.
* Then asks user to specify how many units to remove, and performs input validation on the amount to be removed.
* If both quantities are accepted, the InventoryItem object is modified as requested.
*/
void removeParts(InventoryItem* arrayPtr, int currentRecords)
{
	// Variables
	int itemNumber;
	int currentParts;
	int removeParts;

	// Output a prompt, asking the user to specify the desired item number
	cout << "Choose an item number: ";

	while (true)
	{
		cin >> itemNumber;

		// If input is invalid, reset the fail state, and remove any invalid characters
		if (cin.fail())
		{
			cin.clear();
			cin.ignore();
			cout << "Error: Invalid input. Please enter an integer." << endl;
		}
		// Input validation on itemNumber. Specified number must be greater than 0
		else if (itemNumber < 0)
		{
			cout << "Error: Please enter a positive integer." << endl;
		}
		// Input validation on itemNumber. Specified number must be less than current array
		else if (itemNumber > (currentRecords - 1))
		{
			cout << "Error: Please enter an integer less than " << (currentRecords - 1) << "." << endl;
		}
		else // Input is valid and accepted.
		{
			break;
		}
	}

	// Retrieve currentParts from specified item number
	currentParts = arrayPtr[itemNumber].getUnits();

	// Output a prompt, asking the user to specify how many units to remove
	cout << "How many parts to remove? ";

	while (true)
	{
		cin >> removeParts;
		// If input is invalid, reset the fail state, and remove any invalid characters
		if (cin.fail())
		{
			cin.clear();
			cin.ignore();
			cout << "Error: Invalid input. Please enter an integer." << endl;
		}
		// Input validation on removeParts. Specified number must be greater than 0.
		else if (removeParts < 0)
		{
			cout << "Error: Please enter a positive integer." << endl;
		}
		// Input validation on removeParts. Specified number must be less than currentParts in item.
		else if (removeParts > currentParts)
		{
			cout << "Error: Cannot remove more parts than the item currently holds." << endl;
		}
		else // Input is valid and accepted.
		{
			break;
		}
	}

	// Clear any remaining unecessary inputs
	cin.clear();
	cin.ignore();

	// If there are no errors, modify the InventoryItem object as requested
	arrayPtr[itemNumber].setUnits(currentParts - removeParts);

} // End of removeParts function

/*________________________________________________________________________________________
* Function: createNewItem
* Description: Asks the user for input values (description, cost, quantity) for a new item. Performs input validation on quantity. Then creates the new item as specified.
*/
void createNewItem(InventoryItem* arrayPtr, int& currentRecords)
{
	// Variables
	string description;
	double cost;
	int quantity;

	// Ask user for input values for description, unit cost, and initial quantity for new InventoryItem
	cout << "Enter description for new item: ";
	getline(cin, description);
	cout << "Enter unit cost for new item: ";
	cin >> cost;
	cout << "Enter initial quantity for the new item: ";
	while (true)
	{
		cin >> quantity;

		// If input is invalid, reset the fail state, and remove any invalid characters
		if (cin.fail())
		{
			cin.clear();
			cin.ignore();
			cout << "Error: Invalid input. Please enter an integer." << endl;
		}
		// Input validation for quantity. Specified number must be >= 0 and <= 30.
		else if (quantity < 0 || quantity > 30)
		{
			cout << "Error: initial quantity for the new item must be >= zero and <= 30." << endl;
		}
		else // Input is valid and accepted
		{
			break;
		}
	}

	// Create new item with specified values
	arrayPtr[currentRecords].setDescription(description);
	arrayPtr[currentRecords].setCost(cost);
	arrayPtr[currentRecords].setUnits(quantity);
	currentRecords++;

	cout << "Announcing a new inventory item: " << description << endl;
	cout << "We now have " << currentRecords << " inventory items in stock!";
} // End of createNewItem function

/*________________________________________________________________________________________
* Function: outputInventoryData
* Description: Outputs a prompt asking the user to specify the name of the output file. Writes data from InventoryItem array to output file.
*/
void outputInventoryData(InventoryItem* arrayPtr, int currentRecords)
{
	// Variables
	string outputFileName;
	ofstream outputFile;

	// Output a prompt, asking the user to specify the name of the output file
	cout << "Enter name of output file:";
	getline(cin, outputFileName);
	outputFile.open(outputFileName);

	// Use for loop to write description, cost, and units from InventoryItem array to outputFile.
	for (int i = 0; i < currentRecords; i++)
	{
		outputFile << i << "|" << arrayPtr[i].getDescription() << "|" << arrayPtr[i].getCost() << "|" << arrayPtr[i].getUnits() << endl;
	}

	cout << currentRecords << " records written to file";
} // End of outputInventoryData function
