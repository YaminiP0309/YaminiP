from tkinter import *
from tkinter import messagebox
from tkinter import colorchooser

# Global list to store tasks
tasks_list = []

# Function to check for input error
def inputError():
    if enterTaskField.get() == "":
        messagebox.showerror("Input Error", "Please enter a task.")
        return False
    return True

# Function to clear task number field
def clear_taskNumberField():
    taskNumberField.delete(0.0, END)

# Function to clear task entry field
def clear_taskField():
    enterTaskField.delete(0, END)

# Function to insert task
def insertTask():
    if not inputError():
        return
    content = enterTaskField.get() + "\n"
    tasks_list.append(content)
    updateTextArea()
    clear_taskField()
    messagebox.showinfo("Task Added", "Task has been added successfully!")

# Function to delete specified task
def delete():
    if not tasks_list:
        messagebox.showerror("No Task", "There are no tasks to delete.")
        return
    try:
        task_no = int(taskNumberField.get(1.0, END))
        tasks_list.pop(task_no - 1)
        updateTextArea()
        clear_taskNumberField()
    except ValueError:
        messagebox.showerror("Input Error", "Please enter a valid task number.")

# Function to update text area
def updateTextArea():
    TextArea.delete(1.0, END)
    for i, task in enumerate(tasks_list, start=1):
        TextArea.insert('end', f"[ {i} ] {task}")

# Function to change background color
def change_bg_color():
    color = colorchooser.askcolor()[1]
    gui.configure(background=color)

# Function to change text color
def change_text_color():
    color = colorchooser.askcolor()[1]
    TextArea.configure(fg=color)

# Main function
if __name__ == "__main__":
    gui = Tk()
    gui.configure(background="#f0f0f0")
    gui.title("To-Do App")

    title_label = Label(gui, text="To-Do List", bg="#404040", fg="#ffffff", font=("Arial", 30), padx=10, pady=5)
    enterTaskField = Entry(gui, font=("Arial", 14), width=40)
    submit_button = Button(gui, text="Add Task", fg="white", bg="#008080", font=("Arial", 14), command=insertTask)
    TextArea = Text(gui, font=("Arial", 14), height=15, width=50)
    taskNumberField = Text(gui, height=1, width=5, font=("Arial", 14))
    delete_button = Button(gui, text="Delete Task", fg="white", bg="#cc0000", font=("Arial", 14), command=delete)
    bg_color_button = Button(gui, text="Change BG Color", fg="white", bg="#008080", font=("Arial", 14), command=change_bg_color)
    text_color_button = Button(gui, text="Change Text Color", fg="white", bg="#008080", font=("Arial", 14), command=change_text_color)

    title_label.grid(row=0, column=0, columnspan=4, sticky="ew")
    enterTaskField.grid(row=1, column=0, columnspan=3, padx=10, pady=5)
    submit_button.grid(row=1, column=3, padx=10, pady=5)
    TextArea.grid(row=2, column=0, columnspan=4, padx=10, pady=5)
    taskNumberField.grid(row=3, column=0, padx=10, pady=5)
    delete_button.grid(row=3, column=1, padx=10, pady=5)
    bg_color_button.grid(row=3, column=2, padx=10, pady=5)
    text_color_button.grid(row=3, column=3, padx=10, pady=5)

    gui.mainloop()