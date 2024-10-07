This project is an example of using Redis, RediSearch + Vector Similarity Search with Python

A Useful link to python docs for redis:
https://redis.readthedocs.io/en/stable/redismodules.html 

Some useful links for redisvl:
https://github.com/RedisVentures/redisvl/tree/main 
https://github.com/RedisVentures/redisvl 

SETUP:

To run the Battleship example, which utilizes Redis Vector Similarity Search, you do not need anything but a connection to Redis running Search 2.6 or higher.  

You can create a free Redis Enterprise instance for this purpose by going to https://redis.com/try-free/   (Just be sure to select Redis 8 or Stack as the type of database you create)


## Python-preparation Steps for running the samples on your dev machine:


1. Create a virtual environment:

```
python3 -m venv redisbattleship
```

2. Activate it:  [This step is repeated anytime you want this venv back]

```
source redisbattleship/bin/activate
```

On windows you would do:

```
venv\Scripts\activate
```

3. Python will utilize this requirements.txt in the project:

```
redis>=4.3.4
redisvl>=0.0.3
etc ...
```

4. Install the libraries: [only necesary to do this one time per environment]

```
pip3 install -r requirements.txt
```
 Execute the vss_battleship.py code from the project directory.

The vss_battlehip.py code generates a vector that represents the position of a ship on a 10 x 10 grid.  You take shots at the ship targetting its anchor point.  The similiarity of the vector you generate from your anchor point is compared to the similarity of the available ships's vectors - if you are close enough or have an exact match - you hit!
Note: to make it more challenging, there are 10 quadrants and the ships are scattered across them as well.  Your search will, therefore be a hybrid query - where you select the target quadrant as well as generate a vector that represents the ships occupation of that quadrant.

You should run with all the available options enabled the first time you run the program:  (use your redis -h host and -p port and -s password)

``` 
(redisvl) owentaylor@Owens-MacBook-Pro hellovss % python3 vector_battleship.py -h redis.FIXME.url -p 10000 -s PASSWORD -c y -l y -t y -e y
```

After the first time you run the program you should set the index creation flag -c to n  (you can also choose to turn off the -e explanation)

``` 
(redisvl) owentaylor@Owens-MacBook-Pro hellovss % python3 vector_battleship.py -h FIXME.url -p 10000 -s PASSWORD -c n -l y -t y -e n
MAIN FUNCTION CALLED
user_choice for -c == n
user_choice for -l == y
user_choice for -t == y

<class 'dict'>
search_schema loaded

connecting to: host=FIXME.url port=10000 ...
loading data into redis...
 Target generated battleship has anchor_point of 27
| *  *  *  *  *  *  *  *  *  *  |
| *  *  *  *  *  *  *  *  *  *  |
| *  *  *  *  *  * [ ] *  *  *  |
| *  *  *  *  *  * [ ] *  *  *  |
| *  *  *  *  * [ ][ ][ ] *  *  |
| *  *  *  *  * [ ][ ][ ] *  *  |
| *  *  *  *  * [ ][ ][ ] *  *  |
| *  *  *  *  *  * [ ] *  *  *  |
| *  *  *  *  *  * [ ] *  *  *  |
| *  *  *  *  *  *  *  *  *  *  |
Ship shape_and_coordinates created! - here is a sample of one of the elements:
Submarine

        You are playing BATTLESHIP! 
(executing queries against the redis search index)...
We have spotted 10 ships:
Spotted: 1 Aircraft_Carrier in qudrant 7
Spotted: 1 Submarine in qudrant 7
Spotted: 1 Aircraft_Carrier in qudrant 1
Spotted: 1 Submarine in qudrant 5
Spotted: 1 Skiff in qudrant 3
Spotted: 1 Destroyer in qudrant 3
Spotted: 1 Destroyer in qudrant 1
Spotted: 1 Submarine in qudrant 9
Spotted: 1 Destroyer in qudrant 2
Spotted: 1 Submarine in qudrant 1

        Enter an integer between 2 and 8 for target X coordinate :   7
        Enter an integer between 1 and 4 for target Y coordinate :   4
        Enter an integer between 1 and 10 for target quadrant :   3
query is--> 
quadrant is 3
anchorX == 7  anchorY == 4 
 Target generated battleship has anchor_point of 47
| *  *  *  *  *  *  *  *  *  *  |
| *  *  *  *  *  *  *  *  *  *  |
| *  *  *  *  *  *  *  *  *  *  |
| *  *  *  *  *  *  *  *  *  *  |
| *  *  *  *  *  * [ ] *  *  *  |
| *  *  *  *  *  * [ ] *  *  *  |
| *  *  *  *  * [ ][ ][ ] *  *  |
| *  *  *  *  * [ ][ ][ ] *  *  |
| *  *  *  *  * [ ][ ][ ] *  *  |
| *  *  *  *  *  * [ ] *  *  *  |

Query Results have a length of 1

You HIT Something! ...:
 Document {'id': 'battleship:5dc84ae8476d4069b2c117a41e969276', 'payload': None, 'knn_dist': '5', 'battleship_class': 'Skiff', 'quadrant': '3', 'battleship_id': '753455348152000'}

Should we blast that ship out of the water (remove it from Redis)? Y/N :Y
                !!KABLOOEY!!

exiting ...
```

5. If you get errors it is most likely because you haven't configured the host and port to your redis server

6. If you get no results from your query - try again and load more battleships using this argument/flag: 

```
-l y
``````

7. when you are done exploring this set of examples you can deactivate the virtual environment:

```
source deactivate
```
