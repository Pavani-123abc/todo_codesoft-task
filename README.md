from tkinter import *
from tkinter import messagebox

main = Tk()
main.geometry("500x500")
main.title("TO-DO-LIST")
main.config(bg="orange")
main.minsize(404, 400)
main.maxsize(600, 500)

label = Label(text='''WELCOME TO YOUR TO-DO-LIST ORGANIZER''', bg="#FFFDD0", fg="black", padx=10, pady=10,
              font="comicsansms 13 bold", borderwidth=5, relief=RIDGE)
label.pack()

entry = Entry(main, font=("Helvetica", 13), bg="#FFFDD0")
entry.pack(pady=20)

frame = Frame(main, bg="#FFFDD0")
frame.pack(side=RIGHT)
scrollbar = Scrollbar(frame)
scrollbar.pack(side=RIGHT, fill=Y)
list = Listbox(frame, width=30, height=14, font=('Helivetica', 14), borderwidth=5, relief=RIDGE, fg="black",
               selectbackground="grey", yscrollcommand=scrollbar.set)
list.pack(side=LEFT, fill=BOTH)
scrollbar.config(command=list.yview)

tasks = []


def AddTask():
    task = entry.get()
    if task == "":
        messagebox.showwarning("warning", "Enter a task to be added!")
    else:
        list.insert(END, task)
        tasks.append(task)
        WriterTasksInFile()
        entry.delete(0, END)


AddTaskButton = Button(text="Add task", bg="light pink", fg="black", command=AddTask)
AddTaskButton.pack(pady=30)


def DeleteTask():
    try:
        sindex = list.curselection()[0]
        del tasks[sindex]
        list.delete(sindex)
        WriterTasksInFile()
    except IndexError:
        messagebox.showwarning("warning", "select a task to be ddeleted.")


DeleteTaskButton = Button(text="Delete Task", bg="light green", fg="black", command=DeleteTask)
DeleteTaskButton.pack(pady=10)


def WriterTasksInFile():
    with open("list.txt", "w") as f:
        for i in tasks:
            f.write(i + "\n")


def LoadFromFile():
    try:
        with open("list.txt", "r") as f:
            for line in f:
                line = line.strip()
                tasks.append(line)
                list.insert(END, line)
    except FileNotFoundError:
        pass


def UpdateTask():
    try:
        currentindex = list.curselection()[0]
        UpdatedTask = entry.get()
        if UpdatedTask == "":
            messagebox.showwarning("warning", "Please enter changed task!")
        else:
            list.delete(currentindex)
            list.insert(currentindex, UpdatedTask)
            tasks[currentindex] = UpdatedTask
            WriterTasksInFile()
            entry.delete(0, END)
    except IndexError:
        messagebox.showwarning("warning", "Please select s task to change!")


UpdateTaskButton = Button(text="Update task", bg="light blue", fg="black", command=UpdateTask)
UpdateTaskButton.pack(pady=10)

LoadFromFile()

main.mainloop()
