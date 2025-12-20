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
        cout<<"Construct SkillList: None"<<endl;  
    }  
    bool add(char* name) {  
        if (counts==maxNum){return false;}  
        else {  
                if (counts==0) {  
                    head->name=new char[strlen(name)+1];  
                    strcpy(head->name,name);  
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
        cout<<"Destruct skills: ";  
        if (counts==0) {  
            cout<<"None"<<endl;  
        }else {  
            Delete(head,counts);  
            while (counts>1) {  
                counts--;  
                cout<<", ";  
                Delete(head,counts);  
            }  
            cout<<endl;  
        }  
    }  
};  
int main() {  
    int maxNum;  
    cout << "Max number of skills: ";  
    cin >> maxNum; cin.get();  
    SkillList *pMember = new SkillList (maxNum);  
    while (true) {  
        char *skillName = new char[20];  
        cout << "Learn a new skill: ";  
        cin.getline(skillName, 20);  
        if (!pMember->add(skillName)) {  
            delete[] skillName;  
            break;  
        }  
    }  
    if (maxNum > 0) pMember->display();  
    delete pMember;  
    return 0;  
}  
void Delete(skill* h,int length) {  
    for (int i=1;i<length;i++) {  
        h=h->next;  
    }  
    cout<<h->name;  
    delete []h->name;  
    delete h;  
}
```