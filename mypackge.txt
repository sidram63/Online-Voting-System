from tkinter import *
from tkinter.messagebox import showinfo
import hashlib

import mysql.connector

mydb = mysql.connector.connect(host="localhost", user="root", passwd="Master", auth_plugin="mysql_native_password")
cur = mydb.cursor()
flag = 0


def voterSignin(self, root):
    l1 = Label(root, text="Sign in your self", font='italic 10 bold', padx=30, pady=30)
    l1.pack()
    user = Label(root, text="username", font='italic 10 bold')
    user.pack()
    e1 = Entry(root)
    e1.pack()
    passw = Label(root, text="password", font='italic 10 bold')
    passw.pack()
    e2 = Entry(root)
    e2.pack()
    b1 = Button(root, text='Proceed', bg='green', fg='black', command=lambda: voteme(root, e1, e2))
    b1.pack(padx=10)


def voterReg(self, root):
    l1 = Label(root, text="Register in your self", font='italic 10 bold', padx=30, pady=30)
    l1.pack()
    user = Label(root, text="username", font='italic 10 bold')
    user.pack()
    e1 = Entry(root)
    e1.pack()
    passw = Label(root, text="password", font='italic 10 bold')
    passw.pack()
    e2 = Entry(root)
    e2.pack()
    conc = Label(root, text="Contact", font='italic 10 bold')
    conc.pack()
    e3 = Entry(root)
    e3.pack()

    b1 = Button(root, text='Proceed', bg='green', fg='black', command=lambda: regme(root, e1, e2, e3))
    b1.pack(pady=15)
    user1 = Label(root, text="", font='italic 10 bold')
    user1.pack()


def regme(self, e1, e2, e3):
    global c
    q = "select vid from votereg.votereg ;"
    cur.execute(q)
    for i in cur:
        c = i[0]
    q = "insert into votereg.votereg(vname,con,pass,vid) values(%s,%s,%s,%s);"
    cur.execute(q, (e1.get(), e3.get(), e2.get(), c + 1))
    mydb.commit()
    root2 = Tk()
    root2.title('Vote Here')
    root2.iconbitmap('me.ico')
    root2.geometry('500x200')

    l1 = Label(root2, text='Registration successfull' + '\n' + 'Your public id is' + str(c + 1),
               font='Times 20 italic bold')
    l1.pack()
    # showinfo(message='The progress completed!')
    e1.delete(0, END)
    e2.delete(0, END)
    e3.delete(0, END)


def voteme(self, e1, e2):
    global flag, vtd

    user = e1.get()
    passw = e2.get()
    e1.delete(0, END)
    e2.delete(0, END)
    cur.execute("select vname, pass from votereg.voteReg;")
    for i in cur:
        if i[0] == user and i[1] == passw:
            flag = 1
    cur.execute("select vtd from votereg.voteReg where vname =%s and pass=%s;", (user, passw))
    for i in cur:
        vtd = i[0]
    if flag == 1 and vtd == 0:
        cur.execute("update votereg.votereg set vtd = 1 where vname = %s and pass=%s;", (user, passw))
        root2 = Tk()
        root2.title('Vote Here')
        root2.iconbitmap('me.ico')
        root2.geometry('800x500')

        # arranging vote buttons
        l1 = Label(root2, text="      ")
        l1.grid(row=0, column=1)
        b1 = Button(root2, text='BJP', font='Times 20  bold', bg='red', width=10)
        b1.grid(row=1, column=1)
        b2 = Label(root2, text='   ', bg='white', width=10)
        b2.grid(row=1, column=2)
        b3 = Button(root2, text='Click to vote', font='Times 20  bold', bg='black', fg='white', width=10,
                    command=lambda: voteBJP(self, root2))
        b3.grid(row=1, column=3)

        b7 = Label(root2, text='   ')
        b7.grid(row=2, column=1)

        b4 = Button(root2, text='Cong', font='Times 20  bold', bg='green', width=10)
        b4.grid(row=3, column=1)
        b5 = Label(root2, text='   ', bg='white', width=10)
        b5.grid(row=3, column=2)
        b6 = Button(root2, text='Click to vote', font='Times 20  bold', bg='black', fg='white', width=10,
                    command=lambda: voteCONG(self, root2))
        b6.grid(row=3, column=3)

        root2.mainloop()
    else:
        root2 = Tk()
        root2.title('Vote Here')
        root2.iconbitmap('me.ico')
        root2.geometry('500x200')

        l1 = Label(root2, text='Sorry you are not authenticated or trying to vote again !', font='Times 20 italic bold')
        l1.pack()


def voteBJP(self, root):
    global count
    q = "select votes from votereg.votes where pname='BJP';"
    cur.execute(q)
    for i in cur:
        count = i[0]
    q1 = "update votereg.votes set votes=%s where pname=%s;"
    cur.execute(q1, (count + 1, 'BJP'))

    en = str(count + 1)
    result = hashlib.md5(en.encode())
    result1 = result.hexdigest()
    # result2 = result1[0:10]
    q1 = "update votereg.endata set endata=%s where pid=1"
    cur.execute(q1, (result1,))

    mydb.commit()
    root.destroy()
    root2 = Tk()
    root2.title('Vote Here')
    root2.iconbitmap('me.ico')
    root2.geometry('500x200')

    l1 = Label(root2, text='Thank you for your vote', font='Times 20 italic bold')
    l1.pack()


def voteCONG(self, root):
    global count1
    q = "select votes from votereg.votes where pname='Cong';"
    cur.execute(q)
    for i in cur:
        count1 = i[0]
    q1 = "update votereg.votes set votes=%s where pname=%s;"
    cur.execute(q1, (count1 + 1, 'Cong'))

    en = str(count1 + 1)
    result = hashlib.md5(en.encode())
    result1 = result.hexdigest()
    # result2 = result1[0:10]
    q1 = "update votereg.endata set endata=%s where pid=2"
    cur.execute(q1, (result1,))

    mydb.commit()
    root.destroy()
    root2 = Tk()
    root2.title('Vote Here')
    root2.iconbitmap('me.ico')
    root2.geometry('500x200')

    l1 = Label(root2, text='Thank you for your vote', font='Times 20 italic bold')
    l1.pack()


def admlog(root):
    l1 = Label(root, text='Public id', font='Times 10 bold')
    l1.pack()
    e1 = Entry(root)
    e1.pack()
    l2 = Label(root, text='Private id', font='Times 10 bold')
    l2.pack()
    e2 = Entry(root)
    e2.pack()

    b1 = Button(root, text='Login', bg='green', fg='white', command=lambda: admlogch(e1, e2, root))
    b1.pack(pady=20)


def admlogch(e1, e2, rme):
    global count, count1
    if e1.get() == 'master17' and e2.get() == 'master1234':

        q = "select votes from votereg.votes where pname='BJP';"
        cur.execute(q)
        for i in cur:
            count = i[0]

        result = hashlib.md5(str(count).encode())
        result1 = result.hexdigest()

        q1 = "select endata from votereg.endata where pid=1;"

        cur.execute(q1)
        for i in cur:
            tva = i[0]

        if result1 != tva:
            root3 = Tk()
            root3.title("data altered")
            root3.geometry("200x300")
            l1 = Label(root3, text="Admin the votes of BJP has been altered without intemation!")
            l1.pack()
            root3.mainloop()

        q = "select votes from votereg.votes where pname='Cong';"
        cur.execute(q)
        for i in cur:
            count1 = i[0]

        result = hashlib.md5(str(count1).encode())
        result1 = result.hexdigest()

        q1 = "select endata from votereg.endata where pid=2;"
        cur.execute(q1)
        for i in cur:
            tva = i[0]
        if result1 != tva:
            root3 = Tk()
            root3.title("data altered")
            root3.geometry("200x300")
            l1 = Label(root3, text="Admin the votes of Cong has been altered without intemation!")
            l1.pack()
            root3.mainloop()

        root4 = Tk()
        root4.title('ADMIN PANEL')
        root4.geometry('800x500')
        root4.iconbitmap('me.ico')

        l2 = Label(root4, text='')
        l2.grid(row=0, column=1)
        l2 = Label(root4)
        l2.grid(row=0, column=2)
        b1 = Button(root4, text='Check individual party votes ', font='Times 10 bold', padx=30, pady=10,
                    command=lambda: indvote(root4))
        b1.grid(row=1, column=3)

        l3 = Label(root4)
        l3.grid(row=1, column=4)

        b1 = Button(root4, text='Leading party ', font='Times 10 bold', padx=30, pady=10,
                    command=lambda: totvote(root4))
        b1.grid(row=1, column=5)

        root4.mainloop()
    else:
        l2 = Label(rme, text='you are not a authenticated admin', font='Times 10 bold')
        l2.pack()


def indvote(root):
    q = '''select votes from votereg.votes where pname='BJP';'''
    global b
    cur.execute(q)
    for i in cur:
        b = i[0]
    l3 = Label(root, text='Total votes of BJP = ' + str(b), font='Times 20 bold')
    l3.place(x=100, y=300)
    q = '''select votes from votereg.votes where pname='Cong';'''
    cur.execute(q)
    for i in cur:
        b = i[0]
    l3 = Label(root, text='Total votes of Congress = ' + str(b), font='Times 20 bold')
    l3.place(x=100, y=350)


def totvote(root):
    q = '''select votes from votereg.votes where pname='BJP';'''
    global b
    global d
    cur.execute(q)
    for i in cur:
        b = i[0]
    q = '''select votes from votereg.votes where pname='Cong';'''
    cur.execute(q)
    for i in cur:
        d = i[0]

    if b > d:
        l3 = Label(root, text='BJP is leading Congres by  ' + str(b - d), font='Times 20 bold')
        l3.place(x=100, y=350)
    elif d > b:
        l3 = Label(root, text='Congress is leading BJP by  ' + str(d - b), font='Times 20 bold')
        l3.place(x=100, y=350)
    else:
        l3 = Label(root, text='Both party are going with same votes ' + str(d), font='Times 20 bold')
        l3.place(x=100, y=350)
