# Python tkinter Project:

import tkinter
from tkinter import ttk
from tkinter import filedialog, messagebox
import csv
from pathlib import Path
import os


lista =[]

#configuration for listing courses:
def Display_button():
    courses_listbox.delete(0, 'end')
    path = File_path_entry.get()
    Year = Year_combobox.get()
    Department = Department_Combobox.get()
    check_path_entryBox()
    #path='C:\\Users\\ipkolocad\\Desktop\\projekti python\\sampledata.csv'
    file_ = open(str(path), 'r', encoding="utf8", errors="ignore")
    file2list = []
    for line in file_:
        file2list.append(line)
    for i in file2list:
        if i.split()[1][0] == str(Year) and i.split()[0] == str(Department):
            courses_listbox.insert("end", i)
        if i.split()[1][0] == str(Year) and not Department:
            courses_listbox.insert("end", i)
        elif not Year and i.split()[0] == str(Department):
            courses_listbox.insert("end", i)

#configuration for checking if path entry, year and Deparmendt combo are empty
def check_path_entryBox():
    path = File_path_entry.get()
    year = Year_combobox.get()
    Department = Department_Combobox.get()
    if len(path) == 0:
        warnings_listbox.insert("end", "Please enter the path")
    elif len(year) ==0 and len(Department)==0:
        messagebox.showwarning('Error', "Chose year or Department")
    pass

#configuration for course selection and required limitations
def limit_selected_items(evt):
    event=evt.widget
    selection = event.curselection()
    value = event.get(selection)
    if ('Added ' + value.split(',')[0]) in warnings_listbox.get(0, "end"):
        tkinter.messagebox.showerror(title="Info!", message="You already chose this course!")
    elif warnings_listbox.size() == 6:
        tkinter.messagebox.showerror(title="Verejtje!", message="You can not select more than 6 courses!")
    else:
        warnings_listbox.insert("end", 'Added ' + value.split(',')[0])
        lista.append(value.replace("\n", "").split(","))


#configuration for clear button (e clear warning listbox)
def clear_button():
    warnings_listbox.delete(0,'end')

#the configuration for the Save button saves the timetable in a specified file
def save_button():
    path = File_path_entry.get()
    f = open(os.path.join(str(Path(path).parent), 'Timetable.csv'), 'a', newline="" )
    writer = csv.writer(f)
    writer.writerows(lista)
    f.close()



#GUI window configuration
window = tkinter.Tk()
window.title("file editor")

#main frame configuration
frame = tkinter.Frame(window,background="white")
frame.pack()

#frame configuration for the input part
Input_info_frame = tkinter.LabelFrame(frame, text="Requested_info",background="light grey")
Input_info_frame.grid(row=0,column=0,sticky="news",padx=10,pady=10)

#configuration of path lable and entry
File_Path_lable=tkinter.Label(Input_info_frame,text="Enter the file path:")
File_Path_lable.grid(row=0,column=0,sticky="news",padx=10,pady=10)
File_path_entry=tkinter.Entry(Input_info_frame)
File_path_entry.grid(row=0,column=1,sticky="news",padx=10,pady=10)

#Year lable and combo configuration
Year_lable=tkinter.Label(Input_info_frame,text="Year")
Year_lable.grid(row=1,column=0,pady=10,padx=10)
Year_combobox=ttk.Combobox(Input_info_frame,values=["1","2","3"])
Year_combobox.grid(row=1,column=1,sticky="news",pady=10,padx=10)

#Department label and combo configuration
Department_lable=tkinter.Label(Input_info_frame,text="Department:")
Department_lable.grid(row=1,column=2,padx=10,pady=10)
Department_Combobox=ttk.Combobox(Input_info_frame,values=["CS","EE ","UNI","ECE","EECS","ENGR","FRE", "GER", "IE"])
Department_Combobox.grid(row=1,column=3,padx=10,pady=10)

#frame configuration for Action butons
Action_buttons_frame=tkinter.LabelFrame(frame,text="Action_buttons",background="light grey")
Action_buttons_frame.grid(row=1,column=0,sticky="news",padx=10,pady=10)

#Display button configuration
Display_Button=tkinter.Button(Action_buttons_frame, text="Display",background="grey",command=Display_button)
Display_Button.grid(row=0,column=0,sticky="news",padx=10,pady=10)

#configuration for the Clear button
Clear_Button=tkinter.Button(Action_buttons_frame, text="Clear",background="grey",command=clear_button)
Clear_Button.grid(row=0,column=1,sticky="news",padx=10,pady=10)

#configuration for the Save button
Save_Button=tkinter.Button(Action_buttons_frame, text="Save",background="grey",command=save_button)
Save_Button.grid(row=0,column=3,sticky="news",padx=10,pady=10)

#lable Frame configuration for info output
Output_info_frame=tkinter.LabelFrame(frame,text="Results",background="light grey")
Output_info_frame.grid(row=2,column=0,sticky="news",padx=10,pady=10)

#Configuring the Warnings listbox
warnings_lable=tkinter.Label(Output_info_frame,text="Warnings:")
warnings_lable.grid(row=0,column=0,sticky="news",padx=10,pady=10)
warnings_listbox=tkinter.Listbox(Output_info_frame,width=60)
warnings_listbox.grid(row=1,column=0,sticky="news",pady=20,padx=20)

#Coulses listbox configuration
courses_lable=tkinter.Label(Output_info_frame,text="Courses:")
courses_lable.grid(row=0,column=1,sticky="news",padx=10,pady=10)
courses_listbox=tkinter.Listbox(Output_info_frame, width=70)
courses_listbox.grid(row=1,column=1,sticky="news",padx=20,pady=20)


courses_listbox.bind('<Double-1>',limit_selected_items)




window.mainloop()
