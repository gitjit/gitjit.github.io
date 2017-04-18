---
layout: post
title:  "Python : A simple to do list"
description: "A simple to do list to undertand file i/o in python"
date:   2011-05-10
tags: [Python]
comments: true
source: "https://github.com/GitJit/python-core/tree/master/apps/07.1-wizard_battle"
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Python programming : https://pythonprogramming.net/",
   "Automate boring stuff : https://automatetheboringstuff.com/",
   
]

excerpt: "This post is about understanding file i/o in python. This post explains file i/o through a simple to-do list application. The topics
covered in this post includes os.path module, file reading, file writing. This post targets basic concept of file i/o hence
no exception handling is added."
---
This post is about understanding file i/o in python. This post explains file i/o through a simple to-do list application. The topics
covered in this post includes os.path module, file reading, file writing. This post targets basic concept of file i/o hence
no exception handling is added.

In real program we might be using a database like sqlite to store items, but for a simple program like this let us create a 
simple folder called db with a file in it to store the items. We want this db to reside in the same path where our program
resides. So let us start creating the db folder . 

<pre class='line-numbers'>
<code class='language-python'>

def get_db_path():
    path = os.path.abspath(os.path.join('.', 'db'))
    if not os.path.exists(path):
        os.makedirs(path)
    return path

</code>
</pre>

New entires from user is stored in a list.

<pre class='line-numbers'>
<code class='language-python'>

# journal.py
data = []

def add_entry(item):
    '''
    This function adds a new entry to to-do list
    :param item: new to do item
    :return: None
    '''
    data.append(item + "\n")
    save()

</code>
</pre>

`save()` opens the file in append mode. (a+) ensures that file get created,
if file does not exists.   

<pre class='line-numbers'>
<code class='language-python'>
# journal.py
file_name = 'data.jrl'

def save():
    '''
    This function saves the newly added to do items to data base
    :return: None
    '''
    db_folder = get_db_path()
    file = os.path.join(db_folder, file_name)
    with open(file,'a+') as fout:
        for item in data:
            fout.write(item)
    data.clear()

</code>
</pre>

load() reads the file and returns the entries to the user. We use `str.rstrip()` to
remove new lines and spaces at end of each entries.

<pre class='line-numbers'>
<code class='language-python'>
# journal.py
def load():
    '''
    This function reads the todo list db (file) and returns list of items.
    :return: items in todolist
    '''
    items = []
    db_folder = get_db_path()
    file = os.path.join(db_folder, file_name)
    if os.path.exists(file):
        with open(file, 'r') as fin:
            for item in fin.readlines():
                items.append(item.rstrip())
    return items

</code>
</pre>

The entire file operation code resides in a separate module 'journal.py' and now let us 
see the main program file.  

<pre class='line-numbers'>
<code class='language-python'>
#program.py

if __name__ == '__main__':
    print_heading()
    start_action()
    
def start_action():
    while True:
        cmd = input("Enter the command : [L]ist, [A]dd, E[X]it : ").lower().strip()
        if cmd == 'l':
            items = journal.load()
            print_items(items)
        elif cmd == 'a':
            data = input("Enter item : ")
            journal.add_entry(data)
        elif cmd == 'x':
            journal.save()
            break
        else:
            print("Invalid option{}".format(cmd))
    print('saved and exiting..')

def print_items(items):
    for index, item in enumerate(items):
        print("* {} - {}".format(index,item))

</code>
</pre>

As mentioned above,goal of this post is to share basic file i/o in python and you should add proper
exception handling in real code. 

<pre class='line-numbers'>
<code class='language-bash'>
#output
----------------------------------------
             To Do List                 
----------------------------------------

Enter the command : [L]ist, [A]dd, E[X]it : L
* 0 - Item1
Enter the command : [L]ist, [A]dd, E[X]it : A
Enter item : Item2
Enter the command : [L]ist, [A]dd, E[X]it : L
* 0 - Item1
* 1 - Item2
Enter the command : [L]ist, [A]dd, E[X]it : x
saved and exiting..
</code>
</pre>

_Coding is fun enjoy..._  

