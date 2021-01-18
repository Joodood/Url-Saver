# Url-Saver
This saves all your URLs (internet browsing tabs) in a database. Great for saving a whole list of tabs that you dont want to delete. 
from tkinter import *
import webbrowser
import sqlite3
from tkinter import ttk

root = Tk()



yt = StringVar()
yt1 = StringVar()
yt2 = StringVar()
yt3 = StringVar()
yt4 = StringVar()
yt5 = StringVar()
yt6 = StringVar()
yt7 = StringVar()




root.title("Url Saver")
root.config(bg = "palegreen")

##############################create Databae###############################
conn = sqlite3.connect('SAVETHEURL.db')
c = conn.cursor()
c.execute("""CREATE TABLE IF NOT EXISTS insiousi (
            url TEXT, \
            urlone TEXT, \
            urltwo TEXT, \
            urlthree TEXT, \
            urlfour TEXT, \
            urlfive TEXT, \
            urlsix TEXT, \
            urlseven TEXT, \
            urlname VARCHAR
)""")


conn.commit()
#conn.close()

#add scrollbar
scrollbar = ttk.Scrollbar(root,orient = HORIZONTAL)

##add listbox
listbox = Listbox(root, selectmode = MULTIPLE, yscrollcommand = scrollbar.set,font =('arial', '10', 'bold'), height = 10, width = 80)
listbox.pack()


#config scrollbar
scrollbar.config(command = listbox.xview)
scrollbar.pack(side = "top", fill = "both", pady = 10)

def save_it():
    urlzero = entry.get()
    urloneup = entry1.get()
    urltwoup = entry2.get()
    urlthreeup = entry3.get()
    urlfourup = entry4.get()
    urlfiveup = entry5.get()
    urlsixup = entry6.get()
    urlsevenup = entry7.get()
    urlentrynameup = entryname.get()
    
    All = urlentrynameup + "\n" 
    "UrlZero:" + " " + urlzero,"\n"
    "Url1: " + " " + urloneup, "\n"
    "Url2: " + " " + urltwoup, "\n"
    "Url3: " + " " + urlthreeup, "\n"
    "Url4: " + " " + urlfourup,"\n"
    "Url5: " + " " + urlfiveup, "\n"
    "Url6: " + " " + urlsixup, "\n"
    "Url7: " + " " + urlsevenup, "\n"
    
    
    
    conn = sqlite3.connect('SAVETHEURL.db')
    c = conn.cursor()
    c.execute("INSERT INTO insiousi (url, urlone, urltwo, urlthree, urlfour, urlfive, urlsix, urlseven, urlname) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)", (urlzero, urloneup, urltwoup, urlthreeup, urlfourup, urlfiveup, urlsixup, urlsevenup, urlentrynameup,))
    
    listbox.insert(0, urlentrynameup)
    #listbox.insert(0, All)
    conn.commit()
    conn.close()
    
#def add_it():
#    global new_entry
#    new_entry = Entry(root, width = 100)
#    new_entry.config(textvariable = "")
#    #new_textvar = StringVar()
#    new_entry.pack()

def open_it():
     webbrowser.open(entry.get())
     webbrowser.open(entry1.get())
     webbrowser.open(entry2.get())
     webbrowser.open(entry3.get())
     webbrowser.open(entry4.get())
     webbrowser.open(entry5.get())
     webbrowser.open(entry6.get())
     webbrowser.open(entry7.get())
#    global new_entry
#    webbrowser.open(yt.get(), new = 0, autoraise = True)
#   webbrowser.open(new_entry.get())

###instantiates oid 
def display_it():
    conn = sqlite3.connect("SAVETHEURL.db")
    c = conn.cursor()
    c.execute("SELECT *, oid FROM insiousi")
    
    for row in c.fetchall():
        listbox.insert(END, row)
        
    conn.commit()
    conn.close()

def repopulate_it():
    conn = sqlite3.connect("SAVETHEURL.db")
    c = conn.cursor()
    record_id = entry_id.get()
    c.execute("SELECT * FROM insiousi WHERE oid = " + record_id)
    records = c.fetchall()
    conn.commit()
    
    for record in records:
        entry.insert(0, record[0])
        entry1.insert(0, record[1])
        entry2.insert(0, record[2])
        entry3.insert(0, record[3])
        entry4.insert(0, record[4])
        entry5.insert(0, record[5])
        entry6.insert(0, record[6])
        entry7.insert(0, record[7])
    
#addbtn = Button(root, text = "Add an entry", command = add_it)
#addbtn.pack(side = "top")

def delete_it():
    conn = sqlite3.connect("SAVETHEURL.db")
    c = conn.cursor()
    c.execute("DELETE from insiousi WHERE oid = " + entry_id.get())
    entry_id.delete(0, END)
    #listbox.delete(0, END)
    
#    for row in c.fetchall():
#        listbox.insert(0, row)
    conn.commit()
    conn.close()
entry = Entry(root, text = "Name", width = 100, textvariable = yt)
entry.pack(pady = 5)

entry1 = Entry(root, width = 100, textvariable = yt1)
entry1.pack(pady = 3)

entry2 = Entry(root, width = 100,textvariable = yt2)
entry2.pack(pady = 3)

entry3 = Entry(root, width = 100, textvariable = yt3)
entry3.pack(pady = 3)

entry4 = Entry(root, width = 100, textvariable = yt4)
entry4.pack(pady = 3)

entry5 = Entry(root, width = 100, textvariable = yt5)
entry5.pack(pady = 3)

entry6 = Entry(root, width = 100, textvariable = yt6)
entry6.pack(pady = 3)

entry7 = Entry(root, width = 100, textvariable = yt7)
entry7.pack(pady = 3)

kev = entry.get()
print(kev)
#laugh = webbrowser.open()

down_frame = Frame(root, width = 300, bg = "Ghost White")
down_frame.pack(side = "bottom")

savebtn = Button(down_frame, text = "Save as: ", command = save_it)
savebtn.grid(row = 0, column = 0)
entryname = Entry(down_frame, width = 30)
entryname.grid(row = 0, column = 1)

openbtn = Button(root, text = "Open All", command = open_it)
openbtn.pack(side = "bottom")

#dsplaybtn = Button(root, text = "display", command = display_it)
#dsplaybtn.pack(side = "bottom")

repopulate = Button(down_frame, text = "repopulate", command = repopulate_it)
repopulate.grid()
entry_id = Entry(down_frame, width = 20)
entry_id.grid(row = 1, column = 1)
delete = Button(down_frame, text = "delete", command = delete_it)
delete.grid(row = 1, column = 2)

display_it()
root.mainloop()
