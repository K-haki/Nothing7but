```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long int ll;
char ex='0'; //用于补全不够位数的二进制数
string ip_rule="";//用于存储转换二进制后的规则IP网络前缀
void ip_chansform(int a);//转换规则IP，返回前缀长度
// void chansform(int c,char *a); //函数1：IP地址十进制表示转换成点分十进制表示(参数1：IP十进制 ， 参数2：存储转换后的点分十进制IP地址的字符数组地址)
int ip_check(ll a, int b,int c); //函数2：检测IP地址是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int port_check(int p, int b, int c); //函数3：检测端口是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int tcp_check(int t, int b); //函数4：检测传输协议是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int stot(char *p,float length);//十六进制转换十进制
ll my_strtol(string *a);//二进制转十进制

struct rule{
    char rule_tcp[10];
    int rule_ip1[10],rule_ip2[10];
    int port_1_l,port_1_r,port_2_l,port_2_r;
};

rule r[1000];

int main(int argc, char** argv){
   clock_t start,finish;
    start=clock();
    fpos_t pos_1=0,pos_2=0;
    char waste;
    freopen("out.txt","w",stdout);
    int file,i,j,k;
    int check_1,check_2,check_3,check_4,check_5,port_1,port_2,tcp;
    ll ip_1,ip_2;
//    cin.clear();
    freopen(argv[1],"r",stdin);//读入规则集存储于struct rule中
    for(i=0;i<918;i++){
        for(j=0;j<5;j++){
            cin>>waste>>r[i].rule_ip1[j];
    //        cout<<r[i].rule_ip1[j]<<" ";
        }
        cin>>r[i].rule_ip2[0];
        for(j=1;j<5;j++){
            cin>>waste>>r[i].rule_ip2[j];
    //        cout<<r[i].rule_ip2[j]<<" ";
        }
        cin>>r[i].port_1_l>>waste>>r[i].port_1_r>>r[i].port_2_l>>waste>>r[i].port_2_r>>waste;
        cin>>waste>>r[i].rule_tcp;
    //    cout<<r[i].port_1_l<<" "<<r[i].port_1_r<<" "<<r[i].port_2_l<<" "<<r[i].port_2_r<<" "<<r[i].rule_tcp<<endl;
    }
    for(file=2;file<argc;file++){
        FILE *p=freopen(argv[file],"r",stdin);
        while(cin>>ip_1,cin>>ip_2,cin>>port_1,cin>>port_2,cin>>tcp){
            int cnt=0;
        //    fgetpos(p,&pos_1);    
            while(cnt<918){
                check_1=ip_check(ip_1,cnt,1);
                check_2=ip_check(ip_2,cnt,2);
                check_3=port_check(port_1,cnt,1);
                check_4=port_check(port_2,cnt,2);
                check_5=tcp_check(tcp,cnt);
                if(check_1&&check_2&&check_3&&check_4&&check_5) break; 
                else{
                    cnt++;
                }
                check_1=check_2=check_3=check_4=check_5=0;
            }
            if(cnt==918) cout<<"-1"<<endl;
            else cout<<cnt<<endl;
        //    cin.clear();
        //    freopen(argv[file],"r",stdin);
        //    fsetpos(p,&pos_1);
        }
        finish=clock();
        cout<<"程序执行时间为："<<(double)(finish-start)/CLOCKS_PER_SEC<<endl;
       
    }
    return 0;
} 

void ip_chansform(int a){//只读一条规则
    char s[10];
    int i,j;
    ltoa(a,s,2);
    if(strlen(s)<8){
        for(j=0;j<8-strlen(s);j++){
            ip_rule+=ex;
        }
    }
    ip_rule+=s;
    return ;
}

int ip_check(ll a, int b, int c){
    ip_rule="";
    ll t,min,max,i;
    if(c==1){
        for(i=0;i<4;i++){
            ip_chansform(r[b].rule_ip1[i]);
        }
        t=r[b].rule_ip1[4];
    }else{
        for(i=0;i<4;i++){
            ip_chansform(r[b].rule_ip2[i]);
        }
        t=r[b].rule_ip2[4];
    }
    string ip_min=ip_rule,ip_max=ip_rule;
    for(i=t;i<ip_rule.length();i++){
        ip_min[i]='0';
        ip_max[i]='1';
    }
    min=my_strtol(&ip_min);
    max=my_strtol(&ip_max);
    if(a>=min&&a<=max){
        return 1;
    }else return 0;
}

int port_check(int p, int b, int c){
    if(c==1){
        if(p>=r[b].port_1_l&&p<=r[b].port_1_r) return 1;
        else return 0;
    }else{
        if(p>=r[b].port_2_l&&p<=r[b].port_2_r) return 1;
        else return 0;
    }
    
}

int tcp_check(int t, int b){
    char *p=r[b].rule_tcp;
    int i,x;
    if(r[b].rule_tcp[5]=='0'&&r[b].rule_tcp[6]=='0') return 1;
    x=stot(p,2);
    if(t==x) return 1;
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
    return sum;
}

ll my_strtol(string *a){
    ll sum=0;
    float i;
    for(i=((*a).length()-1);i>=0;i--){
        if((*a)[i]=='1')
        sum+=pow(2,31-i);
    }
    return sum;
}
```