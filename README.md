# Project1
#Author:                    Adriana Lizmory Cuellar (Liz Cuellar)
#Student Number:            8672918
#Date:                      April 14 2022
#Final Proyect:             AUTOMOTIVE SCHOOL SOFTWARE

class Student: # (id, name, age, address, telephoneNumber)

    #statics methods ensure that the information passed by parameter to the initializer is correct
    
    @staticmethod #reserved keyword to create a static method in python
    def intTry(input): # this class function validates the entered integer (input parameter). Returns True if it is a non-negative integer.
         try:
             id = int(input) #an attempt is made to transform the variable "input" to an integer
             if id > 0:      #if it could, then it evaluates if it is a positive integer
                 return True # if it is fulfilled it returns True
             else:           # If it is positive but negative, False is returned.
                 return False
         except:             # If an exception occurs when trying to pass the "input" variable to an integer, False is returned.
             return False
         
            
    @staticmethod  #this class function makes sure that the user enters a valid id. Returns a valid positive integer for id
    def enteringId():
        while(True):    # the while loop allows repeating ...
            entry = input("id: ")  #...reading the id until ...
            if Student.intTry(entry): # the input is validated by the class method intTry()
                return entry# if it is valid, the entry variable is returned ending the cycle
            else:
                print("you have not entered a valid id") # if the intTry() method returns False, a warning message is printed and iterates again
                
      
    @staticmethod
    def enteringAge(): # this class method returns a validated age  
        while(True):    #the while loop allows repeating ...
            entry = input("age: ")  #...reading the id until ...
            if Student.intTry(entry) and int(entry) < 200: # the input is validated by the class method intTry()
                return entry# if it is valid, the entry variable is returned ending the cycle
            else:
                print("you have not entered a valid age") # if the intTry() method returns False, a warning message is printed and iterates again

    @staticmethod
    def enteringStatus(): #this class method returns a validated status
           while(True):                #se despliega un menu de opciones válidas
               print("Possible status")
               print("1. approved")
               print("2. disapproved")
               print("3. in progress")
               entry = input("choose a number: ")  # the input from the console is saved in the entry variable
               
               if entry != "1" and entry != "2" and entry != "3":  #if entry is not "1" and entry is not "2" and entry is not "3"...
                   print("you have to choose a number of the menu. Try again") #...an error message is printed
            
               else:
                   if entry == "1":
                       return "approved"
                   elif entry == "2":
                       return "disapproved"
                   else:
                       return "in progress"
                       

##### END Statics methods ######    
            
    
    def __init__(self, id, name, age, address, telephoneNumber):
        self.id = id
        self.name = name
        self.age = age
        self.address = address
        self.telephoneNumber = telephoneNumber
        self.status = "in progress"
    
    def printStudent(self):
        print("id: {}".format(self.id))
        print("name: {}".format(self.name))
        print("address: {}".format(self.address))
        print("telephone number: {}".format(self.telephoneNumber))
        print("status: {}".format(self.status))     
        
    def printToaFile(self, path): #this instance method prints the student's data in the file corresponding to the path passed by parameter. The path needs to be in windows-python format with 'r' in front of the file path
        try:    
            myFile = open(path, 'a')
            myFile.write("id: {}\n".format(self.id))
            myFile.write("name: {}\n".format(self.name))
            myFile.write("address: {}\n".format(self.address))
            myFile.write("telephone number: {}\n".format(self.telephoneNumber))
            myFile.write("status: {}\n".format(self.status))    
            print("student {} {} is stored in the file {}".format(self.id, self.name, path))
            
            
        except:
            print("Exception: the information about {} student was not written to the file".format(self.id))   
            
        finally:               
            myFile.close()   
        
        
    def setStatus(self, status): # "setStatus" sets the instance attribute value "status". The status parameter is of type string and represents the status of the student
        self.status = status
        
    def getStatus(self):
        return self.status

################### end of Student class #########################################


class School:
    
    def __init__(self):
        self.students = {} # a dictionary as an instance attributte is declared
    
    
    def addStudent(self, Student):
         
        id = Student.id
        self.students[id] = Student #The id of the student passed by parameter is taken by key and the student itself by value
    
    def getStudent(self, id): # this method returns an object of type 'student' that corresponds to the key 'id' 
      return self.students.get(id)
              
    def checkStudentStatus(self, id): # "checkStudentStatus" returns the status of the student with the id passed by parameter
        
        student = self.students.get(id)
        status = student.getStatus()
        return status
    
    def studentExists(self, id): # this method returns True if the id passed by parameter exists as a key in the "students" dictionary
        return id in self.students
    
    def getTotalStudents(self):
        return len(self.students.keys())
    
    def approvedStudents(self): # "approvedStudents" returns a list with all approved students
        listApproved = []       # an empty list is created to store all the passed students
        for id in self.students: # iterate with a loop for the dictionary "students"
            if self.students[id].getStatus() == "approved": #if the student has status equal to "approved" ...
                listApproved.append(self.students[id]) #... is added to the list
                    
        return listApproved                  #lthe approved list "listApproved" is returned
     
    def printApproveds(self): # this method prints all approved students
        listApproved = self.approvedStudents() #the list of approved students is loaded through the approvedStudents() method
        if len(listApproved) == 0:             #if the list is empty...
            print("there are no approved students in the system")
        else:                                  #if the list contains at least one student len(listDisapproved) > 0
            for student in listApproved:       #is iterated and each object of type student in the list, calls the method printStudent()
                student.printStudent()
            
        
    def printDisapproveds(self): # this method prints all failed students
        listDisapproved = self.disapprovedStudents() #the list of disapproved students is loaded through the disapprovedStudents() method
        if len(listDisapproved) == 0:                #if the list is empty...
            print("there are no disapproved students in the system") 
        else:                                        #if the list contains at least one student len(listDisapproved) > 0
            for student in listDisapproved:          #iterates and each object of type student in the list, calls the printStudent() method
                student.printStudent()
        
    
    def printInProgress(self): # this method prints all students in progress
        listInProgress = self.inProgressStudents() #the list of students in progress is loaded through the inProgressStudents() method
        if len(listInProgress) == 0:               #if the list is empty...
            print("there are no in progress students in the system")
        else:                                      #if the list contains at least one student len(listInProgress) > 0
            for student in listInProgress:         #iterates and each object of type student in the list, calls the printStudent() method
                student.printStudent()
    
    
    def getNumberApprovedStudents(self): # "getNumberApprovedStudents" returns the number of students with "approved" status
        number = len(self.approvedStudents()) # the method "approvedStudents" returns the list of approved students and its length is stored in the variable "number"
        return number # "number" variable is returned
    
    def disapprovedStudents(self): #this method returns a list of students of the failed students
        listDisapproved = []       # an empty list is created to store all the passed students
        for id in self.students: # we iterate with a loop for the dictionary "students"
            if self.students[id].getStatus() == "disapproved": #if the student has status equal to "approved" ...
                listDisapproved.append(self.students[id]) #... is added to the list
        return listDisapproved
    
    def inProgressStudents(self): #this method returns a list of students of the students in progress
        listInProgress = []       # an empty list is created to store all students in progress
        for id in self.students: # we iterate with a loop for the dictionary "students"
            if self.students[id].getStatus() == "in progress": #if the student has status equal to "in progress" ...
                listInProgress.append(self.students[id]) #... is added to the list
        return listInProgress
        
    def getNumberDisapprovedStudents(self): # "getNumberDisapprovedStudents"    returns the number of students with status "disapproved"
        number = len(self.disapprovedStudents())  # the method "disapprovedStudents" returns the list of disapproved and its length is stored in the variable "number"
        return number  # "number" variable is returned
    
    def getNumberInProgressStudents(self):
        number = len(self.inProgressStudents())  # the "inProgressStudents" method returns the list of students enrolled with status "in progress" and its length is stored in the variable "number"
        return number  # "number" variable is returned
              
    def percentageApprovedStudents(self): # this method allows to obtain the percentage of approved students
        total = self.getTotalStudents()   # we get the total number of students with the getTotalStudents method
        cant = self.getNumberApprovedStudents() # we get the number of approved students with the method get Number Approved Students
        percentage = (cant*100)/total# calculate the percentage with the formula quantity per hundred divided by the total
        return int(percentage)              
        
    def percentageDisapprovedStudents(self): # this method allows to obtain the percentage of disapproved students
        total = self.getTotalStudents()     # we get the total number of students with the getTotalStudents method
        cant = self.getNumberDisapprovedStudents() # we get the number of disapproved students with the getNumberDisapprovedStudents method
        percentage = (cant*100)/total # calculate the percentage with the formula quantity per hundred divided by the total
        return int(percentage)    
    
    def percentageInProgressStudents(self):
        total = self.getTotalStudents()     # we get the total number of students with the getTotalStudents method
        cant = self.getNumberInProgressStudents() # we get the number of students in progress with the method getNumberInProgressStudents
        percentage = (cant*100)/total # We calculate the percentage with the formula quantity per hundred divided by the total
        return int(percentage)   
    
    def removeStudent(self, id): # this method removes from the student dictionary according to the id key passed by parameter
        if self.existStudent(id): #it is verified that the id key passed by parameter is in the dictionary
            self.students.pop(id) #yes, it is then the pop() method is called to remove an element from the dictionary, it receives the id parameter as a parameter
            print("student removed succesfully")
        else:
            print("the student with id {} was not found".format(id))
         
    
    def existStudent(self, id): # this method returns True if the student exists in the students dictionary, it receives the student id as a parameter
        return id in self.students # returns True if id is in self.students
    
    
    def printAllStudents(self):  #this method prints the list of all the students of the school
        
        for id in self.students.keys():   #iterates over the keys of the students instance attribute ...
            print(self.students[id].printStudent()) #... and print each value (instance of type "Student" and its method "printStudent") by iterated key
    
    def allStudentsToFile(self, path):     # this method prints all the students in the school at the address of the file passed by the path parameter
        
        for id in self.students.keys():     #iterates over the keys of the students instance attribute ...
            self.students[id].printToaFile(path)  #... #... and each value (instance of type "Student" and its method "printToaFile") is printed to the indicated file by iterated key
            

    def getGeneralInform(self):
        print("Total students in School: {}".format(self.getTotalStudents()))
        print("Apporoved students:       {}  {}%".format(self.getNumberApprovedStudents(), self.percentageApprovedStudents()))
        print("Disapproved students:     {}  {}%".format(self.getNumberDisapprovedStudents(), self.percentageDisapprovedStudents()))
        print("In progress students:     {}  {}%".format(self.getNumberInProgressStudents(), self.percentageInProgressStudents()))
        
    
    def generalInformToFile(self, path):  #this instance method prints the school general information data in the file corresponding to the path passed by parameter. The path needs to be in windows-python format with 'r' in front of the file path
        try:     
            
            myFile = open(path, 'a')
            myFile.write("General information\n")
            myFile.write("\nTotal students in School: {}".format(self.getTotalStudents()))
            myFile.write("\nApporoved students:       {}  {}%".format(self.getNumberApprovedStudents(), self.percentageApprovedStudents()))
            myFile.write("\nDisapproved students:     {}  {}%".format(self.getNumberDisapprovedStudents(), self.percentageDisapprovedStudents()))
            myFile.write("\nIn progress students:     {}  {}%".format(self.getNumberInProgressStudents(), self.percentageInProgressStudents()))              
            print("The general information was written in the file {}".format(path))
            
        except:
            
            print("Exception: the general information was not written to the file")
            
        finally:
            myFile.close()
            
            
################### end of School class #########################################

def enrollingNewStudent(): #this method returns an object of type "student"
   
    success = False
    while(success == False):
        
        print("Please, enter the data of the new student")
        #Student(id, name, age, address, telephoneNumber)
        id = Student.enteringId()
        name = input("name: ")
        age = Student.enteringAge()
        address = input("adress: ")
        telephoneNumber = input("telephone number: ")
        print("\nthe information entered was:")
        student = Student(id, name, age, address, telephoneNumber)
        student.printStudent()
        print("confirm the registration")
        print("1. yes")
        print("2. no")
        option = input("option: ")
        if option =="1":
            print("you have entered a new student to the system succesfully")
            print("by default the new student status is 'in progress'")
            success = True
        elif option =="2":
            print("the student was not registered. Try again\n")
            
        else:
            print("Please, select a number of menu")
    
    return student
    
path = r"C:\Applications\studentR.txt"  # this is the path of the file where the information will be written.
school = School()

print("Welcome to our driving school")

while(True):
       
    print("\nPlease select one number of the following options\n ")
    print("1. Enroll new student")
    print("2. Update student status")
    print("3. Check student status")
    print("4. Remove student from record")
    print("5. Print all students")
    print("6. Print general school report")
    print("7. Print only approved students")
    print("8. Print only disapproved students")
    print("9. Print only in progress students")
    print("10. Exit")
    option = input("number option: ")
    
    if option =="1":
        student = enrollingNewStudent()
        school.addStudent(student)
        
    
    elif option == "2":
        id = input("please enter student id: ")
        if school.existStudent(id):
            status = Student.enteringStatus()
            student = school.getStudent(id)
            student.setStatus(status)
            print("the status of {} {} was updated succesfully".format(student.id, student.name))
        else:
            print("the id does not match with any student")
        
    elif option == "3":
        id = input("please enter student id: ")
        if school.existStudent(id):
            student = school.getStudent(id)
            print("The actual status of {} is {}".format(student.name, student.getStatus()))
        else:
            print("the id does not match with any student")
    
    elif option == "4":
        id = input("please enter student id: ")
        if school.existStudent(id):
            school.removeStudent(id)
        else:
            print("the id does not match with any student")
    
    elif option == "5":
        school.printAllStudents()
        school.allStudentsToFile(path)
        
    elif option == "6":
        school.getGeneralInform()
        school.generalInformToFile(path)
        
    elif option == "7":
        school.printApproveds()
    
    elif option == "8":
        school. printDisapproveds()
        
    elif option == "9":
        school.printInProgress()
    
    elif option == "10":
        print("GoodBye!")
        break
    
    else:
        print("you have not choosen a correct option")
