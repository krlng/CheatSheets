# Python CheatSheet

## Basics
#### Load data
``` python
import os

#Load classes from custom file sqlalchemy_declarative.py in same folder
from sqlalchemy_declarative import Base, Person
```

#### Analyse objects
``` python
dir() # lists created objects
vars() # dictonary of all created objects

type(myObj) or myObj.__class__ # get Class
dir(myObject) # List properties / functions of an object

_attr # private attributes
__func__ # used for builtin methods and variables
__attr # get namemangled with class name by interpreter to prevent accidental access
```

## Data Types


###Lists 
mutables, homogeneous data, iterating over list

``` python
a = [66.25, 333, 333, 1, 1234.5]
a.insert(2, -1)
print a.count(333), a.count(66.25), a.count('x') # 2 1 0

[t[0] for t in L] # get first element of each list element
[item for sublist in L for item in sublist] ## flatten then list L
```


### Tuples 
immutable, heterogeneous sequence of elements, using unpacking or indexes

``` python
t = 12345, 54321, 'hello!'
t[0] # 12345
```


### Sets 
unordered, no duplicates, 

``` python
basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
fruit = set(basket)   # create a set without duplicates
'orange' in fruit  # True

a = set('abracadabra')
b = set('alacazam')
a      # set(['a', 'r', 'b', 'c', 'd']) unique letters in a
a - b  # set(['r', 'd', 'b']) letters in a but not in b
```

### Dicitionaries (dict)
unordered set of key: value pairs, indexed by keys (any immutable type), 

``` python
tel = {'jack': 4098, 'sape': 4139}
tel['guido'] = 4127
del tel['sape']
tel.keys() # ['guido', 'jack']
```

### Structs

``` python

```

## Basics


``` python
str(myObj) # return readable explanation of object
repr(myObj) # return unambiguous explanation of object


price = 100.50
price * tax # >> 12.5625
price + _ # >> 113.0625 (adds the last printed value)

3 * 'un' + 'ium' # 'unununium' ( 3 times 'un', followed by 'ium')
'Py' 'thon' # 'Python' (automatically concatting strings next to each other)

mylist[::2] # every second
mylist[-3:-1] # third last till last

print '{} is "{!r}, almost {0:.3f}!"'.format('Pi', math.pi, math.pi) # first format pi exactly, then using 3 digits
print "This %(verb)s a %(noun)s." % {"noun": "test", "verb": "is"}

table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
for name, phone in table.items(): 
    print '{0:10} ==> {1:10d}'.format(name, phone) # print each with a 
    
#Jack       ==>       4098
#Dcab       ==>       7678
#Sjoerd     ==>       4127

for i, letter in enumerate(range(a:e))
	print(i+"->"+letter)

strString = """This is
a multiline
string."""

for number in rangelist:
    # Check if number is one of
    # the numbers in the tuple.
    if number in (3, 4, 7, 9):
        # "Break" terminates a for without
        # executing the "else" clause.
        break
    else:
        # "Continue" starts the next iteration
        # of the loop. It's rather useless here,
        # as it's the last statement of the loop.
        continue
else:
    # The "else" clause is optional and is
    # executed only if the loop didn't "break".
    pass # Do nothing
```

## OS
###display images
```py
for image in os.listdir(folder)
	image_file = os.path.join(folder, image)
	display(Image(filename= image_file))
```


### Loops
``` python


for q, a in zip(questions, answers):
    print 'What is your {0}?  It is {1}.'.format(q, a)
```

###json

``` python
with open('relations.json') as data_file:    
 data = json.load(data_file)
```

```` python
def executeCmd (cur,cmd):
	try:
		cur.execute(cmd)
		print cur.getSchema()
	    #Fetch table results
		for i in cur.fetch():
			print i
	except Exception as inst:
		print type(inst)     # the exception instance
		print inst.args      # arguments stored in .args
		print inst           # __str__ allows args to be printed 
````

## IPython


```` python
variableName? # object inspector
%run ipython_script_test.py #run arbitrary scripts from within IPython
%paste OR %cpaste # to paste longer scripts using multiline-breaks and tabs
%debug OR %pdb # Post-Mortem Debugging (once OR allways)
%run -d -b12 myscript # place breakpoint at line 12

%magic # Shows all Magic commands like timeit
myVar = !cmd # run in system shell and store result in variable
%alias test_alias (cd ch08; ls; cd ..) # define alias
%bookmark db /home/wesm/Dropbox/ # Create a bookmark 
````

## SqlAlchemy

```python

class Person(Base):
    __tablename__ = 'person'
    # Here we define columns for the table person
    # Notice that each column is also a normal Python instance attribute.
    id = Column(Integer, primary_key=True)
    name = Column(String(250), nullable=False)
    
#Create an engine that stores data in the local directory's
# sqlalchemy_example.db file.
engine = create_engine('sqlite:///sqlalchemy_example.db')
 
# Create all tables in the engine. This is equivalent to "Create Table"
# statements in raw SQL.
Base.metadata.create_all(engine)


from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
 
from sqlalchemy_declarative import Address, Base, Person
 
engine = create_engine('sqlite:///sqlalchemy_example.db')
# Bind the engine to the metadata of the Base class so that the
# declaratives can be accessed through a DBSession instance
Base.metadata.bind = engine
 
DBSession = sessionmaker(bind=engine)
# A DBSession() instance establishes all conversations with the database
# and represents a "staging zone" for all the objects loaded into the
# database session object. Any change made against the objects in the
# session won't be persisted into the database until you call
# session.commit(). If you're not happy about the changes, you can
# revert all of them back to the last commit by calling
# session.rollback()
session = DBSession()
 
# Insert a Person in the person table
new_person = Person(name='new person')
session.add(new_person)
session.commit()
 
# Insert an Address in the address table
new_address = Address(post_code='00000', person=new_person)
session.add(new_address)
session.commit()

from sqlalchemy_declarative import Person, Base, Address
from sqlalchemy import create_engine
engine = create_engine('sqlite:///sqlalchemy_example.db')
Base.metadata.bind = engine
from sqlalchemy.orm import sessionmaker
DBSession = sessionmaker()
DBSession.bind = engine
session = DBSession()

# Make a query to find all Persons in the database
session.query(Person).all()

# Return the first Person from all Persons in the database
person = session.query(Person).first()
person.name
u'new person'

# Find all Address whose person field is pointing to the person object
session.query(Address).filter(Address.person == person).all()
[<sqlalchemy_declarative.Address object at 0x2ee3cd0>]

session.query(Address).filter(Address.person == person).one()

address = session.query(Address).filter(Address.person == person).one()
address.post_code
u'00000'
```