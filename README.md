# File-transfer-continued


from tkinter import *
from tkinter import filedialog as fd
import timing
import os
import sys
import time as t
import shutil as s
from datetime import datetime as dt
import sqlite3

conn = sqlite3.connect('Filecopy.db')
c= conn.cursor()



c.execute('CREATE TABLE IF NOT EXISTS FileTransfertime(ROWID INT, unix Real, datestamp TEXT)')





class fold:
    folderC = ''
    folderD = ''
    now = dt.now()


def doFileCheck():
    print('folderC: ' + f.folderC)
    print('folderD: ' + f.folderD)

    folderC = f.folderC
    folderD = f.folderD
    for thefile in os.listdir(folderC):
        modtime = dt.fromtimestamp(os.stat(folderC + thefile).st_mtime)
        diff_seconds = (f.now - modtime).seconds
        diff_days = (f.now - modtime).days
        diff_hours = diff_days*24 + diff_seconds/3600
        print('Checking: ' + thefile)
        print('Modifed, hours ago: ' + str(diff_hours))
        if diff_hours < 24:
            s.copy(folderC + thefile, folderD + thefile)
            print('Copied file: ' + thefile)
        print('')

def folderSelect(x):
    if x == 'C':
         f.folderC = fd.askdirectory() + '/'
         print('Source Folder: ' + f.folderC)
    if x == 'D':
         f.folderD = fd.askdirectory() + '/'
         print('Destination Folder: ' + f.folderD)


def saved_data_entry():
    unix = time.time()
    date = str(datetime.datetime.fronttimestamp(unix).strftime('%Y-%m-%d %H:%M:%S'))

c.execute("INSERT INTO FileTransfertime(rowid,unix,datestamp)\
         Values(?,?,?)"),(rowid,unix,datestamp))
conn.commit()

def read_from_db():
    c.execute("SELECT * FROM FileTransfertime LIMIT 1")
    data = cfetchall()
    print(data)

def buttonClicked(event):
    print saved_data_entry()
    print doFileCheck() 


root = Tk()
f = fold()

textbox=Text(root)
textbox.pack()

button1= Button(root, text="Check last File Check time, command" = read_from_db).pack()
button2= Button(root, text="Select Folder for Source", command = lambda: folderSelect('C')).pack()

button3= Button(root, text="Select Folder for Destination", command = lambda:folderSelect('D')).pack()

button4= Button(root, text="Perform File Check and Copy",command = buttonClicked).pack()

def redirector(inputStr):
    textbox.insert(INSERT, inputStr)

sys.stdout.write = redirector #whenever sys.stdout.write is called, redirector is called.

root.mainloop()
