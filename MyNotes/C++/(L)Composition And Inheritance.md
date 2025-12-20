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
public:  
    explicit SkillList(int Maximum) {  
        head = new skill;  
        head->next=NULL;  
        maxNum = Maximum;    
    }  
    bool add(char* name) {  
        if (counts==maxNum){return false;}  
        else {  
            skill* current=head;  
            while (current->next!=NULL && strcmp(current->next->name,name)<0) {  
                    current=current->next;  
                }  
            skill* newNode=new skill;  
            newNode->next=current->next;  
            newNode->name=new char[strlen(name)+1];  
            strcpy(newNode->name,name);  
            current->next=newNode;  
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