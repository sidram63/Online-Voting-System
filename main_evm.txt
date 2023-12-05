# electronic voting machine
from tkinter import *

from PIL import ImageTk, Image

# from PIL import ImageTk,Image
#create a package and store other function in it
from myPackages.func import *
# import speech_recognition as sr
import pyttsx3


# image = ImageTk.PhotoImage('vote.png')

def speak_text(command):
    engine = pyttsx3.init()
    engine.say(command)
    engine.runAndWait()


root = Tk()
root.title("Electronic voting machine")
root.iconbitmap('me.ico')
root.geometry('800x500')

l1 = Label(root, text="Welcome" + '\n' + "Electronic Voting System " + '\n' + "Encrypted with Block Chains",
           font='Times 20  bold')
l1.pack()
# speak_text("Welcome to Electronic voting system prototype model, designed by rohit ,,which is encrypted with block chain protection ")




# l2 = Label(root,image=image)
# l2.pack()


def vote():
    root1 = Tk()
    root1.title('VOTER PANEL')
    root1.iconbitmap('me.ico')
    root1.geometry('800x500')
    v_f1 = Frame(root1, bd=4, relief=RIDGE)
    v_f1.pack(side='top')
    # la = Label(root1, image=image)
    # la.pack()
    v_l2 = Label(v_f1, text='Welcome Voter ' + '\n'
                            + '"Just one small positive thought in the morning can change your whole day"',
                 font='Italic '
                      '10 '
                      'bold', bd=4, relief=RIDGE)
    v_l2.pack()

    # placing buttons for sign in and register
    v_b1 = Button(v_f1, text='sign in', font='italic 10 bold', bg='green', fg='white',
                  command=lambda: voterSignin(root1, v_f1))
    v_b1.pack(pady=10)
    v_b1 = Button(v_f1, text='Register', font='italic 10 bold', bg='red', fg='white',
                  command=lambda: voterReg(root1, v_f1))
    v_b1.pack(pady=20)

    root1.mainloop()


def admin():
    root1 = Tk()
    root1.title('ADMIN PANEL')
    root1.iconbitmap('me.ico')
    root1.geometry('800x500')

    v_l2 = Label(root1, text='Welcome Admin ' + '\n'
                             + '"With great power comes great Responsibility !"',
                 font='Italic '
                      '10 '
                      'bold', bd=4, relief=RIDGE)
    v_l2.pack()
    v_l3 = Label(root1, text='Sign in with your public id ', font='Italic '
                                                                  '10 '
                                                                  'bold')
    v_l3.pack(pady=10)
    b11 = Button(root1, text='continue', bg='green', fg='white', command=lambda: admlog(root1))
    b11.pack(padx=40, pady=20)


v_f = Frame(root, bd=4, relief=RIDGE)
v_f.place(x=230, y=150)
v_l1 = Label(v_f, text='voter', font='arial 20 bold')
v_l1.pack()
b1 = Button(v_f, text='Vote', bg='red', fg='white', padx=4, pady=4, font='30', command=vote)
b1.pack(pady=10)

v_f = Frame(root, bd=4, relief=RIDGE)
v_f.place(x=460, y=150)
v_l1 = Label(v_f, text='admin', font='arial 20 bold')
v_l1.pack()
b1 = Button(v_f, text='Check', bg='black', fg='white', padx=4, pady=4, font='20', command=admin)
b1.pack(pady=10)

root.mainloop()
