COLLEGE MANAGEMENT SYSTEM (PROJECT - 3)



#include<iostream>
#include<vector>
using namespace std;

class student_data { //child class
    public: 
    
    string name;
    int id;

    // Parametrized constructor:- 

    student_data(string name, int id){
        this->name=name;
        this->id=id;
    }   
    void display(){
        cout<<"NAME: "<<name<<endl;
        cout<<"ID: "<<id<<endl;
        cout<<"--------------"<<endl;
    }

};

class college { //parent class 
    public:

    // creating vector to store student data

    vector<student_data> login;

    void default_data(string name, int id){
        login.push_back(student_data(name, id));
    }
    void add_data(string name, int id){
        login.push_back(student_data(name, id));
        cout<<"THE FOLLOWING STUDENT HAS BEEN ADDED TO THE DATABASE.";
    }
    void remove_data(int id){
        for(size_t i=0; i<login.size();i++){ // size_t is an unsigned integer type designed to represent sizes and counts,
                if(login[i].id==id){
                    cout<<"THE ID EXISTS WITH THE STUDENT AS FOLLOWS: "<<endl;
                    login[i].display();
                    cout<<"ARE U SURE U WANT TO REMOVE THE STUDENT: \n1. YES\n2. No"<<endl;
                    int choice;
                    cin>>choice;
                    if(choice==1){
                        cout<<"THE FOLLOWING STUDENT HAS BEEN REMOVED SUCCESSFULLY"<<endl;
                        login.erase(login.begin() + i);
                        break;
                    } else if(choice==2){
                        cout<<"THE FOLLOWING STUDENT WAS NOT REMOVED!!"<<endl;
                    } else {
                        cout<<"ENETERED CHOICE IS NOT CORRECT, REMOVAL FUNCTION REQUEST DECLINED!!"<<endl;
                        exit(0);
                    }
                }
            
        }
    }
    void display_all_data(){
        for(student_data &s : login){
            s.display();
        }
    }
    void calculate_attendance(float class_conducted, float class_attended){
        float class_percentage = (class_attended / class_conducted) * 100;  // Formula derived from: (attended + x) / (conducted + x) >= 0.75
        int needed_classes = (3 * class_conducted) - (4 * class_attended);
        if (needed_classes < 0) {
        needed_classes = 0; // already above 75%
        }
        cout<<"THE ATTENDANCE IS: "<<class_percentage<<"%"<<endl;
        if(class_percentage>=75){
            cout<<"NOTE: YOU SATISFY THE CRITERIA OF 75%, KEEP GOING!!";
        } else {
            cout<<"\033[1mNOTE: YOU DO NOT SATISFY THE CRITERIA OF 75%, ATTEND \033[0m"<<needed_classes<<"\033[1m CLASSES TO REACH 75%\033[0m\n"<<endl;
        }
    }
    bool function_replay(){
        cout<<"DO U WANT TO PLACE ANOTHER FUNCTION REQUEST: "<<endl;
        cout<<"1. YES"<<endl;
        cout<<"2. NO"<<endl;
        int choice;
        cin>>choice;
        if(choice==1){
            cout<<"THE FOLLOWING REQUEST HAVE BEEN APPROVED"<<endl;
            return true;
        } else if(choice==2){
            cout<<"THANKYOU FOR USING OUR SERVICES!!"<<endl;
            exit(0);
            return false;
        } else {
            cout<<"THE FOLLOWING REQUEST WAS NOT APPROVED!!"<<endl;
            exit(0);
            return false;
        }
    }
    bool login_data_id(int id){
        for(student_data &t : login){
            if(t.id==id){
                cout<<"ENTERED PIN IS CORRECT!!"<<endl;
                cout<<"WELCOME "<<t.name<<endl;
                cout<<"--------------"<<endl;
                return true;
            }
        }
        cout<<"ENTERED PIN IS INCORRECT, ACCOUNT LOGIN FAILED!!"<<endl;
        exit(0);
        return false;
    }
    void grade_system(int marks){
         if(marks>=91){
            cout<<"THE GRADE ACQUIRED IS: O"<<endl;
        } else if(marks>=81){
            cout<<"THE GRADE ACQUIRED IS: A+"<<endl;
        } else if(marks>=71){
            cout<<"THE GRADE ACQUIRED IS: A"<<endl;
        } else if(marks>=61){
            cout<<"THE GRADE ACQUIRED IS: B+"<<endl;
        } else if(marks>=51){
            cout<<"THE GRADE ACQUIRED IS: B"<<endl;
        } else if(marks>=41){
            cout<<"THE GRADE ACQUIRED IS: C"<<endl;
        } else {
            cout<<"THE SUBJECT COMES UNDER FAIL!!";
        }
    }
    int grade_points(int marks){
        if(marks >= 90) return 10;
        else if(marks >= 80) return 9;
        else if(marks >= 70) return 8;
        else if(marks >= 60) return 7;
        else if(marks >= 50) return 6;
        else if(marks >= 40) return 5;
        else return 0;
    }
    void gpa(){
        // declaring local scope variables
        int arr[50]; // for storing marks
        int arr1[50]; // for storing credit points
        int arr2[50]; // for storing grade points
        int x;
        double weighted_sum = 0;
        int credit_sum = 0;
        double gpa;
        // int credit;
        cout<<"ENTER THE NUMBER OF SUBJECTS: ";
        cin>>x;
        for(int i=0; i<x; i++){
            cout<<"MARKS OF SUBJECT "<<i+1<<" IS: "<<endl;
            cin>>arr[i];
            cout<<"CREDIT POINTS OF SUBJECT "<<i+1<<" IS:"<<endl;
            cin>>arr1[i];
            arr2[i] = grade_points(arr[i]);
            
        }
        cout<<"THE MARKS AND CREDIT PROVIDED ARE AS FOLLOWS: \n"<<endl;
        for(int i=0; i<x; i++){
            cout<<"MARKS OF SUBJECT "<<i+1<<" IS: "<<arr[i]<<endl;
            cout<<"CREDIT POINTS OF SUBJECT "<<i+1<<" IS: "<<arr1[i]<<endl;
            cout<<"GRADE POINTS OF SUBJECT "<<i+1<<" IS: "<<arr2[i]<<endl;
        }
        for(int i=0; i<x; i++){
            weighted_sum += (arr1[i] * arr2[i]);
            credit_sum += (arr1[i]);
        }
         gpa = (weighted_sum) / (credit_sum);
        cout<<"THE GPA FOR THE SEMESTER IS: "<<gpa<<endl;
    }
    void cgpa(){
        // all local scope variables
        int x;
        double arr[50]; // for gpa's for number of semesters
        double weighted_sum = 0;
        double cgpa = 0;
        cout<<"ENTER THE NUMBER OF SEMESTERS: "<<endl;
        cin>>x;
        for(int i=0; i<x; i++){
            cout<<"GPA OF SEMESTER "<<i+1<<" IS: ";
            cin>>arr[i];
            weighted_sum += arr[i];
        }
        cgpa = weighted_sum / x;
        cout<<"THE CGPA CALCULATED IS: "<<cgpa<<endl;
    }

};

int main(){

    // All local scope variables:-
    int choice;
    int pin;
    string student_name;
    int student_id;
    float class_conducted;
    float class_attended;
    int marks;
    // non parametrized constructor call
    college c;
    // adding to the database (vector):-

    c.default_data("Shashwat Bhatt", 1056);
    c.default_data("Atharv Verma", 1060);
    c.default_data("Abhinav", 1073);
    c.default_data("Vibhor", 1093);
    c.default_data("Tanmay Tyagi", 1083);
    c.default_data("Tanishq Pal", 1080);
    c.default_data("Harshit", 1109);
    
    // Main code:-
    cout<<"\n\033[1mWELCOME TO THE COLLEGE - STUDENT WELFARE SYSTEM\033[0m\n";
    cout<<"PLEASE ENTER YOUR FOUR DIGIT PASSCODE: ";
    cin>>pin;
    c.login_data_id(pin);
    while(true){
        cout<<"SELECT THE FUNCTION FROM THE DROP DOWN MENU: \n"<<endl;
    string menu[] = {"1. CALCULATE ATTENDANCE",
             "2. CALCULATE GRADE",
             "3. CALCULATE SEMESTER GPA",
             "4. CALCULATE CGPA",
             "5. SHOW STUDENT DATA",
             "6. ADD A NEW STUDENT",
             "7. REMOVE A STUDENT", 
             "8. EXIT"};

    for(int i=0; i<7; i++){
        cout<<menu[i]<<endl;
    }

    cout<<"PLEASE ENTER YOUR FUNCTION: ";
    cin>>choice;

    switch(choice) {
        case 1: 
        cout<<"PLEASE ENTER THE TOTAL CLASS CONDUCTED: "<<endl;
        cin>>class_conducted;
        cout<<"PLEASE ENTER THE NUMBER OF CLASS ATTENDED: "<<endl;
        cin>>class_attended;
        c.calculate_attendance(class_conducted, class_attended);
        c.function_replay();
        break;
        case 2:
        cout<<"ENTER THE MARKS OF THE SUBJECT: "<<endl;
        cin>>marks;
        c.grade_system(marks);
        c.function_replay();
        break;
        case 3:
        c.gpa();
        c.function_replay();
        break;
        case 4:
        c.cgpa();
        c.function_replay();
        break;
        case 5:
        c.display_all_data();
        c.function_replay();
        break;
        case 6:
        cout<<"ENTER THE NAME OF THE STUDENT: "<<endl;
        cin.ignore();
        getline(cin, student_name);
        cout<<"ENTER THE ID OF THE STUDENT: "<<endl;
        cin>>student_id;
        c.add_data(student_name, student_id);
        c.function_replay();
        break;
        case 7:
        cout<<"ENTER THE STUDENT ID TO BE REMOVED"<<endl;
        cin>>student_id;
        c.remove_data(student_id);
        c.function_replay();
        break;
        case 8:
        exit(0);
    }
}
}