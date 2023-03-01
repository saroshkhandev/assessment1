## Employee Management Project

This project uses concepts of OOPs. It is an Employee Management system with a proper admin panel.

---

## Project Structure

*   Employee
    *   Admin(Superuser)
    *   Manager
    *   Accountant
    *   Engineer
*   Employee Composite Classes:
    *   Salary
    *   Attendance

### Parent Class:

**Employee:** 

```java
package com.fc.company;

import com.fc.company.helperclasses.Attendance;
import com.fc.company.helperclasses.Salary;

public class Employee {

//    instance vars
    private int emp_id;
    private String emp_name;
    private String emp_role;

    private Salary emp_sal;
    private float bonus;
    private Attendance emp_attd;

//    constructor
    Employee(int emp_id, String emp_name, String emp_role, Salary emp_sal, Attendance emp_attd) {
        this.emp_id = emp_id;
        this.emp_name = emp_name;
        this.emp_role = emp_role;
        this.emp_sal = emp_sal;
        this.bonus = 0;
        this.emp_attd = emp_attd;
    }


//    getter and setter
    public String getEmp_name() {
        return emp_name;
    }

    public int getEmp_id() {
        return emp_id;
    }

    public Salary getEmp_sal() {
        return emp_sal;
    }

    public String getEmp_role() {
        return emp_role;
    }

    public void setBonus(float bonus) {
        this.bonus = bonus;
    }

    public float getBonus() {
        return bonus;
    }

    public Attendance getEmp_attd() {
        return emp_attd;
    }

    public String toString() {
        return "Employee Name: " + getEmp_name() + "\n" + "Employee Role: " + getEmp_role() + "\n" + "Employee Bonus: " + getBonus() + "\n" + "Employee Attendance: " + getEmp_attd();
    }
}


// setattendance
```

## Subclasses

**Admin:**

```java
package com.fc.company;

import com.fc.company.helperclasses.Attendance;
import com.fc.company.helperclasses.Salary;

import java.io.*;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.Scanner;

public class Admin extends Employee {
    //    init Scanner
    Scanner sc = new Scanner(System.in);
    private String project;
    private ArrayList<Employee> employeeList = new ArrayList<>();

    Admin(int emp_id, String emp_name, String emp_role, Salary emp_sal, String project, Attendance emp_attendance) {
        super(emp_id, emp_name, emp_role, emp_sal, emp_attendance);
        this.project = project;
    }

    public String getProject() {
        return project;
    }


    //    Functions responsible for adding employee
    public void createEmployee() {
        System.out.println("Select Type: ");
        System.out.println("1: Accountant");
        System.out.println("2: Manager");
        System.out.println("3: Engineer");
        int choice = sc.nextInt();
        String role = "";
        switch (choice) {
            case 1 -> role = "accountant";
            case 2 -> role = "manager";
            case 3 -> role = "engineer";
        }
        addEmployee(role);
    }

    public void addEmployee(String role) {
        switch (role) {
            case "engineer" -> {
                Salary emp_sal = initSalary(role);
                Attendance emp_attd = initAttendance();

                System.out.println("Enter Details: ");
                System.out.println("Id: ");
                int id = sc.nextInt();
                sc.nextLine();

                System.out.println("Name: ");
                String emp_name = sc.nextLine();

                System.out.println("Current Project: ");
                String emp_cr_project = sc.nextLine();
                System.out.println("Team: ");
                String emp_team = sc.nextLine();

                Employee Engineer = new Engineer(id, emp_name, role, emp_sal, emp_attd, emp_cr_project, emp_team);
                employeeList.add(Engineer);
            }
            case "manager" -> {
                Salary emp_sal = initSalary(role);
                Attendance emp_attd = initAttendance();

                System.out.println("Enter Details: ");
                System.out.println("Id: ");
                int id = sc.nextInt();
                sc.nextLine();

                System.out.println("Name: ");
                String emp_name = sc.nextLine();

                System.out.println("Branch Name: ");
                String emp_branch = sc.nextLine();

                Employee Manager = new Manager(id, emp_name, role, emp_sal, emp_attd, emp_branch);
                employeeList.add(Manager);
            }

            case "accountant" -> {
                Salary emp_sal = initSalary(role);
                Attendance emp_attd = initAttendance();

                System.out.println("Enter Details: ");
                System.out.println("Id: ");
                int id = sc.nextInt();
                sc.nextLine();

                System.out.println("Name: ");
                String emp_name = sc.nextLine();

                System.out.println("Certified Type: ");
                System.out.println("1: Chartered");
                System.out.println("2: Finance");
                int choice = sc.nextInt();
                String type = "";
                switch (choice) {
                    case 1 -> type = "Chartered";
                    case 2 -> type = "Finance";
                }

                Employee Accountant = new Accountant(id, emp_name, role, emp_sal, emp_attd, type);
                employeeList.add(Accountant);
                System.out.println("Success!");
            }
        }
    }


    public Salary initSalary(String role) {
        int basic = 0, hra = 0, ta = 0;
        switch (role) {
            case "manager" -> {
                basic = 1800000;
                hra = 200000;
                ta = 45000;
            }
            case "accountant" -> {
                basic = 1100000;
                hra = 150000;
                ta = 15000;
            }
            case "engineer" -> {
                basic = 1500000;
                hra = 300000;
                ta = 45000;
            }
        }

        return new Salary(basic, ta, hra);
    }

    public Attendance initAttendance() {
        Date date = new Date();
        SimpleDateFormat sdf1 = new SimpleDateFormat("dd/MM/yy");
        SimpleDateFormat sdf2 = new SimpleDateFormat("hh:mm");
        String str = sdf1.format(date) + " " + sdf2.format(date) + " " + sdf2.format(date);

        ArrayList<String> arr = new ArrayList<>(30);
        arr.add(str);

        return new Attendance(arr);
    }

    public ArrayList<Employee> getEmployeeList() {
        return employeeList;
    }

    //    function to add attendance
    public void makeAttendance(int id) {
        for (Employee emp : employeeList) {
            if (emp.getEmp_id() == id) {
                emp.getEmp_attd().punchIn();
                System.out.println("Success!");
                break;
            }
        }
    }

    public void punchOut(int id) {
        for (Employee emp : employeeList) {
            if (emp.getEmp_id() == id) {
                emp.getEmp_attd().punchOut();
                System.out.println("Success!");
                break;
            }
        }
    }

    public boolean isAccountantPresent() {
        boolean flag = false;
        for (Employee emp : employeeList) {
            if (emp.getEmp_role().equals("accountant")) {
                flag = true;
                break;
            }
        }

        return flag;
    }

    public Employee callAccountant() {
        Employee acc = null;
        for (Employee emp : employeeList) {
            if (emp.getEmp_role().equals("accountant")) {
                acc = emp;
                break;
            }
        }
        return acc;
    }

    public void calcBonus(int id) {
        for (Employee emp : employeeList) {
            if (emp.getEmp_id() == id) {
                Employee accountant = callAccountant();
                float bonus = ((Accountant) accountant).calcBonus(emp);
                emp.setBonus(bonus);
                System.out.println("Success!");
            }
        }
    }

    public void printEmployeeDetails(int id) {
        for (Employee emp : employeeList) {
            if (emp.getEmp_id() == id) {
                System.out.println("__________________________________");
                System.out.println(emp);
                System.out.println("__________________________________");
            }
        }
    }

    public void exportUserData(int id) {
        Path path = Paths.get("C:\\Users\\sarosh.abdullah\\Desktop\\employeedetail.txt");
        String details = "";
        for (Employee emp : employeeList) {
            if (emp.getEmp_id() == id) {
                details = emp.toString();
            }
        }
        BufferedWriter bw = null;
        try {
            File file = new File("C:\\Users\\sarosh.abdullah\\Desktop\\details.txt");
            if (!file.exists()) file.createNewFile();

            FileWriter fw = new FileWriter(file);
            bw = new BufferedWriter(fw);
            bw.write(details);
            System.out.println("Success!");
            System.out.println("File exported to Desktop");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }


    }


    public String toString() {
        return super.toString() + "\n" + "Project Name: " + getProject();
    }
}
```

**Manager:**

```java
package com.fc.company;

import com.fc.company.helperclasses.Attendance;
import com.fc.company.helperclasses.Salary;

public class Manager extends Employee{

    private String mr_branch;

//    init super class and current class
    Manager(int emp_id, String emp_name, String emp_role, Salary emp_sal, Attendance emp_attendance, String mr_branch) {
        super(emp_id, emp_name, emp_role, emp_sal, emp_attendance);
        this.mr_branch = mr_branch;
    }

    public String getMr_branch() {
        return mr_branch;
    }

    public String toString() {
        return super.toString() + "\n" + "Branch Name: " + getMr_branch();
    }
}
```

**Accountant:** 

```java
package com.fc.company;

import com.fc.company.helperclasses.Attendance;
import com.fc.company.helperclasses.Salary;

public class Accountant extends Employee {

    private String certified_type;

    Accountant(int emp_id, String emp_name, String emp_role, Salary emp_sal, Attendance emp_attendance, String certified_type) {
        super(emp_id, emp_name, emp_role, emp_sal, emp_attendance);
        this.certified_type = certified_type;
    }

    public String getCertified_type() {
        return certified_type;
    }

    public float calcBonus(Employee emp) {
        String role = emp.getEmp_role().toLowerCase();
        int baseSalary = emp.getEmp_sal().getBasic() + emp.getEmp_sal().getHra();
        float bonus = 0;
        switch (role) {
            case "manager":
                bonus = (float) (baseSalary * 0.12);
                break;
            case "admin":
                bonus = (float) (baseSalary * 0.1);
                break;
            case "engineer":
                bonus = (float) (baseSalary * 0.12);
                break;
            case "accountant":
                bonus = (float) (baseSalary * 0.8);
                break;
            default:
                System.out.println("Invalid role");
        }

        return bonus;
    }

    public String toString() {
        return super.toString() + "\n" + "Certified Type: " + certified_type;
    }
}
```

**Engineer:**

```java
package com.fc.company;

import com.fc.company.helperclasses.Attendance;
import com.fc.company.helperclasses.Salary;

public class Engineer extends Employee {

    private String currentProject;
    private String team;

    Engineer(int emp_id, String emp_name, String emp_role, Salary emp_sal, Attendance emp_attendance, String currentProject, String team) {
        super(emp_id, emp_name, emp_role, emp_sal, emp_attendance);
        this.currentProject = currentProject;
        this.team = team;
    }

    public String getCurrentProject() {
        return currentProject;
    }

    public String getTeam() {
        return team;
    }

    public String toString() {
        return super.toString() + "\n" + "Current Project: " + getCurrentProject() + "\n" + "Team: " + getTeam();
    }
}
```

### Composite Classes:

**Salary:**

```java
package com.fc.company.helperclasses;

public class Salary {
    private int basic;
    private int ta;
    private int hra;

    public Salary(int basic, int ta, int hra) {
        this.basic = basic;
        this.ta = ta;
        this.hra = hra;
//        System.out.println(basic + " " + ta + " " + hra);
    }

    public int getBasic() {
        return basic;
    }

    public int getHra() {
        return hra;
    }

    public int getTa() {
        return ta;
    }
}
```

**Attendance:**

```java
package com.fc.company.helperclasses;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;

public class Attendance {
    private ArrayList<String> calender;
    private ArrayList<String> presentDaysList;

    private int presentDays;
    private int absentDays;
    private int totalDays;

    public Attendance(ArrayList<String> calender) {
        this.calender = calender;
//        System.out.println(this.calender);
//        System.out.println(this.calender.size());
        calcPresentDays(this.calender);
//        setPresentDaysList(this.calender);
        this.totalDays = this.calender.size();
//        System.out.println(totalDays);
    }


    //    Productivity Functions
    public void calcPresentDays(ArrayList<String> calender) {
        int pdays = 0;
        for (String str : calender) {
            String[] data = str.split(" ");
            if (!data[1].equals("null")) {
                pdays++;
            }
        }

        setPresentDays(pdays);
        setAbsentDays(pdays);
    }

    public void punchIn() {
        Date date = new Date();
        SimpleDateFormat sdf1 = new SimpleDateFormat("dd/MM/yy");
        SimpleDateFormat sdf2 = new SimpleDateFormat("hh:mm");
        String str = sdf1.format(date) + " " + sdf2.format(date) + " ";
        this.calender.add(str);
        calcPresentDays(calender);
    }

    public void punchOut() {
        Date date = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("hh:mm");
        String str = sdf.format(date);
        int index = calender.size() - 1;
        String data = calender.get(index);
        data = data + str;
        calender.set(index, data);
    }

    //    Getter Setters
    public ArrayList<String> getPresentDaysList() {
        return presentDaysList;
    }

//    public void setPresentDaysList(ArrayList<String> calender) {
//        for (String str : calender) {
//            String[] data = str.split(" ");
//            if (!data[1].equals("null")) {
////                System.out.println(data[0]);
////                presentDaysList.add(data[0]);
//            }
////            System.out.println();
//        }
//    }

    public int getPresentDays() {
        return presentDays;
    }

    public void setPresentDays(int presentDays) {
        this.presentDays = presentDays;
    }

    public int getAbsentDays() {
        return absentDays;
    }

    public void setAbsentDays(int presentDays) {
        this.absentDays = calender.size() - presentDays;
    }

    public int getTotalDays() {
        return totalDays;
    }

    public String toString() {
        return "\nTotal Days: " + getTotalDays() + "\n" + "Present Days: " + getPresentDays() + "\n" + "Absent Days: " + getAbsentDays();
    }
}
```

## Main Class:

```java
package com.fc.company;

import com.fc.company.helperclasses.Attendance;
import com.fc.company.helperclasses.Salary;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.Scanner;

public class Main {
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        // Main Method Init

        ArrayList<String> admDates = new ArrayList<>();
        admDates.add("1/2/23 null null");
        admDates.add("2/2/23 null null");
        admDates.add("3/2/23 11.10am 6:15pm");
        admDates.add("4/2/23 10.05am 6:15pm");
        admDates.add("5/2/23 9.05am 6.15pm");
        admDates.add("6/2/23 11.50am 6.15pm");
        admDates.add("7/2/23 11.00am 6.15pm");

        //        Admin
        Salary adm_sal = new Salary(250000, 100000, 150000);
        Attendance adm_attd = new Attendance(admDates);
        Employee admin = new Admin(1001, "Micheal", "Admin", adm_sal, "CloudApp", adm_attd);

        boolean status = false;
        System.out.println("Dummy account details-> id: 1001 - password: pass1");
        System.out.println("Enter Admin id: ");
        int adminId = sc.nextInt();
        sc.nextLine();
        System.out.println("Enter Password: ");
        String pass = sc.nextLine();

        if(adminId == admin.getEmp_id() && pass.equals("pass1")) status = true;


        while (status) {
            System.out.println("Welcome " + admin.getEmp_name());
            System.out.println("ADMIN PANEL");
            System.out.println();
            System.out.println("Choose any option: ");
            System.out.println("1: Add an Employee");
            System.out.println("2: Mark Attendance");
            System.out.println("3: Punch Out");
            System.out.println("4: Calculate Bonus");
            System.out.println("5: Print Employee Details");
            System.out.println("6: Export Employee Details");
            System.out.println("9: To Exit");
            System.out.println("____________________________");
            int choice = sc.nextInt();

            switch (choice) {
                case 1 -> {
                    ((Admin) admin).createEmployee();
                }
                case 2 -> {
                    System.out.println("Enter Employee id: ");
                    int id = sc.nextInt();
                    ((Admin) admin).makeAttendance(id);
                }
                case 3 -> {
                    System.out.println("Enter Employee id: ");
                    int id = sc.nextInt();
                    ((Admin) admin).punchOut(id);
                }

                case 4 -> {
                    System.out.println("Checking Accountant: ");
                    if(((Admin) admin).isAccountantPresent()) {
                        System.out.println("Enter Employee id: ");
                        int id = sc.nextInt();
                        ((Admin) admin).calcBonus(id);
                    } else {
                        System.out.println("No Accountant Found: Add accountant first.");
                    }
                }

                case 5 -> {
                    System.out.println("Enter Employee id: ");
                    int id = sc.nextInt();
                    ((Admin) admin).printEmployeeDetails(id);
                }

                case 6 -> {
                    System.out.println("Enter Employee id: ");
                    int id = sc.nextInt();
                    ((Admin) admin).exportUserData(id);
                }

                case 9 -> System.exit(1);
            }

            System.out.println("Do you want to continue (Y/N)");
            char ch = sc.next().charAt(0);
            ch = Character.toLowerCase(ch);
            status = ch == 'y';
        }


    }


    public static void printBonus(Employee emp) {
        System.out.println("------------------------------");
        System.out.println("Emp id: " + emp.getEmp_id());
        System.out.println("Name: " + emp.getEmp_name());
        System.out.println("Bonus: " + emp.getBonus());
        System.out.println("------------------------------");
    }
}
```

## Project Snapshots:

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/4f902361d520a7f7f6f6a9ee1b868b0bf65335990fc9022e.png)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/bf3dcdd2257613e705a591c77d47bf3a5d509d5aa7e14fcd.png)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/94f107f2da5089c7537966bcfa43d3dc4a9124aada87aa42.png)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/51cb6e897b8e2a67b5dc6f76fef57809c0531c340f714b4b.png)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/48e0577b91409b77fb0693bef5fa164c48123bbaecc4cbe5.png)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/3df34cd6cbc08d20b00fa5654ab6a2c6d4bf9571285adb97.png)