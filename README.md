/*
// Sample code to perform I/O:
#include <stdio.h>

int main(){
	int num;
	scanf("%d", &num);              			// Reading input from STDIN
	printf("Input number is %d.\n", num);       // Writing output to STDOUT
}

// Warning: Printing unwanted or ill-formatted data to output will cause the test cases to fail
*/

// Write your code here
#include <iostream>
#include <stdio.h>
using namespace std;

struct node
{
    int keyl;int keyh;int keym;
    struct node *left; struct node *right;
    struct node *parent;
};
struct tree
{
    struct node *root;
};
void prin(struct node * i){
    if(i->left!=NULL){
        prin(i->left);
    }
    printf("%d %d %d\n",i->keyl,i->keyh,i->keym);
    if(i->right!=NULL){
        prin(i->right);
    }
}
void insertl(struct tree *T,struct node *i){
    struct node *z;
    z=T->root;
    struct node *y=NULL;
    while(z!=NULL){
        y=z;
        if(z->keyl>i->keyl){
            if(z->keym<i->keyh){
                z->keym=i->keyh;// change
            }
            z=z->left; 
        }
        else{
            if(z->keym<i->keyh){
                z->keym=i->keyh;
            }
            z=z->right;    
        }
    }
    i->parent=y;// change
    //printf("%d\n",y);
    if(y==NULL){
        T->root=i;
        //printf("%d\n",i->keyh);
    }
    else if(y->keyl>i->keyl){
        if(y->keym<i->keyh){
            y->keym=i->keyh;//change
        }
        y->left=i;//change
    }
    else{
        if(y->keym<i->keyh){
            y->keym=i->keyh;//change//
        }
        //printf("check\n");
        y->right=i;    //change
        //printf("%d\n",T->root->right->keyh);
    }
} //i not NULL
void inserth(struct tree *T,struct node *i){
    struct node *z;
    z=T->root;
    struct node *y=NULL;
    while(z!=NULL){
        y=z;
        if(z->keyh>i->keyh){
            if(z->keym<i->keyh){
                z->keym=i->keyh;
            }
            z=z->left; 
        }
        else{
            if(z->keym<i->keyh){
                z->keym=i->keyh;
            }
            z=z->right;    
        }
    }
    i->parent=y;
    if(y==NULL){
        T->root=i;
    }
    else if(y->keyh>i->keyh){
        if(y->keym<i->keyh){
            y->keym=i->keyh;
        }
        y->left=i;
    }
    else{
        if(y->keym<i->keyh){
            y->keym=i->keyh;
        }
        y->right=i;
    }
}
struct node *treeminimum(struct node *i){
    struct node *z;
    z=i;
    if(z!=NULL){
        while(z->left!=NULL){
            z=z->left; 
        }
    }
    return z;//change
}
struct node *treemaximum(struct node *i){
    struct node *z;
    z=i;
    if(z!=NULL){
        while(z->right!=NULL){
            z=z->right; 
        }
    }
    return z;
}
struct node *treesucc(struct tree *T,struct node *i){
    if(i!=NULL){    
        if(i->right!=NULL){
            return treeminimum(i->right);
        }
        struct node *y=i->parent;
        while(y!=NULL&&i==y->right){
            i=i->parent;
            y=y->parent;
        }
        return y;
    }
    return NULL;
} // i not NULL
void transplant(struct tree *T,struct node *u,struct node *v){
    if(u->parent==NULL){
        T->root=v;
    }
    else if(u->parent->left==u){
        u->parent->left=v;
    }
    else{
        u->parent->right=v;
    }
    if(v!=NULL){
        v->parent=u->parent;
    }
}  // u not NULL
int max(struct node* a,int b,struct node* c){
    int max;
    if(a!=NULL&&a->keym>b){
        max=a->keym;
    }
    else{
        max=b;
    }
    if(c!=NULL&&max<c->keym){
        max=c->keym;
    }
    return max;
} 
// int sibling(struct tree T,struct node i){
//     if(i.parent.left==i){
//         return i.parent.right
//     }
//     else{
//         return i.parent.left
//     }
// }// i not null
void delte(struct tree *T,struct node *i){
    if(i->right==NULL){
        struct node *z=i->parent;    // can be NULL
        transplant(T,i,i->left);
        //sib=sibling(T,i); // can be NULL
        while(z!=NULL){//&&z->keym!=max(z->left->keym,z->keyh,z->right->keym)
            z->keym=max(z->left,z->keyh,z->right);
            //i=i.parent;
            z=z->parent;
        }
    }
    else if(i->left==NULL){
        struct node *z=i->parent;
        transplant(T,i,i->right);
         //can be NULL
        //sib=sibling(T,i); // can be NULL
        while(z!=NULL){
            z->keym=max(z->left,z->keyh,z->right);
            //i=i.parent;
            z=z->parent;
        }
    }
    else{
        //struct tree *T_1;
        //T_1->root=i->right;
        struct node *y=treeminimum(i->right);
        i->keyl=y->keyl;i->keyh=y->keyh;
        struct node *z=y->parent;
        transplant(T,y,y->right);
        //i->keym=y->keym;
        //sib=sibling(T,y);
        while(z!=NULL){
            z->keym=max(z->left,z->keyh,z->right);
            //i=i.parent;
            z=z->parent;
        }
    }
}
int isoverlap(struct tree *T,int low, int high){
    struct node *z=T->root;
    while(z!=NULL&&(z->keyl>high||z->keyh<low)){
        if(z->left!=NULL&&z->left->keym>low){
            z=z->left;
        }
        else{
            z=z->right;
        }
    }
    if(z==NULL){
        return 0;
    }
    return 1;
}
struct node *searchl(struct tree *T,int low,int high){
    //struct node *i;
    //i->keyl=low;
    struct node *z;
    z=T->root;
    struct node *y=NULL;
    while(z!=NULL){
        y=z;
        if(z->keyl==low){
            //if(z->keyh==high){
                return z;//change
            //}
            //else{
               // return NULL;
            //}
        }
        else if(z->keyl>low){
            z=z->left; 
        }
        else{
            z=z->right;    
        }
    }
    return NULL;
}
struct node *searchh(struct tree *T,int low,int high){
    //struct node *i;
    //i->keyh=high;
    struct node *z;
    z=T->root;
    struct node *y=NULL;
    while(z!=NULL){
        y=z;
        if(z->keyh==high){
            //if(z->keyl==low){
                return z;//change
            //}
            //else{
                //return NULL;
            //}
        }
        else if(z->keyh>high){
            z=z->left; 
        }
        else{
            z=z->right;    
        }
    }
    return NULL;
}
int main(){
    int cases;
    scanf("%d\n",&cases);
    //printf("%d ",cases);
    for(int j=0;j<cases;j++){
        struct tree* T=(struct tree *) malloc(sizeof(struct tree ));
        struct tree* T1=(struct tree *) malloc(sizeof(struct tree ));
        //struct node *head= (struct node *) malloc(sizeof(struct node ));
        //struct node *head1= (struct node *) malloc(sizeof(struct node ));
        //struct node *head;
        //struct node *head1;
        T->root=NULL;
        T1->root=NULL;
        //head=NULL;
        //head->left=NULL;
        //head->right=NULL;
        //head->parent=NULL;
        //head1=NULL;
        //head1->left=NULL;
        //head1->right=NULL;
        //head1->parent=NULL;
        int len;
        scanf("%d\n",&len);
        //printf("%d\n",len);
        for(int i=0;i<len;i++){
            char c1;
            char c2;
            scanf("%c%c",&c1,&c2);
            //printf("%c%c\n",c1,c2);
            if(c1=='+'){
                //printf("check\n");
                int low;int high;
                scanf("%d %d\n",&low,&high); 
                //printf("%d\n",low);
                if(low<high){
                    struct node *insp= (struct node *) malloc(sizeof(struct node ));
                    insp->left=NULL;
                    insp->right=NULL;
                    insp->parent=NULL;
                    //struct node ins= (struct node ) malloc(sizeof(struct node ));
                    struct node *insp1= (struct node *) malloc(sizeof(struct node ));
                    insp1->left=NULL;
                    insp1->right=NULL;
                    insp1->parent=NULL;
                    //struct node ins1= (struct node ) malloc(sizeof(struct node ));
                    //*insp=ins;*insp1=ins1;
                    insp->keyl=low;insp->keyh=high;insp->keym=high;
                    insp1->keyl=low;insp1->keyh=high;insp1->keym=high;
                    insertl(T,insp);
                    inserth(T1,insp1);
                }
                //printf("%d\n",T->root->right->keyh);
                //printf("%d\n",T->root->right->keyh);
            }
            else if(c1=='-'){
                int low;int high;
                scanf("%d %d\n",&low,&high); 
                struct node *del;
                struct node *del1;
                del=NULL;del1=NULL;
                del=searchl(T,low,high);
                del1=searchh(T1,low,high);
                if(del!=NULL&&low<high){
                    delte(T,del);
                    delte(T1,del1);  
                }
            }
            else if(c1=='m'&&c2=='i'){
                char un;
                scanf("%c\n",&un);
                struct node *z=NULL;
                z=treeminimum(T->root);
                if(z==NULL){
                    printf("INVALID\n");
                }
                else{
                    printf("[%d %d]\n",z->keyl,z->keyh);
                }
            }
            else if(c1=='m'&&c2=='a'){
                char un;
                scanf("%c\n",&un);
                struct node *z=NULL;
                z=treemaximum(T1->root);
                if(z==NULL){
                    printf("INVALID\n");
                }
                else{
                    printf("[%d %d]\n",z->keyl,z->keyh);
                }
            }
            else if(c1=='l'){
                char un;char un1;char un2;
                int low;int high;
                scanf("%c%c%c %d %d\n",&un,&un1,&un2,&low,&high);
                //printf("%c%c%c %d %d",un,un1,un2,low,high);
                struct node *z=NULL;
                z=searchl(T,low,high);
                // if(z==NULL){
                //     printf("check");
                // }
                struct node *m=NULL;
                if(low>=high){
                    printf("INVALID\n");
                }
                else if(z!=NULL){
                    m=treesucc(T,z);
                    if(m==NULL){
                        printf("INVALID\n");
                    }
                    else{
                        printf("[%d %d]\n",m->keyl,m->keyh);
                    }
                }
                else{
                    printf("INVALID\n");
                }
            }
            else if(c1=='h'){
                char un;char un1;char un2;
                int low;int high;
                scanf("%c%c%c %d %d\n",&un,&un1,&un2,&low,&high);
                struct node *z=NULL;
                z=searchh(T1,low,high);
                if(low>=high){
                    printf("INVALID\n");
                }
                else if(z!=NULL){
                    struct node *m=NULL;
                    m=treesucc(T1,z);
                    if(m==NULL){
                        printf("INVALID\n");
                    }
                    else{
                        printf("[%d %d]\n",m->keyl,m->keyh);
                    }
                }
                else{
                    printf("INVALID\n");
                }
            }
            else if(c1=='o'){
                char un;char un1;char un2;char un3;char un4;
                int low;int high;
                scanf("%c%c%c%c%c %d %d\n",&un,&un1,&un2,&un3,&un4,&low,&high);
                //struct node *z=NULL;
                //z=searchl(T,low);
                //printf("%d\n",z->keyh)
                if(low>=high){
                    printf("INVALID\n");
                }
                else{
                    int isov;
                    isov=isoverlap(T,low,high);
                    printf("%d\n",isov);
                }
            }
        }
        //prin(T->root);
        //printf("%d",max(NULL,2,NULL));
    }
    return 0;
}


