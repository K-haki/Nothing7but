```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long int ll;
char ex='0'; //用于补全不够位数的二进制数
string ip_rule="";//用于存储转换二进制后的规则IP网络前缀
int ip_chansform(void);//转换规则IP，返回前缀长度
// void chansform(int c,char *a); //函数1：IP地址十进制表示转换成点分十进制表示(参数1：IP十进制 ， 参数2：存储转换后的点分十进制IP地址的字符数组地址)
int ip_check(char a[]); //函数2：检测IP地址是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int port_check(int p); //函数3：检测端口是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int tcp_check(int t); //函数4：检测传输协议是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int judge(int a, int b, int c, int d, int e); //遍历规则集，以五元组作为参数传入，调用函数2/3/4并判断其返回是否都为1，
                               //若是，则返回规则编号；若否，则返回-2，继续循环读入下一条rule，再次调用函数2/3/4继续判断；
                               //check_1的值为-1,表示此时编号已经为规则集内最大，则返回-1
int stot(char *p,float length);




/*int judge(int a, int b, int c, int d, int e){
    int cnt=0;             //cnt用于记录规则编号；
    if(a==-1) return -1;  //a的值为-1,表示此时编号已经为规则集内最大，则返回-1
    else if(a==b&&b==c&&c==d&&d==e&&a==1){
        return cnt;
    }else{
        cnt++;
        return -2;
    }
}*/

int main(){
//    ios::sync_with_stdio(0);
//    cin.tie(0); cout.tie(0);
//    cout.flush();
    fpos_t pos_1=0,pos_2=0;
    FILE *p=freopen("input.txt","r",stdin);
    freopen("out.txt","w",stdout);
    char cip_1[35],cip_2[35],waste;//cip_1[]和cip_2[]用于存储转换后的IP地址
    ll ip_1,ip_2,ans,i,j,port_1,port_2,tcp,k=0;
    int check_1,check_2,check_3,check_4,check_5;
    while(cin>>ip_1,cin>>ip_2,cin>>port_1,cin>>port_2,cin>>tcp){
//        cout<<ip_1<<" "<<ip_2<<" "<<port_1<<" "<<port_2<<" "<<tcp<<endl;
        int f=0,cnt=0,l1,l2;//  f标记是否遍历规则集仍无法匹配而直接进入下一条数据的匹配；
        ltoa(ip_1,cip_1,2);
        ltoa(ip_2,cip_2,2);
        l1=strlen(cip_1);
        l2=strlen(cip_2);
        if(strlen(cip_1)<32){
            for(j=l1-1;j>=0;j--){
                cip_1[j+32-l1]=cip_1[j];
            }
            for(j=0;j<=31-l1;j++){
                cip_1[j]='0';
            }
            cip_1[32]='\0';
        }
        if(strlen(cip_2)<32){
            for(j=l2-1;j>=0;j--){
                cip_2[j+32-l2]=cip_2[j];
            }
            for(j=0;j<=31-l2;j++){
                cip_2[j]='0';
            }
            cip_2[32]='\0';
        }
//        cout<<cip_1<<" "<<cip_2<<endl;
        fgetpos(p,&pos_1);
        cin.clear();
        FILE *p2=freopen("rule1.txt","r",stdin);
//        rewind(p);
        while(cin>>waste){
            check_1=ip_check(cip_1);
            check_2=ip_check(cip_2);
            check_3=port_check(port_1);
            check_4=port_check(port_2);
            check_5=tcp_check(tcp);
        //    cout<<check_1<<" "<<check_2<<endl;
            if(check_1&&check_2&&check_3&&check_4&&check_5) break; 
            else if(check_1==-1||check_2==-1){
                f=1;
                break;
            }
            else{
                cnt++;
            }
        }
        if(f) cout<<"-1"<<endl;
        else if(cnt==918) cout<<"-1"<<endl;
        else cout<<cnt<<endl;
    /*  if(cin.rdstate()!=ios_base::eofbit){
            cout<<"end"<<endl;
            return 0;
        }*/
        cin.clear();
        freopen("input.txt","r",stdin);
        fsetpos(p,&pos_1);
        k++;
    }
    if(k==3) return 0;
    return 0;
} 

int ip_chansform(void){//只读一条规则
    int a[6],step,i=0,j;
    char x,s[10];
//    getc(p);
//    if(feof(p)) return -1;
    while(cin>>a[i]>>x){
//        cout<<"\n"<<"a["<<i<<"]="<<a[i]<<" x="<<x<<endl;
        ltoa(a[i],s,2);
        if(strlen(s)<8){
            for(j=0;j<8-strlen(s);j++){
                ip_rule+=ex;
            }
        }
//        cout<<s<<endl;
        ip_rule+=s;
        if(i>=3) break;
        i++;
    }
    if(i<3) return -1; 
    cin>>step;
    return step;
}

int ip_check(char a[]){
    ip_rule="";
    int t=ip_chansform(),i=0;
//    cout<<t<<endl;
//    cout<<ip_rule<<" ";
    if(t==-1) return -1;
    for(i=0;i<t;i++){
        if(a[i]!=ip_rule[i]) break;
    }
    if(i==t) return 1;
    else return 0;
}

int port_check(int p){
    int l,r;
    char x;
    cin>>l>>x>>r;
//    cout<<"l="<<l<<"r="<<r<<endl;
    if(p>=l&&p<=r) return 1;
    else return 0;
}

int tcp_check(int t){
    char l[5],r[5],x,y;
    int i,b=0,e=0;
    cin>>x>>y>>l[0]>>l[1];
//    cout<<l[0]<<l[1]<<endl;
    cin>>x;
    cin>>x>>y>>r[0]>>r[1];
//    cout<<r[0]<<r[1]<<endl;
    if(r[0]=='0'&&r[1]=='0') return 1;
    b=stot(l,2);
//    e=stot(r,2);
    if(t==b) return 1;
    else return 0;
}

int stot(char *p,float length){
    int sum=0,i,t=length;
    for(i=0;i<t;i++){
        if(p[i]>='0'&&p[i]<='9'){
            sum+=(p[i]-'0')*pow(16,--length);
        }else if(p[i]>='A'&&p[i]<='F'){
            sum+=(p[i]-'A'+10)*pow(16,--length);
        }
    }
//    cout<<sum<<endl;
    return sum;
}
```