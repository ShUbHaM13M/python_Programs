# my first GUI app
# Feel free to explore and a feedback would be apreciated
# credits: Tutorialspoint and Priyanka ma'am 
# --Maurya Shubham
# ThankYou

from tkinter import *
from tkinter import Menu
from tkinter import messagebox
# from tkinter import tkFileDialog
import os

def get_input():

    filename = raw_filename.get()
    if not raw_filename.get():
        messagebox.showwarning('Filename required','Filename has not been entered')
    input_data = raw_data.get('1.0','end-1c')
    os.chdir('D:/User_data/'+login_Username+'/User_Notes')
    file = open(filename,'w')
    file.write(input_data)
    file.close()
    messagebox.showinfo('Saved','Your file '+filename+'  has been saved successfully')

def createNotes():
    
    global raw_filename
    global raw_data
    raw_filename = StringVar()

    notes_window = Toplevel(mainScreen)
    notes_window.geometry('700x450')

    Label(notes_window, text='Enter Name of your file :').pack()
    Label(notes_window, text='').pack()
    Entry(notes_window, textvariable=raw_filename,width=25).pack()
    Label(notes_window, text='').pack()
    raw_data = Text(notes_window, height=19)
    raw_data.pack()
    Label(notes_window, text='').pack()
    Button(notes_window, text='Save',command=get_input,width=25,height=2,bg='cyan').pack()
    notes_window.mainloop()

def show_file():
    os.chdir('D:/User_data/'+login_Username+'/User_Notes')
    file = open(selected_File,'r')
    data = file.read()

    def exit():
        if messagebox.askokcancel('Exit ?','Do you want to exit'):
            show_file_window.destroy()

    show_file_window = Toplevel(mainScreen)
    show_file_window.geometry('320x450')
    Label(show_file_window, text = selected_File,height=2,width=300,bg='grey').pack()
    Label(show_file_window, text = '').pack()
    Label(show_file_window, text = data).pack()
    Label(show_file_window, text = '').pack()
    Button(show_file_window, text = 'Exit',bg='cyan',width=25,height=2,command=exit).pack()

def viewNotes():

    view_Screen = Toplevel(mainScreen)
    view_Screen.geometry('250x250')

    os.chdir('D:/User_data/'+login_Username+'/User_Notes')
    List_Files =(os.listdir())
    view_List = StringVar(view_Screen)
    # List_Files = ('default','second_value')
    view_List.set('Select')

    popupMenu = OptionMenu(view_Screen, view_List, *List_Files)
    Label(view_Screen,text  = 'choose the file to view').pack()
    popupMenu.pack()
    def change_file(*List_Files):
        global selected_File
        selected_File=view_List.get()
        show_file()
    Label(view_Screen, text = '').pack()
    Button(view_Screen, text='View',height=2,width=25,bg='cyan',command=change_file).pack()

def delete_file():
    if messagebox.askokcancel('Delete','Delete '+selected_File_for_delete +'?' ):
        os.chdir('D:/User_data/'+login_Username+'/User_notes')
        os.remove(selected_File_for_delete)

def deleteNotes():
    delete_Screen = Toplevel(mainScreen)
    delete_Screen.geometry('250x250')

    os.chdir('D:/User_data/'+login_Username+'/User_Notes')
    List_Files =(os.listdir())
    view_List = StringVar(delete_Screen)
    # List_Files = ('default','second_value')
    view_List.set('Select')

    popupMenu = OptionMenu(delete_Screen, view_List, *List_Files)
    Label(delete_Screen,text  = 'choose the file to view').pack()
    popupMenu.pack()
    def change_file(*List_Files):
        global selected_File_for_delete
        selected_File_for_delete=view_List.get()
        delete_file()
    Label(delete_Screen, text = '').pack()
    Button(delete_Screen, text='Delete',height=2,width=25,bg='cyan',command=change_file).pack()

def session():

    loginScreen.destroy()
    session_window = Toplevel(mainScreen)
    session_window.geometry('320x350')

    Label(session_window,text = '').pack()
    Button(session_window,text = 'Create New Note',height=2,width=25,command=createNotes,bg="cyan").pack()
    Label(session_window,text = '').pack()
    Button(session_window,text = 'View Notes',bg='cyan',height=2,width=25,command=viewNotes).pack()
    Label(session_window,text = '').pack()
    Button(session_window,text='Delete Notes',bg='cyan',height=2,width=25,command=deleteNotes).pack()


# def main_Notes():
      # still working here
#     global Notes_screen
#     Notes_screen = Tk()
#     Notes_screen.title('Notes v0.04-Beta')
#     Notes_screen.geometry('700x450')

#     def command():
#         pass

#     def Exit():
#         if messagebox.askokcancel('Quit', 'Do you really want to quit'):
#             Notes_screen.destroy()
#     menuBar = Menu(Notes_screen)
#     fileMenu = Menu(menuBar, tearoff=0)
#     menuBar.add_cascade(label='File', menu=fileMenu)
#     fileMenu.add_command(label='New', command=command)
#     fileMenu.add_command(label='Open', command=command)
#     fileMenu.add_command(label='Save', command=command)
#     fileMenu.add_separator()
#     fileMenu.add_command(label='Exit', command=Exit)

#     text = Text(Notes_screen)
#     text.pack()
#     Notes_screen.config(menu=menuBar)
#     Notes_screen.mainloop()


# def guest_login():
#     # print('continuing in guest mode')
#     main_Notes()


def login():

    global login_Username
    login_Username = Username1.get()
    login_pass = Password1.get()

    if not Password1.get() or not Username1.get():
        messagebox.showerror('Error', 'Username or Password is not entered')
    else:
        try:
            os.chdir('D:/User_data/' + login_Username)
            list_of_Files = (os.listdir())
            # print(list_of_Files)
            for i in list_of_Files:
                if login_Username + 'Enc' == i:
                    file = open(login_Username + 'Enc', 'r')
                    verify = file.read().splitlines()
                    if login_pass in verify:
                        session()
                    else:
                        messagebox.showerror('Error', 'Wrong password entered')

                # else:
                    # messagebox.showerror('Error', 'User doesnot exists')
        except FileNotFoundError:
            messagebox.showerror('Error', 'User doesnot exist''\n''please Register to continue ')


def register():
    global userName
    global password
    userName = RegUsername.get()
    password = RegPassword.get()
    if not RegPassword.get() or not RegUsername.get():
        messagebox.showerror('Error', 'Username or password is not entered')
# print(userName)
# print(password)
# os.chdir('C:/Users/admin/Desktop/Python_Programs/Data')
    else:
        try:
            os.chdir('D:/User_data')
        except:
            os.chdir('D:')
            os.mkdir('User_data')
            os.chdir('D:/User_data')
        finally:
            os.mkdir(userName)
            os.chdir('D:/User_data/' + userName)
            file = open(userName + 'Enc', 'w')
            file.write(userName + '\n')
            file.write(password)
            file.close()
            os.mkdir('User_Notes')
            os.chdir('User_Notes')
            file1 = open('default','w')
            file1.write('none')
            file1.close()
            # a = os.getcwd()
            # print(a)
            messagebox.showinfo('Registered', 'Registeration successfull')
            RegScreen.destroy()


def register_Screen():

    global RegUsername
    global RegPassword
    RegUsername = StringVar()
    RegPassword = StringVar()

    global RegScreen
    RegScreen = Toplevel(mainScreen)
    RegScreen.title('Register')
    RegScreen.geometry('320x350')

    Label(RegScreen, text='Enter details for Registeration', width=300, height=2, bg='grey', font=('calibri')).pack()
    Label(RegScreen, text='').pack()
    Label(RegScreen, text='UserName *').pack()
    Entry(RegScreen, textvariable=RegUsername, width=25).pack()
    Label(RegScreen, text='').pack()
    Label(RegScreen, text='Password *',).pack()
    Entry(RegScreen, textvariable=RegPassword, width=25).pack()
    Label(RegScreen, text='').pack()
    Button(RegScreen, text='Register', height=2, width=25, bg='cyan', command=register).pack()

    RegScreen.mainloop()


def login_Screen():
    global Username1
    global Password1
    Username1 = StringVar()
    Password1 = StringVar()

    global loginScreen
    loginScreen = Toplevel(mainScreen)
    loginScreen.title("Login")
    loginScreen.geometry('320x350')

    Label(loginScreen, text='Enter Username and password', width=300, height=2, font=('calibri'), bg='grey').pack()
    Label(loginScreen, text="").pack()
    Label(loginScreen, text='Username *').pack()
    Entry(loginScreen, textvariable=Username1, width=25).pack()
    Label(loginScreen, text="").pack()
    Label(loginScreen, text='Password *').pack()
    Entry(loginScreen, textvariable=Password1, width=25).pack()
    Label(loginScreen, text="").pack()
    Button(loginScreen, text='Login', bg='cyan', height=2, width=25, command=login).pack()

    loginScreen.mainloop()


def Notes_App():

    global mainScreen
    mainScreen = Tk()
    mainScreen.title('Notes v0.04-Beta')
    mainScreen.geometry('320x350')
    mainScreen.config(bg='cyan')

    def exit():
        if messagebox.askokcancel('Quit ?', 'Are you sure you want to exit ?'):
            mainScreen.destroy()
    Label(mainScreen, text='Notes v0.04-Beta', width=300, height=2, font=('calibri'), bg='grey').pack()
    Label(mainScreen, text='', bg='cyan').pack()
    Button(mainScreen, text='Login', height=2, width=25, command=login_Screen).pack()
    Label(mainScreen, text='', bg='cyan').pack()
    Button(mainScreen, text='Register', height=2, width=25, command=register_Screen).pack()
    Label(mainScreen, text='', bg='cyan').pack()
    # Button(mainScreen, text='Continue as Guest', height=2, width=25, command=guest_login).pack()
    Label(mainScreen, text='', bg='cyan').pack()
    Button(mainScreen, text='Exit', height=2, width=25, command=exit).pack()
    mainScreen.mainloop()


Notes_App()
