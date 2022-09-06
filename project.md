#************************************ LAIBRARIES ************************************************************************
import tkinter
from tkinter import * 
from tkinter.messagebox import *
from tkinter.scrolledtext import *
from sqlite3 import *
from tkinter import ttk
from PIL import ImageTk,Image
import requests
import bs4
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

#*********************************** MAIN WINDOW ************************************************************************
mw = Tk()
mw.title("EMPLOYEE MANAGEMENT SYSTEM")
mw.geometry("950x650+250+100")
img =Image.open("C:\demo\project\ee.png")
bg = ImageTk.PhotoImage(img)
label = Label(mw, image=bg)
label.place(x = 0,y = 10)
f= ("italic	", 20, "bold")
y=10


#********************************** FUNCTIONS ****************************************************************************
def f1():
	mw.withdraw()
	ae.deiconify()
def f2():
	mw.withdraw()
	ve.deiconify()
	ve_st_data.delete(1.0,END)	
	info = ""
	con = None
	try:
		con = connect("emp.db")
		cursor = con.cursor()
		sql = "select * from employee;"
		cursor.execute(sql)
		data = cursor.fetchall()
		for d in data:
			info = info +"eid ="+ str(d[0]) +"\t"+"name="+ str(d[1])+"\t\t"+"sal=" +str(d[2]) + "\n"
		ve_st_data.insert(INSERT, info)
	except Exception as e :
		showerror("ISSUE", e)
	finally:
		if con is not None:
			con.close()
def f3():
	ae.withdraw()
	mw.deiconify()
def f4():
	ve.withdraw()
	mw.deiconify()
def f5():
	mw.withdraw()
	ue.deiconify()
def f6():
	ue.withdraw()
	mw.deiconify()
def f7():
	de.withdraw()
	mw.deiconify()
def f8():
	mw.withdraw()
	de.deiconify()
def f9():
	con=None
	try:	
		con=connect("emp.db")
		cursor=con.cursor()
		eid=str(ae_ent_eid.get())
		name=str(ae_ent_name.get())
		sal=str(ae_ent_sal.get())	
		if len(eid) == 0 or eid.isnumeric() == False or int(str(eid)) <= 0 :
			showerror("Issue","please enter valid employee Id")	
		elif len(name) == 0 or len(name) < 2 or name.isalpha() ==  False:
			showerror("Issue","please enter valid employee name")
		elif len(sal) == 0  or sal.isnumeric() == False or int(str(sal)) <= 7999:
			showerror("Isuue", "please enter valid employee salary")
		else:
			sql="insert into employee values('%s','%s','%s')"
			cursor.execute(sql %(eid,name,sal))
		   	#cursor.rowcount == 1
			con.commit()
			showinfo("success","record added,thank you")
	except IntegrityError:
		showerror("mistake","record already exists")		
	#except Exception as e:
		#showerror(e,"record already exists")
	except Exception as e:
		showerror("issue",e)
		 
		con.rollback()
	finally:		
		if con is not None:
			con.close()
			ae_ent_eid.delete(0,END)
			ae_ent_name.delete(0,END)
			ae_ent_sal.delete(0,END)							

def f10():
	eid=str(ue_ent_eid.get())
	name=str(ue_ent_name.get())
	sal=str(ue_ent_sal.get())
	con = None
	try:
		con=connect("emp.db")
		print("connected")
		cursor = con.cursor()
		if len(eid) == 0 or eid.isnumeric() == False or int(str(eid)) <= 0 :
			showerror("Issue","please enter valid employee Id")
		elif len(name) == 0 or len(name) < 2 or name.isalpha() ==  False:
			showerror("Issue","please enter valid employee name")
		elif len(sal) == 0  or sal.isnumeric() == False or int(str(sal)) <= 7999:
			showerror("Isuue", "please enter valid employee salary")
		else:
			sql = "update employee set name = '%s', sal = '%s' where eid= '%s' "
			cursor.execute(sql % (name,sal,eid))
		if cursor.rowcount == 1:
			con.commit()
			showinfo("success","record updated")	

		else: 
			showerror("issue","record does not exists,try again")

	except Exception as e:
		print("issue", e)
		con.rollback()
	finally:
		if con is not None:
			con.close()
			ue_ent_eid.delete(0,END)
			ue_ent_name.delete(0,END)
			ue_ent_sal.delete(0,END)
			ue_ent_eid.focus()
			print("closed")	
def f11():
	eid=str(de_ent_eid.get())	
	con = None
	try:
		con = connect("emp.db")
		print("connected")
		cursor = con.cursor()
		if len(eid) == 0 or eid.isnumeric() == False or int(str(eid)) <= 0 :
			showerror("Issue","please enter valid employee Id")
		
		else:
			sql = "delete from employee where eid = '%s' "
			cursor.execute(sql % (eid))
		if cursor.rowcount == 1:
			con.commit()
			showinfo("success","deleted")
		else:
			showerror("issue","record does not exists")
			print(eid, "does not exits")
	except Exception as e :
			print("issue ", e)
	finally:
		if con is not None:
			con.close()
			de_ent_eid.delete(0,END)	
			de_ent_eid.focus()
			print("closed")

def f12():
		global quote
		try:
			wa = "https://www.brainyquote.com/quote_of_the_day"
			res = requests.get(wa)
			data=bs4.BeautifulSoup(res.text , "html.parser")
			info = data.find("img" , ["p-qotd"])
			msg = info["alt"]
			quote = msg
		except Exception as e :
			print("issue" , e)
f12()
def f13():
		data = pd.read_csv("jeet.csv")
		emp=data.sort_values(by=["sal"], ascending = False)
		jeet=emp.head(5)
		name = jeet["name"].tolist()
		sal = jeet["sal"].tolist()
		plt.bar(name, sal)
		plt.xlabel("Employee Name")
		plt.ylabel("Employee Salary in RS")
		plt.title("TOP 5 EARNING SALARIED EMPLOYEE")
		plt.show()

#********************************* MAIN WINDOW(LABELS AND BUTTONS) ************************************************************

mw_lab_emp = Label(mw, text="EMPLOYEE MANAGEMENT SYSTEMS", font=f,fg="white", bg='black')
mw_btn_ADD = Button(mw, text="ADD", font=f,fg="white", bg='black', activebackground='#00ff00', width=10,command=f1)
mw_btn_VIEW = Button(mw, text="VIEW", font=f,fg="white", bg='black', activebackground='#00ff00', width=10, command=f2)
mw_btn_UPDATE = Button(mw, text="UPDATE", font=f,fg="white", bg='black', activebackground='#00ff00', width=10,command=f5)
mw_btn_DELETE = Button(mw, text="DELETE", font=f,fg="white", bg='black', activebackground='#00ff00', width=10,command=f8)
mw_btn_CHART = Button(mw, text="CHART", font=f,fg="white", bg='black', activebackground='#00ff00', width=10,command=f13)
mw_lab_QOTH = Label(mw, text="QOTD:", font=("Helvetica",30),fg="black", bg='white')
mw_lab_qotd = Label(mw, text= "Progress was all right.Only it went on too long. - James Thurber", font=f,fg="black", bg='white')
mw_lab_emp.pack(padx=5,pady=y)
mw_btn_ADD.pack(pady=y)
mw_btn_VIEW.pack(pady=y)
mw_btn_UPDATE.pack(pady=y)
mw_btn_DELETE.pack(pady=y)
mw_btn_CHART.pack(pady=y)
separator = ttk.Separator(mw, orient='horizontal')
separator.pack(fill='x')
mw_lab_QOTH.pack(padx=120,pady=y)
mw_lab_qotd.pack(pady=50)


#******************************** ADD EMPLOYEE WINDOW(LABELS AND BUTTONS) *****************************************************

ae =Toplevel(mw)
ae.title("ADD EMPLOYEE")
ae.geometry("950x650+250+100")
ae.configure(bg="#FFC0CB")
ae_lab_eid = Label(ae, text="Enter Employee Id: ", font= f,bg='#FFC0CB')
ae_ent_eid = Entry(ae,font= f, bd=3)
ae_lab_name = Label(ae, text="Enter Employee Name: ",font=f,bg='#FFC0CB')
ae_ent_name = Entry(ae,font= f, bd=3)
ae_lab_sal = Label(ae, text="Enter Employee Salary: ", font= f,bg='#FFC0CB')
ae_ent_sal = Entry(ae,font= f, bd=3)
ae_btn_save= Button(ae, text="Add", font=f,activebackground='#7CFC00',command=f9)
ae_btn_back= Button(ae, text="Back", font=f,activebackground='#FF00FF', command=f3)
ae_lab_eid.pack(pady=y)
ae_ent_eid.pack(pady=y)
ae_lab_name.pack(pady=y)
ae_ent_name.pack(pady=y)
ae_lab_sal.pack(pady=y)
ae_ent_sal.pack(pady=y)
ae_btn_save.pack(pady=y)
ae_btn_back.pack(pady=y)
ae.withdraw()

#******************************* VIEW EMPLOYEE WINDOW(LABELS AND BUTTONS) ***************************************************************
ve= Toplevel(mw)
ve.title("VIEW EMPLOYEE")
ve.geometry("950x650+250+100")
ve.configure(bg="#856FF8")
ve_st_data = ScrolledText(ve, width=40, height=15, font=f,fg="black",bg="#FFC0CB")
ve_st_data.pack(pady=y)
ve_btn_back= Button(ve, text = "Back",font=f,activebackground='#FF00FF', command=f4,fg="black")
ve_btn_back.pack(pady=y)
ve.withdraw()

#******************************* UPDATE EMPLOYEE WINDOW(LABELS AND BUTTONS) *************************************************************
ue =Toplevel(mw)
ue.title("UPDATE EMPLOYEE")
ue.geometry("950x650+250+100")
ue.configure(bg="#FFFFE0")
ue_lab_eid = Label(ue, text="Enter Employee Id: ", font= f,bg='#FFFFE0')
ue_ent_eid = Entry(ue,font= f, bd=3)
ue_lab_name = Label(ue, text="Enter Employee Name: ", font= f,bg='#FFFFE0')
ue_ent_name = Entry(ue,font= f, bd=3)
ue_lab_sal = Label(ue, text="Enter Employee Salary: ", font= f,bg='#FFFFE0')
ue_ent_sal = Entry(ue,font= f, bd=3)
ue_btn_save= Button(ue, text="Update", font=f,activebackground='#7CFC00',command=f10)
ue_btn_back= Button(ue, text="back", font=f,activebackground='#FF00FF',command=f6)
ue_lab_eid.pack(pady=y)
ue_ent_eid.pack(pady=y)
ue_lab_name.pack(pady=y)
ue_ent_name.pack(pady=y)
ue_lab_sal.pack(pady=y)
ue_ent_sal.pack(pady=y)
ue_btn_save.pack(pady=y)
ue_btn_back.pack(pady=y)
ue.withdraw()

#****************************** DELETE EMPLOYEE WINDOW(LABELS AND BUTTONS) ****************************************************************
de=Toplevel(mw)
de.title("DELETE EMPLOYEE DETAILS")
de.geometry("950x650+250+100")
de.configure(bg="#7FFFD4")
de_lab_eid = Label(de, text="enter employee id:", font=f,bg='#7FFFD4')
de_ent_eid = Entry(de, font=f, bd=3)
de_btn_save = Button(de, text="Delete", font=f,activebackground='#7CFC00',command=f11)
de_btn_back = Button(de, text="Back",font=f,activebackground='#FF00FF', command=f7)
de_lab_eid.pack(pady=y)
de_ent_eid.pack(pady=y)
de_btn_save.pack(pady=y)
de_btn_back.pack(pady=y)
de.withdraw()

mw.mainloop()
