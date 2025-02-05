# Developer Documentation
**Author**
<br>
Justin Lin
___
**Tooling**
<br>
Programming Language: C++
<br>
IDE: VSCode Linux
<br>
Libraries:
- iostream
- string
- vector
- unordered_map
- unordered_set
- limits
- cstdlib
- ctime
- fstream
___
**Design**
<br>
This code is broken down into three major sections: Ecosystem controller, Map data structure, and the Organism Class hierachy. 
<br>
- The Map data structure with the help of MapTile defines the 2D area in which the Ecosystem resides. It holds the data of the location of Organisms and allows Ecosystem to access this information.
- The Organism Class hierachy containing the base class Organism and its children Plant and Animal, with Animal further being derived by Herbivore and Omnivore defines the entities that will be iteracting with the 2D environment. The Organisms themselves don't posess the power to iteract with Ecosystem, but provides the Ecosystem the information regarding the status of each Organism.
- The Ecosystem controller is the one that actually runs the simulation, holding the most power with direct access of the Map and its Organisms. It also defines how each Organism behaves during its turn within an iteration. It also provides the user with the iterface to iteract with the Ecosystem.
<br>
<br>
The strategy taken in the process of creating this application was to break down each function in smaller steps. Instead of one function that does all the needs of simulating an iteration, break it down into smaller chunks. Each being chunk being a seperate function with its purpose being to accomplish a smaller portion of the bigger application.
<br>
<br>
A major design pattern on which this application was built on was Object-Oriented Design, OOD. The Organism hierachy was created in a way where each subclass below only needs to implement extra functionality specific to their subclass and nothing more. This keeps managing these classes much easier, especially when you need to refactor a portion of the code.
<br>
<br>
A final design choice taken was readability. Variables and functions created and named in a way where it is easy to get high level understanding of what it does without anything extra. This is result in the creation of many extra methods which may only have one or two lines of code, but this lets anyone reading the code understand what is happening from the names of varables and methods rather than needing to going into depth of what each thing does.
___
**Files**
<br>
*Makefile* - the Makefile for building Binary.bin
<br>
*Ecosystem.cpp, Ecosystem.h* - Class for Ecosystem Simulator application
<br>
*Map.cpp, Map.h* - Class for 2D map datastructure of ecosystem
<br>
*MapTile.cpp, MapTile.h* - Class for the tiles of the map
<br>
*enums.h* - relevant enums, only holds SpeciesType
<br>
*util.cpp, util.h* - holds relevant utilities
<br>
*Organism.cpp, Organism.h* - Base class for the Organism hierachy
<br>
*Plant.cpp, Plant.h* - Class for the Plant object, derives Organism
<br>
*Animal.cpp, Animal.h* - Abstract class for the Animal object, derives Organism
<br>
*Herbivore.cpp, Herbivore.h* - Class for the Herbivore object, derives Organism
<br>
*Omnivore.cpp, Omnivore.h* - Class for the Omnivore object, derives Organism
___
**Methods:**
___
**Ecosystem**

- Ecosystem() - default constructor
- Ecosystem(std::string& mapFile, std::string& speciesFile) - constructor using a map file and species file
- Map getMap() - getter for m_map
- void iterate() - iterate ecosystem once
- void iterate(int steps) - iterate ecosystem a number of steps
- void populateMap(std::string& mapFile) - populate map with Organisms in correct spots
- void setUpOrganism(Organism* organism, int x, int y) - creates Organisms based off of map and species
- void createSpeciesList(std::string& speciesFile) - create a map to match each unique Organism with its ID
- void takeTurn(Organism* organism) - organism takes a turn according to its type
- void takeTurn(Plant* plant) - specifies how a Plant takes its turn
- void takeTurn(Herbivore* herbivore) - specifies how a Herbivore takes its turn
- void takeTurn(Omnivore* omnivore) - specifies how a Omnivore takes its turn
- void sortTiles(Omnivore* omnivore, std::vector<MapTile*>& food, std::vector<MapTile*>& empty) - sorts adjacent tiles of a Omnivore according to actions it can take
- void sortTiles(Herbivore* herbivore, std::vector<MapTile*>& predator, std::vector<MapTile*>& food, std::vector<MapTile*>& empty) - sorts adjacent tiles of a Herbivore according to actions it can take
- void moveAnimal(Animal* animal, MapTile* tile) - moves an Animal to another MapTile
- void tryEscape(Herbivore* prey, Omnivore* predator) - defines how a Herbivore tries to escape an Omnivore
- bool tryEscapeTo(Herbivore* prey, int x, int y) - try to escape to a specific tile, return false if unable
- Organism* parseSpecies(std::string& s) - parses text into a Organism
- void Menu(Ecosystem& ecosystem) - the user iterface to use Ecosystem Simulator
___
**Map**
- Map() - default constructor
- Map(int height, int width) - constructor taking a height and width
- MapTile* getTile(int x, int y) const - gets the MapTile at a spot, return nullptr if out of bounds
- std::vector<MapTile*> getAdjacent(int x, int y) const - returns a vector of existing adjacent tiles to a spot
- int getWidth() const - getter for map width
- int getHeight() const - getter for map height
- void print() const - prints out the map, chars represent an Organism in that tile
___
**MapTile**
- MapTile() - default constructor
- MapTile(int x, int y, Plant* plant, Animal* animal) - constructor taking in xy-coordinates and pointers to a plant and animal
- int getX() const - getter for x-coordinate
- int getY() const - getter for y-coordinate
- void setX(int x) - setter for x-coordinate
- void setY(int y) - setter for y-coordinate
- bool hasPlant() - check if MapTile contains a Plant
- bool hasAnimal() - check if MapTile contains an Animal
- bool isEmpty() - check if MapTile contains no Plant and Animal
- Plant* getPlant() - getter for Plant pointer
- Animal* getAnimal() - getter for Animal pointer
- void setPlant(Plant* plant) - setter for Plant pointer
- void setAnimal(Animal* animal) - setter for Animal pointer
___
**util**
- int mapWidth(std::string& mapFile) - returns width of map file
- int mapHeight(std::string& mapFile) - returns height of a map file
- int indexOf(std::string& s,char c) - returns index of first instance of a char in a string
- unsigned int randomNum(unsigned int ceiling) - return a random number from 0 to ceiling-1
- bool fileExist(std::string& file) - returns true if a file exists
- int enterNum() - gets a valid non-negative number from user
___
**Organism**
- Organism() - default constructor
- Organism(int x, int y, char id) - constructor taking xy-coordinates and ID
- int getX() const - getter for x-coordinate
- int getY() const - getter for y-coordinate
- void setX(int x) - setter for x-coordinate
- void setY(int y) - setter for y-coordinate
- char getID() const - getter for m_id
- virtual bool isDead() = 0 - returns true if an Organism is dead, is a virtual - function (Plants don´t "die" while Animals do)
- virtual SpeciesType getSpeciesType() = 0 - returns a enum representing a Organism's species type (plant,herbivore,or omnivore), is a virtual function
___
**Plant**
- Plant() - default constructor
- Plant(int x, int y, char id, int regrowCoef, int energyPts) - constructor with info for parent class (Organism) and regrowCoef and energyPts
- int getRegrowCoef() const - getter for m_regrowCoef
- int getEnergyPts() const - getter for m_energyPts
- int getRegrowTimer() const - getter for m_regrowTimer
- bool isEaten() const - return true if m_isEaten is true
- void grow() - Plant grows one turns, also handles if fully grown
- void wasEaten() - Plant becomes eaten and starts regrowing process
- Plant* clone() const - creates a pointer to a clone of itself
- SpeciesType getSpeciesType() override - returns SpeciesType plant, overrides parent method
- bool isDead() override - returns false (plants don´t "die"), overrides parent method
___
**Animal**
- Animal() - default constructor
- Animal(int x, int y, char id, int maxEnergy) - constructor with info for parent class (Organism) and maxEnergy
- int getEnergy() const - getter for m_energy
- int getMaxEnergy() const - getter for m_maxEnergy
- void setEnergy(int energy) - setter for m_energy
- bool canConsume(Plant* plant) const - returns true if it can consume the Plant
- void eat(Plant* plant) - Animal eats the Plant gaining energy
- bool isDead() override - return true if m_energy is zero
___
**Herbivore**
- Herbivore() - default constructor
- Herbivore(int x, int y, char id, int maxEnergy) - constructor with info for parent class (Animal)
- Herbivore* clone() const - creates a pointer to a clone of itself
- SpeciesType getSpeciesType() override - returns SpeciesType herbivore, overrides parent method
- void wasEaten() - Herbivore was eaten set m_energy to zero
___
**Omnivore**
- Omnivore() - default constructor
- Omnivore(int x, int y, char id, int maxEnergy) - constructor with info for parent class (Animal)
- Omnivore* clone() const - creates a pointer to a clone of itself
- SpeciesType getSpeciesType() override - returns SpeciesType omnivore, overrides parent method
- bool canConsume(Animal* animal) const - returns true if it can consume the Animal
- void eat(Herbivore* herbivore) - Omnivore eats the Herbivore gaining its energy
___
**Notes:**
- In my implementation the order in which Organisms take their turn is based off of when they are created, it was reverse order for me (I used enhance for loop on a unordered_set)
