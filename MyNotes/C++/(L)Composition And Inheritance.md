```cpp
#include <iostream>  
#include <cstring>  
using namespace std;  
struct skill;  
void DeleteList (skill*&);  
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
        if (counts==0) {  
            cout<<"None"<<endl;  
        }  
        else {  
            skill* p=head->next;  
            cout<<p->name;  
            while(p->next!=NULL) { 
	            p=p->next; 
                cout<<", "<<p->name;  
            }  
            cout<<endl;  
        }  
  
    }  
    ~SkillList() {  
		 DeleteList(head);  
    }  
};  

void DeleteList(skill*& h) {  
    if (!h){return;}
    if (!h->next){delete h;h=nullptr;}
    DeleteList(h->next);
}
```