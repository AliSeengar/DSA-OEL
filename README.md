# DSA-OEL
CODE OEL LAB:


import java.util.*;
 
 class Student {
     String rollNo;
     String department;
 
     public Student(String rollNo, String department) {
         this.rollNo = rollNo;
         this.department = department;
     }
 
     @Override
     public String toString() {
         return rollNo + " (" + department + ")";
     }
 }
 
 class ClassroomOrganizer {
     int rows, cols;
     Student[][] seats;
     HashMap<String, Queue<Student>> deptMap = new HashMap<>();
 
     public ClassroomOrganizer(int rows, int cols) {
         this.rows = rows;
         this.cols = cols;
         this.seats = new Student[rows][cols];
     }
 
     public void addStudent(Student student) {
         deptMap.putIfAbsent(student.department, new LinkedList<>());
         deptMap.get(student.department).add(student);
     }
 
     public void assignSeats() {
         List<Queue<Student>> deptQueues = new ArrayList<>(deptMap.values());
         int deptCount = deptQueues.size();
         int index = 0;
 
         // Loop through all seats
         for (int i = 0; i < rows; i++) {
             int start = (i % 2 == 0) ? 0 : cols - 1;
             int end = (i % 2 == 0) ? cols : -1;
             int step = (i % 2 == 0) ? 1 : -1;
 
             // Assign students to the seats
             for (int j = start; j != end; j += step) {
                 int qIndex = index % deptCount;
                 while (deptQueues.get(qIndex).isEmpty()) {
                     qIndex = (qIndex + 1) % deptCount;
                 }
                 seats[i][j] = deptQueues.get(qIndex).poll();
                 index++;
                 if (index >= rows * cols) { // Stop if we run out of seats
                     return;
                 }
             }
         }
     }
 
     public void printSeats() {
         System.out.println("\n--- Seating Arrangement ---");
         for (Student[] row : seats) {
             for (Student s : row) {
                 System.out.print((s != null ? s : "Empty") + " | ");
             }
             System.out.println();
         }
     }
 }
 
 public class Main {
     public static void main(String[] args) {
         ClassroomOrganizer organizer = new ClassroomOrganizer(3, 4);
 
         organizer.addStudent(new Student("BSCS01", "CS"));
         organizer.addStudent(new Student("BSCS02", "CS"));
         organizer.addStudent(new Student("BSAI01", "AI"));
         organizer.addStudent(new Student("BSAI02", "AI"));
         organizer.addStudent(new Student("BSSE01", "SE"));
         organizer.addStudent(new Student("BSSE02", "SE"));
         organizer.addStudent(new Student("BSCS03", "CS"));
         organizer.addStudent(new Student("BSAI03", "AI"));
         organizer.addStudent(new Student("BSSE03", "SE"));
 
         organizer.assignSeats();
         organizer.printSeats();
     }
 }
