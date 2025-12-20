```cpp
#include <iostream>  
#include <cstring>  
using namespace std;  
struct skill;  
void Delete(skill* ,int );  
struct skill {  
    skill* next;  
    char* name;  
};  
class SkillList {  
private:  
    int maxNum;  
    int counts=0;  
    skill* head; 
    skill* rear; 
public:  
    explicit SkillList(int Maximum) {  
        head = rear = new skill;  
        rear->next=NULL;  
        maxNum = Maximum;    
    }  
    bool add(char* name) {  
        if (counts==maxNum){return false;}  
        else {  
                if (counts==0) {  
	                rear->next=new skill;
                    rear->name=new char[strlen(name)+1];  
                    strcpy(rear->name,name);  
                    counts++;  
                    return true;  
                }  
                if (strcmp(head->name,name)>0) {  
                    skill* p=new skill;  
                    p->next=head;  
                    p->name=new char[strlen(name)+1];  
                    strcpy(p->name,name);  
                    head=p;  
                    counts++;  
                    return true;  
                }  
  
                skill* current=head;  
                while (current->next!=NULL && strcmp(current->next->name,name)<0) {  
                    current=current->next;  
                }  
                skill* p=new skill;  
                p->next=current->next;  
                p->name=new char[strlen(name)+1];  
                strcpy(p->name,name);  
                current->next=p;  
                counts++;  
  
                return true;  
        }  
    }  
    void display() {  
        cout<<"Learned skills: ";  
        if (counts==0) {  
            cout<<"None"<<endl;  
        }  
        else {  
            skill* p=head->next;  
            cout<<head->name;  
            for (int i=1;i<counts;i++) {  
                cout<<", "<<p->name;  
                p=p->next;  
            }  
            cout<<endl;  
        }  
  
    }  
    ~SkillList() {  
	    if (counts！=0) {
	        Delete(head,counts);  
        }
    }  
};  

void Delete(skill* h,int length) {  
    for (int i=1;i<length;i++) {  
        h=h->next;  
    }  
    cout<<h->name;  
    delete []h->name;  
    delete h;  
}
```